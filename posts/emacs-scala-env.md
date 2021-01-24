---
title: "scala + emacs lsp = heckin cool"
date: "2021-01-23"
---

[**emacs**](https://www.gnu.org/software/emacs) is my favorite thing in life. so much so, that it is typically the first thing i install on any machine that i'm going to use with any frequency. i will most likely produce many posts about emacs over the course of this blog, and i will love every one of them. this love stems from the extensibility of its human interface; within reason, there is nothing emacs cannot do. it integrates well with gnu file descriptors and streams; has an expressively-unmatched extension language, dubbed **elisp**; and has a community of visionaries that craft astounding software for this... text editor.

while others may use microsoft's rounded-edged **vscode**, a proprietary attempt to capture the extensible editor market; or **vim**, a good editor with a bad extension language; emacs' unparalled database of code, idioms, and community will shine on as part of the computing universe forever.

my emacs config is... [huge](https://github.com/lagooned/emacs)<sup>tm</sup>. i've been using emacs for so long that i've finally gained the confidence to point other emacs users in the right direction. so listen up and copy this down; you'll have a fully functioning, free-and-open-source scala ide in no time.

# terms

- **[lsp](https://github.com/microsoft/language-server-protocol)**
  - short for *language server protocol*. this is a standard for implementing a language server for a given language. it defines how to create a REST server which understands its implementing language (whether it be java, c++, scala, or what have you) and accepts queries which return information which can aid in the development process.

- **[scala](https://www.scala-lang.org)**
  - a multi-paradigm language which blends object-oriented and functional programming. it successfully predicted the growth-trend of preference toward stateless code and offers features for a community retreating toward a model of computing which better handles the concurrency needs of a modern world.

- **[mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Modes.html)**
  - an operating mode for emacs which is enabled upon opening a buffer. a mode usually offers a set of functionality that is useful for the type of buffer that was opened. for example, when opening a *\*.java* file, **java-mode** is started.

- **[hook](https://www.gnu.org/software/emacs/manual/html_node/elisp/Setting-Hooks.html)**
  - an unordered list of elisp functions which run when a particular *mode* is enabled. for example, when *java-mode*, **java-mode-hook** is executed.

- **[lsp-mode](https://github.com/emacs-lsp/lsp-mode)**
  - a client library for emacs which interacts with servers which implement the language server protocol.

- **[use-package](https://github.com/jwiegley/use-package)**
  - a set of elisp functions and macros which makes it easy to lazy-load elisp packages with particular configurations. while it is completely optional, loading elisp packages all at once can be quite time complex and cause very slow startup time. use of this is very much advised as it will future proof your configurations.

# scope

in order to keep this short, i will only be going over the setup of *use-package*, *lsp-mode*, and the operating system configurations required to make a elegant development environment. if you would like to know more about emacs, please check out the following:

- **[je/emacs](http://github.com/lagooned/emacs)**
  - the configuration that i currently use. it is heavily inspired by *[spacemacs](https://www.spacemacs.org)* and *[doom-emacs](https://github.com/hlissner/doom-emacs)*. please feel free to copy stuff out of my config or raise a pr, id be happy to have a look at it.

- **[sanemacs](https://sanemacs.com)**
  - provides a good set of sane defaults

- **[awesome emacs](https://github.com/emacs-tw/awesome-emacs)**
  - a great, frequently updated place to look for and learn about more packages to add to your emacs configuration.

# install binaries

- [sbt](https://www.scala-sbt.org)
  - the quintessential **s**cala **b**uilt **t**ool. install this and make sure it's on your `PATH` before you start tying to use this config
- [metals](https://github.com/scalameta/metals)
  - the imlementation of lsp for scala; the centerpiece of this config and required for functionality
- [bloop](https://scalacenter.github.io/bloop/)
  - a compile server for scala that works with sbt, is automatically used by metals if your project has a .bloop directory, installed mainly because metals throws errors upon startup if you don't have it installed, not sure how required it is... follow the instructions [here](https://scalameta.org/metals/docs/build-tools/bloop.html) here to install it, just to be safe

# using use-package

*use-package* is a huge convenience for adding emacs packages. the most common syntax for a use-package invocation is as follows:

```elisp
(use-package package-name
 :ensure (automatically-download-package-if-not-found-p)
 :when (prerequsite-for-package-p)
 :commands
 (command-to-trigger-load-1 command-to-trigger-load-2)
 :init
 (initial-config-before-loading-package)
 :after
 (package-to-wait-for-1 package-to-wait-for-2)
 :config
 (subsequent-config-after-loading-package))
```

here **package-name** is the package we are controlling with this definition; **:ensure** is a predicate (either being **t** or **nil**) which controls whether or not to download and install the package if it is not locally available; **:commands** takes a list of commands, which once invoked will trigger the loading of *package-name*; **:init** is a block of commands that will be evaluated sequentially immediately, before package load; and **:config** is a block of commands much like *:init*, however, these commands are evaluated when the package loads. there are some [other useful macros](https://github.com/jwiegley/use-package/blob/master/README.md), but these are the ones we will use.

# setting up lsp

now that we know a little about how to *use-package* we can start by installing it programatically by doing the following in out ~/.emacs.d/init.el:

```elisp
(require 'package)

(setq
 package-archives
 '(("gnu" . "https://elpa.gnu.org/packages/")
   ("marmalade" . "https://marmalade-repo.org/packages/")
   ("melpa-stable" . "https://stable.melpa.org/packages/")
   ("melpa" . "https://melpa.org/packages/")))

(when (version< emacs-version "27")
  (package-initialize))

(if (not (package-installed-p 'use-package))
      (progn (package-refresh-contents)
             (package-install 'use-package)))
```

this will initialize the elisp module which controls package installation, and then install the latest version of use-package. to make it execute, run `(load-file "~/.emacs.d/init.el")` in the **\*scratch\*** buffer. this will start the installation process.

then after that has been done, add the following use-package definitions:

```elisp
(use-package company
  :ensure t
  :diminish company-mode
  :commands company-mode
  :hook (prog-mode . company-mode))

(use-package lsp-mode
  :ensure t
  :commands lsp)

(use-package company-lsp
  :ensure t
  :after lsp company)

(use-package scala-mode
  :ensure t
  :interpreter ("scala" . scala-mode))

(use-package sbt-mode
  :ensure t
  :when (executable-find "sbt")
  :commands sbt-start sbt-command
  :config (setq sbt:program-options '("-Dsbt.supershell=false")))

(use-package lsp-metals
  :ensure t
  :when (and (executable-find "metals-emacs")
             (executable-find "bloop"))
  :hook (scala-mode . lsp))
```

if you have installed the binaries enumerated earlier, reload your init file and open a scala file. lsp will offer to import the project and you should have a working scala ide. get your test driven development on and learn some emacs and scala!
