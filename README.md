# Npm + Emacs Scaffolding
Settup an npm project with babel, eslint & now tobe edited with emacs.
# Prerequisites
  * EMACS 24 or later.
  * npm
  * git
# Setup
## Repository
 * Clone this repositoy.
 * Delete the `.git` directory to get rid of previous history.
 * Reinitialize git.
 ```
 git clone https://github.com/gomain/npm-emacs-scaffolding.git your-new-project-name
 cd your-new-project-name
 rm -rf .git && git init
 ```
 or use the **Use this template** button then clone your new repo.
 * run `npm install`
## EMACS
### Use js-mode, not js2-mode
Because of js2-mode parsing issue when encounter babel syntax, use js-mode instead.
### Linting with flycheck
Add these lines to your EMACS init file
```
;; set flycheck-javascript-eslint-executable to node_modules/.bin/eslint
(defun use-eslint-from-node-modules ()
  (let* ((root (locate-dominating-file 
		(or (buffer-file-name) default-directory)
		"node_modules"))
	 (eslint (and root
		      (expand-file-name "node_modules/.bin/eslint" root))))
    (when (and eslint (file-executable-p eslint))
      (setq-local flycheck-javascript-eslint-executable eslint))))

(add-hook 'flycheck-mode-hook 'use-eslint-from-node-modules)

(add-hook 'js-mode-hook (lambda ()
			  (flycheck-mode t)))
```        
### Auto complete with Tern
  * Clone [Tern's repository](https://github.com/ternjs/tern).
  ```
  git clone https://github.com/ternjs/tern.git
  ```
  * Assuming Tern was cloned to `~/tern`, add these lines to your EMACS init file
  ```
  ;; setup tern
  (add-to-list 'load-path "~/tern/emacs/")
  (autoload 'tern-mode "tern.el" nil t)
  (eval-after-load 'tern
	  	 '(progn
		      (require 'tern-auto-complete)
		      (tern-ac-setup)))
  (add-hook 'js-mode-hook (lambda ()
	  		  (tern-mode t)
		  	  (auto-complete-mode t)))
  ```
# Known issues
# None issues
### Warning of eslint peerdepency not met
You will get an npm warning such as `WARN eslint-config-standard@14.1.0 requires a peer of eslint@>=6.2.2 but none is installed...`. This is due to `eslint@>=6` is not yet supported by emacs' `flycheck-mode`. This warning can be ignored.
