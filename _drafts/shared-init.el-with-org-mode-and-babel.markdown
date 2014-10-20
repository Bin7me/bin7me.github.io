---
layout: post
title: "Eine für alle: Mit Org-mode die init.el plattformunabhängiger machen"
category: emacs
---

Im Verlaufe des letzten Jahres ist mir Emacs überraschenderweise ans Herz gewachsen. Zunächst ging es ja nur darum mit Org-mode einen [Ersatz für Evernote][goodbyeEvernote] zu schaffen, aber dann hat [evil][evil] auf einmal Vim ersetzt und auf einmal war Emacs mein Standard-Werkzeug in Sachen Textbearbeitungen aller Art. Natürlich wollte ich diesen Luxus auf all meinen Systemen haben, zum Glück ist Emacs plattformunabhängig. Meine `init.el` war, obwohl fein säuberlich in einzelne Dateien gegliedert, es allerdings nicht. Nach jeder Änderung mussten die betreffenden Abschnitte auf meiner Windows-Kiste händisch angepasst werden. Lästig, aber zu dem Zeitpunkt noch im Rahmen.

Jetzt kam eine zweite Windows-Maschine dazu und aus *lästig* wurde *nervtötend*. Also musste eine neue Lösung her, am Besten eine Art Single-Point-Of-Configuration um zwischen den einzelnen Systemen umzuschalten. Org-mode und Babel to the rescue!

<!--more-->

## Step 0: Embed your Emacs configuration in an Org-mode file
{% highlight lisp linenos=table %}
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
{% endhighlight %}

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
