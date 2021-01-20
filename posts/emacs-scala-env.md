---
title: "scala + emacs lsp"
date: "2021-01-15"
---

[**emacs**](https://www.gnu.org/software/emacs) is my favorite thing in life. so much so, that it is typically the first thing i install on any machine that i'm going to use with any frequency. i will most likely produce many posts about emacs over the course of this blog, and i will love every one of them. this love stems from the extensibility of its human interface; within reason, there is nothing emacs cannot do. it integrates well with gnu file descriptors and streams; has an expressively-unmatched extension language, dubbed **elisp**; and has a community of visionaries that craft astounding software for this... text editor.

while others may use microsoft's rounded-edged **vscode**, a proprietary attempt to capture the extensible editor market; or **vim**, a good editor with a bad extension language; emacs' unparalled database of code, idoms, and community will shine on as part of the computing universe forever.

my emacs config is... [huge](https://github.com/lagooned/emacs)<sup>tm</sup>. i've been using emacs for so long that i've finally gained the confidence to point other emacs users in the right direction. so listen up and copy this down; you'll have a fully functioning, free-and-open-source scala ide in no time.

# terms

- **[lsp](https://github.com/microsoft/language-server-protocol)**
  - short for *language server protocol*. this is a standard for implementing a language server for a given language. it defines how to create a REST server which understands its implenting language (whether it be java, c++, scala, or what have you) and accepts queries which return information which can aid in the development process.

- **[lsp-mode](https://github.com/emacs-lsp/lsp-mode)**
  - a client library for emacs which interacts with servers which implement the language server protocol.

- **[scala](https://www.scala-lang.org)**
  - scala is a multi-paradigm language which blends object-oriented and functional programming. it successfully predicted the growth-trend of preference toward stateless archetectures and offers features for a community retreating toward a model of computing which better handles the concurrency needs of a modern world.
