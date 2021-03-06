---
layout: post
title: Agda 中的证明，从一点五到二
category: Agda
tags: Agda, Proof
keywords: Agda, Proof
description: Proof in Agda, from 1 to 2
inline_latex: true
agda: true
---

上一篇说了很多只有一种情况的证明，这一篇说个有两种情况的。
到目前为止，按理说所有的字符都还能正常显示。

## 前置知识

+ [上一篇文章](../../../../2017/11/02/ProofInAgda2/)

以及，由于 Agda 语言的特殊性，本文将继续使用 LaTeX 和代码块来共同展示代码。
代码块唯一的作用在于便于复制，主要的呈现途径为 LaTeX 。

### 上一篇的习题

上一篇文章我留下了一个没提供证明的命题，现在给出完整答案：

$$
\DeclareMathOperator{Set}{Set}
\DeclareMathOperator{where}{where}
\DeclareMathOperator{proof}{proof}
\DeclareMathOperator{data}{data}
\DeclareMathOperator{pr}{pr}
\DeclareMathOperator{qr}{qr}
\DeclareMathOperator{intro}{intro}
\DeclareMathOperator{comm}{comm}
\DeclareMathOperator{assoc}{assoc}
\DeclareMathOperator{elim}{elim}

\begin{align*}
& {\land}{-}{\assoc}_0 : \forall \{P\ Q\ R\} \rightarrow ((P \land Q) \land R) \rightarrow (P \land (Q \land R)) \\
& {\land}{-}{\assoc}_0 \ ({\land}{-}{\intro}\ ({\land}{-}{\intro}\ p\ q)\ r) = {\land}{-}{\intro}\ p\ ({\land}{-}{\intro}\ q\ r) \\
& \\
& {\land}{-}{\assoc}_1 : \forall \{P\ Q\ R\} \rightarrow (P \land (Q \land R)) \rightarrow ((P \land Q) \land R) \\
& {\land}{-}{\assoc}_1 \ ({\land}{-}{\intro}\ p\ ({\land}{-}{\intro}\ q\ r)) = {\land}{-}{\intro}\ ({\land}{-}{\intro}\ p\ q)\ r \\
& \\
& {\land}{-}{\assoc} : \forall \{P\ Q\ R\} \rightarrow (P \land (Q \land R)) \Leftrightarrow ((P \land Q) \land R) \\
& {\land}{-}{\assoc} ={\land}{-}{\intro} \  {\land}{-}{\assoc}_1 \  {\land}{-}{\assoc}_0
\end{align*}
$$

```agda
∧-assoc₀ : ∀ {P Q R} → ((P ∧ Q) ∧ R) → (P ∧ (Q ∧ R))
∧-assoc₀ (∧-intro (∧-intro p q) r) = ∧-intro p (∧-intro q r)

∧-assoc₁ : ∀ {P Q R} → (P ∧ (Q ∧ R)) → ((P ∧ Q) ∧ R)
∧-assoc₁ (∧-intro p (∧-intro q r)) = ∧-intro (∧-intro p q) r

∧-assoc : ∀ {P Q R} → (P ∧ (Q ∧ R)) ⇔ ((P ∧ Q) ∧ R)
∧-assoc = ∧-intro ∧-assoc₁ ∧-assoc₀
```

确实没什么好说的，所以才能说是即得易见平凡，仿照上例显然。

## 或相关的证明

上一篇我有个东西没讲完，就是 "或" 。
它和 "与" 相对，它只要求两个命题中的一个成立。

因此，它对应着两个不同的情况：

$$
p \rightarrow (p \lor q) \\
q \rightarrow (p \lor q)
$$

### 定义 GADT

把这个关系写成 GADT ，就是这样：

$$
\begin{align*}
& \data\ \_{\lor}\_\ (P\ Q : \Set) : \Set \where \\
& \ \ {\lor}{-}{\intro}_0 : P \rightarrow P \lor Q \\
& \ \ {\lor}{-}{\intro}_1 : Q \rightarrow P \lor Q
\end{align*}
$$

```agda
data _∨_ (P Q : Set) : Set where
  ∨-intro₀ : P → P ∨ Q
  ∨-intro₁ : Q → P ∨ Q
```

这里我们遇到了一种和之前不一样的情况：
我们的 GADT 有了两种 instance 。
这意味着我们需要在证明的时候考虑两种不同的情况，分别针对这两种 instance 。

### 证明一

比如，我们可以证明一下这个命题：

$$
(p \rightarrow r) \land (q \rightarrow r) \land (p \lor q) \rightarrow r
$$

它的逻辑很简单，在 `p` 和 `q` 都能推出 `r` 的时候， `p` `q` 只需要成立一个， `r` 就成立。
这个命题写成 Agda 的类型，就是：

$$
{\lor}{-}{\elim} : \forall \{P\ Q\} \{R : \Set\} \rightarrow (P \rightarrow R) \rightarrow (Q \rightarrow R)
\rightarrow (P \lor Q) \rightarrow R
$$

```agda
∨-elim : ∀ {P Q} {R : Set} → (P → R) → (Q → R) → (P ∨ Q) → R
```

我们在证明中，需要同时对 $ (P \lor Q) $ 的两种可能的情况进行处理
（因为这个类型的东西既可以是通过 `P` 构造的，也可以是通过 `Q` 构造的），
不然 Agda 的 exhaustiveness check 会报错的
（这也是为什么 `postulate` 不被推荐使用）。

首先考虑 `P` 成立的情况，我们有：

$$
{\lor}{-}{\elim}\pr \_ \ ({\lor}{-}{\elim}_0 \ p) = \pr p
$$

```agda
∨-elim pr _ (∨-intro₀ p) = pr p
```

然后考虑 `Q` 成立的情况，我们有：

$$
{\lor}{-}{\elim}\ \_ \qr \ ({\lor}{-}{\elim}_1 \ q) = \qr q
$$

```agda
∨-elim _ qr (∨-intro₁ q) = qr q
```

放在一起，就是：

$$
\begin{align*}
& {\lor}{-}{\elim} : \forall \{P\ Q\} \{R : \Set\} \rightarrow (P \rightarrow R) \rightarrow (Q \rightarrow R)
\rightarrow (P \lor Q) \rightarrow R \\
& {\lor}{-}{\elim}\pr \_ \ ({\lor}{-}{\elim}_0 \ p) = \pr p \\
& {\lor}{-}{\elim}\ \_ \qr \ ({\lor}{-}{\elim}_1 \ q) = \qr q
\end{align*}
$$

```agda
∨-elim : ∀ {P Q} {R : Set} → (P → R) → (Q → R) → (P ∨ Q) → R
∨-elim pr _ (∨-intro₀ p) = pr p
∨-elim _ qr (∨-intro₁ q) = qr q
```

这样，就 check 了。
十分简单。

### 证明二

和 $ \land $ 一样， $ \lor $ 也有交换律：

$$
\begin{align*}
& {\lor}{-}{\comm}' : \forall \{P\ Q\} \rightarrow (P \lor Q) \rightarrow (Q \lor R) \\
& {\lor}{-}{\comm}' \ ({\lor}{-}{\intro}_0 \ p) = {\lor}{-}{\intro}_1 \ p \\
& {\lor}{-}{\comm}' \ ({\lor}{-}{\intro}_1 \ q) = {\lor}{-}{\intro}_0 \ q \\
\\
& {\lor}{-}{\comm} : \forall \{P\ Q\} \rightarrow (P \lor Q) \Leftrightarrow (Q \lor R) \\
& {\lor}{-}{\comm} = {\land}{-}{\intro} \ {\lor}{-}{\comm}' \ {\lor}{-}{\comm}'
\end{align*}
$$

```agda
∨-comm′ : ∀ {P Q} → (P ∨ Q) → (Q ∨ P)
∨-comm′ (∨-intro₀ p) = ∨-intro₁ p
∨-comm′ (∨-intro₁ q) = ∨-intro₀ q

∨-comm : ∀ {P Q} → (P ∨ Q) ⇔ (Q ∨ P)
∨-comm = ∧-intro ∨-comm′ ∨-comm′
```

## 结束

这么快就没了？

其实只是填一下上一篇留下的坑。

是的，我说完了。
