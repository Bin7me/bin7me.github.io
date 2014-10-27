---
layout: post
title: "Eine für alle: Mit Org-mode die init.el plattformunabhängiger machen"
category: emacs
---

Im Verlaufe des letzten Jahres ist mir Emacs überraschenderweise ans Herz gewachsen. Zunächst ging es ja nur darum mit Org-mode einen [Ersatz für Evernote][goodbyeEvernote] zu schaffen, aber dann hat [evil][evil] auf einmal Vim ersetzt und auf einmal war Emacs mein Standard-Werkzeug in Sachen Textbearbeitungen aller Art. Natürlich wollte ich diesen Luxus auf all meinen Systemen haben, zum Glück ist Emacs plattformunabhängig. Meine `init.el` war, obwohl fein säuberlich in einzelne Dateien gegliedert, es allerdings nicht. Nach jeder Änderung mussten die betreffenden Abschnitte auf meiner Windows-Kiste händisch angepasst werden. Lästig, aber zu dem Zeitpunkt noch im Rahmen.

Jetzt kam eine zweite Windows-Maschine dazu und aus *lästig* wurde *inakzeptabel*. Also musste eine neue Lösung her, am Besten eine Art Single-Point-Of-Configuration um zwischen den einzelnen Systemen umzuschalten. Org-mode und Babel to the rescue!

<!--more-->

## Schritt 0: Die Emacs-Konfiguration in ein Org-mode-Dokument einbetten
Zunächst gilt es natürlich die `init.el` bzw. deren Bestandteile in ein entsprechendes Org-mode-Dokument zumzuziehen, die [Introduction to Babel][literateProgramming] verrät was es damit auf sich hat. Die Grundidee dabei ist, den Inhalt der `init.el` in Emacs-Lisp-SRC-Blöcke aufzuteilen und beim Start von Emacs diese Blöcke aus der Org-mode-Datei zu extrahieren und zusammen als Konfiguration zu laden. Soweit so gut.

Nun möchte ich aber nicht *alle* SRC-Blöcke laden, sondern nur die für die jeweilige Maschine relevanten, zur Unterscheidung bieten sich hierbei Tags an. Im untenstehenden Codebeispiel ist ein Auschnitt aus einer Org-mode-Datei zu sehen, die unter der Überschrift *"Pfade"* zwei Alternative Konfigurationen enthält. Getaggt sind die Unter-Überschriften mit dem Namen des jeweiligen Rechners. Die Magie passiert jetzt in den Zeilen 3 bzw. 11, im Header der einzelnen SRC-Blöcke. 

{% highlight lisp linenos=table %}
* Pfade
** base																		:my-linux-box:
#+BEGIN_SRC emacs-lisp :tangle (and (car (member tangle-tag (org-get-tags-at (point)))) "yes")
  ;; org-directory
  (setq org-directory "~/org/")

  ;;; more configuration
#+END_SRC

** alternative	       														:my-windows-box:
#+BEGIN_SRC emacs-lisp :tangle (and (car (member tangle-tag (org-get-tags-at (point)))) "yes")
  ;; org-directory
  (setq org-directory "C:/Users/username/Documents/org/")

  ;;; more configuration
#+END_SRC
{% endhighlight %}

Der Audruck `(and (car (member tangle-tag (org-get-tags-at (point)))) "yes")` prüft, ob in den Tags der aktuellen Org-Überschrift der Inhalt der Variable `tangle-tag` enthalten ist und setzt in dem Fall den Wert des `:tangle`-Parameters auf `"yes"`. D.h. es werden nur die SRC-Blöcke extrahiert die mit dem `tangle-tag` getaggt sind. Hört sich schon mal gut an, fehlt nur noch eine Kleinigkeit.

## Step 1: Die init.el austauschen
Nachdem die Emacs-Konfiguration jetzt ordentlich aufgeteilt und getaggt ist, muss sie nur noch gemäß den Tags aus der Org-mode-Datei extrahiert und geladen werden. Dafür sorgt folgende `init.el` (siehe auch die [Introduction to Babel][literateProgramming]):

{% highlight lisp linenos=table %}
(package-initialize)

(setq tangle-tag "my-linux-box")
(setq dotfiles-dir (file-name-directory (or (buffer-file-name) load-file-name)))

(setq package-enable-at-startup nil)
(require 'org)
(require 'ob-tangle)

(mapc #'org-babel-load-file (directory-files dotfiles-dir t "\\.org$"))
{% endhighlight %}

Relevant ist vor allem Zeile 3, in der `tangle-tag` entsprechend der aktuellen Maschine gesetzt wird. Diese Anpassung ist auf jedem System einmalig vorzunehmen. Die letzte Zeile lädt jetzt alle `*.org`-Dateien im gleichen Verzeichnis wie die `init.el`, extrahiert alle Emacs-Lisp-Blöcke und lädt diese in Emacs. Das Ergebnis: Eine Emacs-Konfiguration die nur Code enthält, der für das aktuelle System getaggt wurde. Nice.

## Step 2: Zurücklehnen und genießen


[evil]: https://gitorious.org/evil/pages/Home
[goodbyeEvernote]: {% post_url 2013-11-08-goodbye-evernote-hello-org-mode %} 
[literateProgramming]: http://orgmode.org/worg/org-contrib/babel/intro.html#literate-programming
