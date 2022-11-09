---
title: Use fake built-in packages to organize your GNU Emacs config
notetype: feed
date: 2022-11-09
---

By using `use-package`, you can organize your
`~/.config/emacs/init.el` file and group related settings together by
package.  So, for instance, you can both install and setup the
`company` package by using the code below:

```elisp
(use-package company
  :config
  (setq company-dabbrev-downcase nil)
  (setq company-idle-delay 0.5)
  :hook (after-init . global-company-mode))
```

You can do something similar even for settings that are not related to
any specific package. For instance, you can create a fake package
called `saner-defaults` so all your settings related to improving GNU
Emacs' default settings are in the same place:

```elisp
(use-package saner-defaults
  :straight '(saner-defaults :type built-in)
  :init
  (setq inhibit-startup-message t)
  (column-number-mode 1)
  (scroll-bar-mode -1)
  (tool-bar-mode -1)
  (menu-bar-mode 1)
  (add-hook 'after-init-hook #'global-display-line-numbers-mode)
  (setq frame-resize-pixelwise t)

  (provide 'saner-defaults'))
```

You can have multiple of these throughout your config file and you can
even use them to, for instance, make sure your code only runs after
multiple packages have been installed.  As another example, you can
have something like this:

```elisp
(use-package all-themes
  :straight '(all-themes :type built-in)
  :after modus-themes gruvbox-theme dracula-theme
         color-theme-sanityinc-tomorrow ef-themes
         everforest-theme kaolin-themes
  :init
  (load-theme ars/dark-theme t)
  (set-face-attribute 'default nil
		      :font ars/dark-font
		      :height ars/dark-height
		      :weight 'normal
		      :width ars/dark-width)

  (provide 'all-themes))
```

Which will wait until all the themes listed after `:after` are loaded
before running the code under `:init`. The secret for this to work is
to use a custom Straight recipe using the `built-in` type.  In the
examples above, the recipes were, respectivelly, `'(saner-defaults :type built-in)`
and `'(all-themes :type built-in)`.

Also, don't forget to include `(provide 'the-name-you-chose')` at the
end of the `:init` block so that straight/use-package thinks the
package was actually loaded.

For a full example of how a file using this technique looks like, take
a look at my
[~/.config/emacs/init.el](https://github.com/ars-dev-br/dot-config/blob/d9c0c0e8bf175f9be4a338eee196362286aee198/emacs/init.el) in GitHub.

###### Changelog

2022-11-09
: Page creation.
