---
layout: post
title: "Emacs on all the things: Org-mode and babel ftw"
category: emacs
---

Over the last year Emacs somewhat grew on me. At first it was only Org-mode as a [replacement for Evernote][goodbyeEvernote], but then [evil][evil] replaced vim and suddenly Emacs was my goto application for all text editing needs. With every new task my `init.el` got a few lines longer and soon I divided it into several files to stay on top of things.

Of course I want to have this goodness on all my systems, fortunately Emacs is platform-independent. Unfortunately my Emacs configuration is not, different machines have e.g. different paths or a different color theme. Until now I declared one configuration as *the* configuration and manually edited the corresponding settings on all other machines. But there is a better way utilizing the power of Org-mode and babel.

<!--more-->

## Step 0: Embed your Emacs configuration in an Org-mode file
~~~~lisp
* base																		:my-tangle-tag:
#+BEGIN_SRC emacs-lisp :tangle (and (car (member tangle-tag (org-get-tags-at (point)))) "yes")
  ;; org-directory
  (setq org-directory "~/org/")

  ;;; more configuration
#+END_SRC

* alternative	       														:my-other-tag:
#+BEGIN_SRC emacs-lisp :tangle (and (car (member tangle-tag (org-get-tags-at (point)))) "yes")
  ;; org-directory
  (setq org-directory "C:/Users/username/Documents/org/")

  ;;; more configuration
#+END_SRC
~~~~

## Step 1: Replace your init.el
~~~~lisp
(package-initialize)

(setq tangle-tag "my-tangle-tag")
(setq dotfiles-dir (file-name-directory (or (buffer-file-name) load-file-name)))

(setq package-enable-at-startup nil)
(require 'org)
(require 'ob-tangle)

(mapc #'org-babel-load-file (directory-files dotfiles-dir t "\\.org$"))
~~~~

## Step 2: Enjoy!

[evil]: https://gitorious.org/evil/pages/Home
[goodbyeEvernote]: {% post_url 2013-11-08-goodbye-evernote-hello-org-mode %} 
[literateProgramming]: http://orgmode.org/worg/org-contrib/babel/intro.html#literate-programming
