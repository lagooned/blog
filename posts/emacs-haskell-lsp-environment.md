---
title: "make haskell less intimidating with emacs & lsp"
date: "2021-02-25"
---

as i said before, there would be many posts about emacs and this is no exception. in this iteration, we are looking at haskell language server protocol integration. as i am currently, and always will be, learning haskell extremely slowly, i figured it necessary to leverage my experience with emacs, lsp, and gnu/linux to reap the benefits of the haskell community's ide integration efforts.

# terms

- **[lsp](https://github.com/microsoft/language-server-protocol)**
  - short for *language server protocol*. this is a standard for implementing a language server for a given language. it defines how to create a REST server which understands its implementing language (whether it be haskell, java, c++, scala, or what have you) and accepts queries which return information which can aid in the development process.

- **[haskell](https://www.haskell.org)**
  - named after logicial Haskell Curry, Haskell is a general-purpose, statically-typed, & purely-functional programming language which touts most of the recent innovations in programming and is a consistent source of good ideas for other languages to copy from.
  
- **[mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Modes.html)**
  - an operating mode for emacs which is enabled upon opening a buffer. a mode usually offers a set of functionality that is useful for the type of buffer that was opened. for example, when opening a *\*.hs* file, **haskell-mode** is started.

- **[hook](https://www.gnu.org/software/emacs/manual/html_node/elisp/Setting-Hooks.html)**
  - an unordered list of elisp functions which run when a particular *mode* is enabled. for example, when *haskell-mode*, **haskell-mode-hook** is executed.

- **[lsp-mode](https://github.com/emacs-lsp/lsp-mode)**
  - a client library for emacs which interacts with servers which implement the language server protocol.

- **[use-package](https://github.com/jwiegley/use-package)**
  - a set of elisp functions and macros which makes it easy to lazy-load elisp packages with particular configurations. while it is completely optional, loading elisp packages all at once can be quite time complex and cause very slow start-up time. use of this is very much advised as it will future proof your configurations.

# scope

as was said in my previous [scala-lsp post](/blog/posts/emacs-scala-env), in order to keep this short, i will only be going over the setup of *use-package*, *lsp-mode*, and the operating system configurations required to make a elegant development environment. if you would like to know more about emacs, please check out the following:

- **[je/emacs](http://github.com/lagooned/emacs)**
  - the configuration that i currently use. it is heavily inspired by *[spacemacs](https://www.spacemacs.org)* and *[doom-emacs](https://github.com/hlissner/doom-emacs)*. please feel free to copy stuff out of my config or raise a pr, id be happy to have a look at it.
  - here you can find the configuration as i implemented it, in terms of my own configuration setup, please feel free to use this as a reference

- **[sanemacs](https://sanemacs.com)**
  - provides a good set of sane defaults

- **[awesome emacs](https://github.com/emacs-tw/awesome-emacs)**
  - a great, frequently updated place to look for and learn about more packages to add to your emacs configuration.

# install binaries

- [stack](https://docs.haskellstack.org/en/stable/README/#the-haskell-tool-stack)
  - follow the guide for this and make sure stack is in your **user-wide ~/.profile $PATH** so that emacs can see it
- [haskell-language-server](https://github.com/haskell/haskell-language-server)
  - either build from source or download the pre-compiled binaries; again make sure stack is in your **user-wide ~/.profile $PATH** so that emacs can see it 

# use-package: what do?

i previously went over the syntax for **use-package** **[here](/blog/posts/emacs-scala-env#using-use-package)**, as well as the initial **package.el** setup **[here](/blog/posts/emacs-scala-env#setting-up-package-el)**. perform both of these on your local machine to continue.

# setting up lsp

```elisp
(use-package company
  :ensure t
  :diminish company-mode
  :commands company-mode
  :hook (prog-mode . company-mode))

(use-package lsp-mode
  :ensure t
  :commands lsp)

(use-package haskell-mode
  :ensure t
  :hook (haskell-mode . haskell-doc-mode)
  :commands haskell-mode
  :init
  (add-hook
   ;; prevent eldoc minibuffer hijacking
   'haskell-mode-hook
   (lambda () (eldoc-mode 0))))

(use-package lsp-haskell
  :when
  (and (executable-find "stack")
       (executable-find
        "haskell-language-server-wrapper"))
  :hook
  ((haskell-mode . lsp)
   (haskell-literate-mode . lsp)))
```

# create new stack project

...

# configuring hie.yaml

...

# conclusion

...

-jared
