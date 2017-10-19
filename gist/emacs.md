---
layout: gist
title: 我的 Emacs 配置
---

```elisp
(setq tab-width 2)
(setq default-tab-width 2)
(setq kotlin-tab-width 2)
(setq java-tab-width 2)

(require 'package)
(setq package-archives
      '(("gnu"   . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/")
        ("melpa" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/")))

(package-initialize)
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(ac-auto-show-menu 0.1)
 '(ac-modes
   (quote
    (emacs-lisp-mode lisp-mode lisp-interaction-mode slime-repl-mode nim-mode c-mode cc-mode c++-mode objc-mode swift-mode go-mode java-mode malabar-mode clojure-mode clojurescript-mode scala-mode scheme-mode ocaml-mode tuareg-mode coq-mode haskell-mode agda-mode agda2-mode perl-mode cperl-mode python-mode ruby-mode lua-mode tcl-mode ecmascript-mode javascript-mode js-mode js-jsx-mode js2-mode js2-jsx-mode coffee-mode php-mode css-mode scss-mode less-css-mode elixir-mode makefile-mode sh-mode fortran-mode f90-mode ada-mode xml-mode sgml-mode web-mode ts-mode sclang-mode verilog-mode qml-mode apples-mode markdown-mode dart-mode shell-mode kotlin-mode haskell-mode fundamental-mode rust-mode yaml-mode csharp-mode latex-mode llvm-mode nxml-mode idris-mode makefile-mode)))
 '(blink-cursor-mode t)
 '(column-number-mode t)
 '(custom-enabled-themes nil)
 '(flyspell-abbrev-p t)
 '(flyspell-after-incorrect-word-string nil)
 '(font-use-system-font t)
 '(global-auto-complete-mode t)
 '(global-linum-mode t)
 '(idris-interpreter-path "/home/ice1000/.cabal/bin/idris")
 '(ispell-program-name "aspell")
 '(package-selected-packages
   (quote
    (latex-extra auctex yaml-mode zone-select
    zone-rainbow zone-nyan scala-mode sbt-mode
    rust-mode ruby-test-mode mode-icons
    markdown-preview-mode markdown-mode+ llvm-mode
    kotlin-mode jekyll-modes j-mode indent-guide
    idris-mode ibuffer-git haskell-mode groovy-mode
    go-mode gited elm-mode dart-mode d-mode
    csharp-mode bing-dict ace-flyspell ac-c-headers)))
 '(show-paren-mode t)
 '(size-indication-mode t))

;;; Fira code
;; This works when using emacs --daemon + emacsclient
(add-hook 'after-make-frame-functions
          (lambda (frame)
            (set-fontset-font t '(#Xe100 . #Xe16f) "Fira Code Symbol")))
;; This works when using emacs without server/client
(set-fontset-font t '(#Xe100 . #Xe16f) "Fira Code Symbol")
;; I haven't found one statement that makes both of the above situations work, so I use both for now

(when (window-system)
  (set-default-font "Fira Code"))

(defconst fira-code-font-lock-keywords-alist
  (mapcar
   (lambda (regex-char-pair)
     `(,(car regex-char-pair)
       (0 (prog1 ()
            (compose-region (match-beginning 1)
                            (match-end 1)
                            ;; The first argument to concat is a string containing a literal tab
                            ,(concat "  " (list (decode-char 'ucs (cadr regex-char-pair)))))))))
   '(("\\(www\\)"                   #Xe100)
     ("[^/]\\(\\*\\*\\)[^/]"        #Xe101)
     ("\\(\\*\\*\\*\\)"             #Xe102)
     ("\\(\\*\\*/\\)"               #Xe103)
     ("\\(\\*>\\)"                  #Xe104)
     ("[^*]\\(\\*/\\)"              #Xe105)
     ("\\(\\\\\\\\\\)"              #Xe106)
     ("\\(\\\\\\\\\\\\\\)"          #Xe107)
     ("\\({-\\)"                    #Xe108)
     ("\\(\\[\\]\\)"                #Xe109)
     ("\\(::\\)"                    #Xe10a)
     ("\\(:::\\)"                   #Xe10b)
     ("[^=]\\(:=\\)"                #Xe10c)
     ("\\(!!\\)"                    #Xe10d)
     ("\\(!=\\)"                    #Xe10e)
     ("\\(!==\\)"                   #Xe10f)
     ("\\(-}\\)"                    #Xe110)
     ("\\(--\\)"                    #Xe111)
     ("\\(---\\)"                   #Xe112)
     ("\\(-->\\)"                   #Xe113)
     ("[^-]\\(->\\)"                #Xe114)
     ("\\(->>\\)"                   #Xe115)
     ("\\(-<\\)"                    #Xe116)
     ("\\(-<<\\)"                   #Xe117)
     ("\\(-~\\)"                    #Xe118)
     ("\\(#{\\)"                    #Xe119)
     ("\\(#\\[\\)"                  #Xe11a)
     ("\\(##\\)"                    #Xe11b)
     ("\\(###\\)"                   #Xe11c)
     ("\\(####\\)"                  #Xe11d)
     ("\\(#(\\)"                    #Xe11e)
     ("\\(#\\?\\)"                  #Xe11f)
     ("\\(#_\\)"                    #Xe120)
     ("\\(#_(\\)"                   #Xe121)
     ("\\(\\.-\\)"                  #Xe122)
     ("\\(\\.=\\)"                  #Xe123)
     ("\\(\\.\\.\\)"                #Xe124)
     ("\\(\\.\\.<\\)"               #Xe125)
     ("\\(\\.\\.\\.\\)"             #Xe126)
     ("\\(\\?=\\)"                  #Xe127)
     ("\\(\\?\\?\\)"                #Xe128)
     ("\\(;;\\)"                    #Xe129)
     ("\\(/\\*\\)"                  #Xe12a)
     ("\\(/\\*\\*\\)"               #Xe12b)
     ("\\(/=\\)"                    #Xe12c)
     ("\\(/==\\)"                   #Xe12d)
     ("\\(/>\\)"                    #Xe12e)
     ("\\(//\\)"                    #Xe12f)
     ("\\(///\\)"                   #Xe130)
     ("\\(&&\\)"                    #Xe131)
     ("\\(||\\)"                    #Xe132)
     ("\\(||=\\)"                   #Xe133)
     ("[^|]\\(|=\\)"                #Xe134)
     ("\\(|>\\)"                    #Xe135)
     ("\\(\\^=\\)"                  #Xe136)
     ("\\(\\$>\\)"                  #Xe137)
     ("\\(\\+\\+\\)"                #Xe138)
     ("\\(\\+\\+\\+\\)"             #Xe139)
     ("\\(\\+>\\)"                  #Xe13a)
     ("\\(=:=\\)"                   #Xe13b)
     ("[^!/]\\(==\\)[^>]"           #Xe13c)
     ("\\(===\\)"                   #Xe13d)
     ("\\(==>\\)"                   #Xe13e)
     ("[^=]\\(=>\\)"                #Xe13f)
     ("\\(=>>\\)"                   #Xe140)
     ("\\(<=\\)"                    #Xe141)
     ("\\(=<<\\)"                   #Xe142)
     ("\\(=/=\\)"                   #Xe143)
     ("\\(>-\\)"                    #Xe144)
     ("\\(>=\\)"                    #Xe145)
     ("\\(>=>\\)"                   #Xe146)
     ("[^-=]\\(>>\\)"               #Xe147)
     ("\\(>>-\\)"                   #Xe148)
     ("\\(>>=\\)"                   #Xe149)
     ("\\(>>>\\)"                   #Xe14a)
     ("\\(<\\*\\)"                  #Xe14b)
     ("\\(<\\*>\\)"                 #Xe14c)
     ("\\(<|\\)"                    #Xe14d)
     ("\\(<|>\\)"                   #Xe14e)
     ("\\(<\\$\\)"                  #Xe14f)
     ("\\(<\\$>\\)"                 #Xe150)
     ("\\(<!--\\)"                  #Xe151)
     ("\\(<-\\)"                    #Xe152)
     ("\\(<--\\)"                   #Xe153)
     ("\\(<->\\)"                   #Xe154)
     ("\\(<\\+\\)"                  #Xe155)
     ("\\(<\\+>\\)"                 #Xe156)
     ("\\(<=\\)"                    #Xe157)
     ("\\(<==\\)"                   #Xe158)
     ("\\(<=>\\)"                   #Xe159)
     ("\\(<=<\\)"                   #Xe15a)
     ("\\(<>\\)"                    #Xe15b)
     ("[^-=]\\(<<\\)"               #Xe15c)
     ("\\(<<-\\)"                   #Xe15d)
     ("\\(<<=\\)"                   #Xe15e)
     ("\\(<<<\\)"                   #Xe15f)
     ("\\(<~\\)"                    #Xe160)
     ("\\(<~~\\)"                   #Xe161)
     ("\\(</\\)"                    #Xe162)
     ("\\(</>\\)"                   #Xe163)
     ("\\(~@\\)"                    #Xe164)
     ("\\(~-\\)"                    #Xe165)
     ("\\(~=\\)"                    #Xe166)
     ("\\(~>\\)"                    #Xe167)
     ("[^<]\\(~~\\)"                #Xe168)
     ("\\(~~>\\)"                   #Xe169)
     ("\\(%%\\)"                    #Xe16a)
     ;; ("\\(x\\)"                     #Xe16b) ;; This ended up being hard to do properly so i'm leaving it out.
     ("[^:=]\\(:\\)[^:=]"           #Xe16c)
     ("[^\\+<>]\\(\\+\\)[^\\+<>]"   #Xe16d)
     ("[^\\*/<>]\\(\\*\\)[^\\*/<>]" #Xe16f))))

(defun add-fira-code-symbol-keywords ()
  (font-lock-add-keywords nil fira-code-font-lock-keywords-alist))

(add-hook 'prog-mode-hook
          #'add-fira-code-symbol-keywords)

(set-language-environment "UTF-8")
(set-default-coding-systems 'utf-8)

;; (load-path "/home/ice1000/.emacs.d/")

(global-set-key (kbd "C-c C-d") 'bing-dict-brief)
(setq bing-dict-save-search-result t)
;; (setq zone-when-idle t 90)
(setq gitter-token "")

(global-set-key (kbd "C-x C-b") 'ibuffer)
(autoload 'ibuffer "ibuffer" "List buffers." t)
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
```