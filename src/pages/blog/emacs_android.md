---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Emacs GUI in android
description: Emacs GUI in android
date: 2024-01-09
author: "Gold Ayan"
category: "Emacs"
tags:
  - Emacs
  - Android
---

Last year had so much fun in Emacs, When i heard we can install the GUI version in android i was hyped. Recently had time to try it and like to share my experience on it.

First i tried the Emacs available in Fdroid but faced the following issue in it.
- Not able to install any third party software (because of gnuTLS issue)
- Able to integrate with other command line tools like rg (ripgrep), pass and other.

So i was looking for another way to make it possible, i found the *SourceForge - Android ports for GNU Emacs* where we can download both termux and emacs.

### Installation
- First if you have installed Termux and emacs you need to remove it.
- WAIT... before removing Termux, clear App data and Cache. This is important
- If you fail to do above step Termux installation will fail.
- Download the Termux from the sourceforge website `https://sourceforge.net/projects/android-ports-for-gnu-emacs/files/`.
- Then install the Termux provided in the sourceforge repo.
- Once that is done, you can download the Emacs based on your architecture.

### What next?
- You need to give permission for emacs to access the internal storage.
- You can access the internal storage using */sdcard* path.
- After permission, i can able to access the internal files.
- Then tried to load my emacs config and custom themes which to my surprise worked.
- Inorder to access the application installed in the termux, README of the source forge suggest the following lines in `early-init.el` file
```elisp
(setenv "PATH" (format "%s:%s" "/data/data/com.termux/files/usr/bin"
		       (getenv "PATH")))
(setenv "LD_LIBRARY_PATH" (format "%s:%s"
				  "/data/data/com.termux/files/usr/lib"
				  (getenv "LD_LIBRARY_PATH")))
(push "/data/data/com.termux/files/usr/bin" exec-path)
```
- Above code worked, i was able to run `rg` from the eshell.

### What's Missing? (For me atleast)
- Don't know how to change the font.
- I love doom mode line, icons are not loaded.
- I tried installing the nerd font using `nerd-icons-install-font` but still no clue where to put it.
- Also the Termux sibling apps are not able to install from Fdroid like - Termux Styling, Termux widget, Termux API and other...

### Thanks to,
- Po Lu, read more on https://lwn.net/Articles/936576/
- Jianwei Hu, For providing us the Emacs apk + Termux

### Resources
- https://sourceforge.net/projects/android-ports-for-gnu-emacs/files/
- https://www.androidpolice.com/clear-app-cache-data-android/ 
- https://marek-g.github.io/posts/tips_and_tricks/emacs_on_android/
