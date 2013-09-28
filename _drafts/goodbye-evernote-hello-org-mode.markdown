---
layout: post
title: "Goodbye Evernote, Hello org-mode!"
category: technology
---
## Introduction
It finally happened. I got hooked on [Emacs][emacs] [Org-mode][orgmode]. And the worst part: I felt right at home. Though some keybindings are still mind-boggling, Emacs is going strong to become my text editor of choice for most use-cases. (Don't worry VIM, I still love you) Org-mode on the other hand is going strong to become my PIM-tool of choice, at least for notes and todos and maybe a bit of my calendar. Thus, I now have two tools trying to fill the same void in my life: [Evernote][evernote] and Org. Evernote has a slick smartphone application and a polished web-interface, Org is free software, based on the glory of plain text and gives more geek-cred. Let's see if the latter can replace the former.

<!--more-->

## Reevaluating Evernote
After some fiddling with org-mode I reevaluated my use of Evernote and realized a few things:

1. Most of my notes are clipped articles or auto-generated.
2. Most of my hand-written notes are more often read than written.
3. Most of my notes were accessed only a couple of times. (or never)
4. A lot of notes contain PDFs.
5. On my smartphone I only read notes most of the time.

Especially 1. and 3. might be a hint that Evernote was never really integrated into my daily workflow. Therefore migrating only the core of my notes to org-mode and ditching everything else will be enough.

## The tricky part
For versioning and syncing my notes I use a private [git][git] repository hosted on [bitbucket][bitbucket]. Emacs as well as git are available for Windows too, finally a native solution for my desktop and laptop. The problem in this equation is my smartphone. There is a Emacs port for Android, but without a hardware keyboard it's tedious to use and I would still need a git client to get access to my notes. Org has a feature to interact with a companion application named MobileOrg, but I haven't had the time to look into it and to be honest, it looks a bit outdated.

[bitbucket]: https://www.bitbucket.org
[droidEditPro]: https://play.google.com/store/apps/details?id=com.aor.droidedit.pro
[emacs]: http://www.gnu.org/software/emacs/
[evernote]: http://www.evernote.com
[gitAnnex]: http://git-annex.branchable.com/
[git]: http://git-scm.com/
[orgmode]: http://www.orgmode.org
