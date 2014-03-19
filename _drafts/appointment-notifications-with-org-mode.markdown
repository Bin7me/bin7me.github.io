---
layout: post
title: "Appointment notifications with org-mode"
category: technology
---

something something dunst

<!--more-->

~~~lisp
(defun dunst-notify (head msg &optional urgency) 
  "Display a message msg using notify-send."
  (unless urgency (setq urgency "normal"))
  (save-window-excursion
	(shell-command
	 (format
	  "notify-send \"%s\" \"%s\" -u %s"    
	  head
	  msg
	  urgency))))
~~~

~~~lisp
; Erase all reminders and rebuilt reminders for today from the agenda
(defun ms/org-agenda-to-appt ()
  (interactive)
  (setq appt-time-msg-list nil)
  (org-agenda-to-appt))

; Rebuild the reminders everytime the agenda is displayed
(add-hook 'org-agenda-finalize-hook 'ms/org-agenda-to-appt 'append)

; setup appointments when emacs starts
(ms/org-agenda-to-appt)

; activate appointments
(appt-activate t)

;; use notifications for appointment reminders
(when window-system

  (setq appt-display-format 'window)
  
  (defun org-osd-display  (min-to-app new-time msg)
    (dunst-notify "org-appt" msg ))
  
  (setq appt-disp-window-function (function org-osd-display)))
~~~
