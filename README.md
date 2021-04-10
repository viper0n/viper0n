- 👋 Hi, I’m @viper0n
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
viper0n/viper0n is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

(setq lexical-binding t)
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)

(package-initialize)

(setq gc-cons-threshold 100000000)

(add-to-list 'default-frame-alist '(font . "DejaVu Sans Mono-11"))

(setq
 ;; No need to see GNU agitprop.
 inhibit-startup-screen t
 ;; No need to remind me what a scratch buffer is.
 initial-scratch-message nil
 ;; Never ding at me, ever.
 ring-bell-function 'ignore
 ;; Prompts should go in the minibuffer, not in a GUI.
 use-dialog-box nil
 ;; Let C-k delete the whole line.
 kill-whole-line t
 )

;; Never mix tabs and spaces. Never use tabs, period.
;; We need the setq-default here because this becomes
;; a buffer-local variable when set.
(setq-default indent-tabs-mode nil)

(set-charset-priority 'unicode)
(setq locale-coding-system 'utf-8)
(set-terminal-coding-system 'utf-8)
(set-keyboard-coding-system 'utf-8)
(set-selection-coding-system 'utf-8)
(prefer-coding-system 'utf-8)
(setq default-process-coding-system '(utf-8-unix . utf-8-unix))

(delete-selection-mode t)
(global-display-line-numbers-mode t)
(column-number-mode)

(setq
 make-backup-files nil
 auto-save-default nil
 create-lockfiles nil)

(setq enable-local-variables :all)


(add-to-list 'default-frame-alist '(fullscreen . maximized))

(when (window-system)
  (tool-bar-mode -1)
  (scroll-bar-mode -1)
  (tooltip-mode -1))

(show-paren-mode)

(use-package all-the-icons)


;; Neotree because we need more trees
(use-package neotree
  :bind ("<f2>" . neotree-toggle)
  :config (setq neo-window-width 55))


;; Use doom-themes becasue they are so much better
(use-package doom-themes
  :config
  (setq doom-challenger-deep-brighter-comments t
        doom-challenger-deep-brighter-modeline t)
  (load-theme 'doom-dracula t)
  (doom-themes-visual-bell-config)
  (doom-themes-neotree-config)
  ;;(doom-themes-org-config)
  )


;; Better than the default crap
(use-package doom-modeline
  :config (doom-modeline-mode))


;; Color match "matching" braces
(use-package rainbow-delimiters
  :hook ((prog-mode . rainbow-delimiters-mode)))


;; Bring up next possible key combination list
(use-package which-key
  :config
  (which-key-mode)
  (which-key-setup-side-window-bottom)
  :custom (which-key-idle-delay 1.2))


;; Makes jumping between multiple split windows easier
(use-package ace-window
  :config
  ;; Show the window designators in the modeline.
  (ace-window-display-mode)

   ;; Make the number indicators a little larger. I'm getting old.
  (set-face-attribute 'aw-leading-char-face nil :height 2.0 :background "black")

  (defun my-ace-window (args)
    "As ace-window, but hiding the cursor while the action is active."
    (interactive "P")
    (cl-letf
        ((cursor-type nil)
         (cursor-in-non-selected-window nil))
      (ace-window nil)))


  :bind (("C-," . my-ace-window))
  :custom
  (aw-keys '(?a ?s ?d ?f ?g ?h ?j ?k ?l) "Designate windows by home row keys, not numbers.")
  (aw-background nil))


;; Matching indent easy to follow :)
(use-package highlight-indent-guides
  :config
  (setq highlight-indent-guides-method 'character)  
  :hook (prog-mode . highlight-indent-guides-mode))


;; Beautify page break lines
(use-package page-break-lines)


;; Fancy front page
;;(use-package dashboard
;;  :after page-break-lines
;;  :config  
;;  (setq dashboard-startup-banner 'logo)
;;  (setq dashboard-set-init-info t)
;;  (setq dashboard-set-heading-icons t)
;;  (setq dashboard-set-file-icons t)
;;  (dashboard-setup-startup-hook))


;; Setup ivy, counsel etc
(use-package ivy
  :diminish
  :bind(:map ivy-minibuffer-map
        ("<return>" . ivy-alt-done))
  :custom
  (ivy-height 30)
  (ivy-use-virtual-buffers t)
  (ivy-use-selectable-prompt t)
  :config
  (ivy-mode 1))

(use-package all-the-icons-ivy-rich
  :init (all-the-icons-ivy-rich-mode 1)
  :config
  (setq inhibit-compacting-font-caches t))

(use-package ivy-rich
  :custom
  (ivy-virtual-abbreviate 'full)
  (ivy-rich-switch-buffer-align-virtual-buffer nil)
  (ivy-rich-path-style 'full)
  :config
  (ivy-rich-project-root-cache-mode)
  (setcdr (assq t ivy-format-functions-alist) #'ivy-format-function-line)
  (ivy-rich-mode))

(use-package counsel
  :init
  (counsel-mode 1)
  :diminish)


;; Custom functions and minor-mode actions

;;(defun find-file-right (filename)
;;  (interactive)
;;  (split-window-right)
;;  (other-window 1)
;;  (find-file filename))
;;
;;(defun find-file-below (filename)
;;  (interactive)
;;  (split-window-below)
;;  (other-window 1)
;;  (find-file filename))
;;
;;(ivy-add-actions
;; 'counsel-find-file
;; '(("v" find-file-right "open right")
;;   ("h" find-file-below "open below")))
;;
;;(ivy-add-actions
;; 'counsel-recentf
;; '(("v" find-file-right "open right")
;;   ("h" find-file-below "open below")))
;;
;;(ivy-add-actions
;; 'counsel-buffer-or-recentf
;; '(("v" find-file-right "open right")
;;   ("h" find-file-below "open below")))
;;
;;(ivy-add-actions
;; 'ivy-switch-buffer
;; '(("v" find-file-right "open right")
;;   ("h" find-file-below "open below")))

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(package-selected-packages
   '(dashboard neotree page-break-lines highlight-indent-guides all-the-icons-ivy-rich ivy-rich counsel ace-window which-key rainbow-delimiters doom-modeline all-the-icons use-package doom-themes)))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )
