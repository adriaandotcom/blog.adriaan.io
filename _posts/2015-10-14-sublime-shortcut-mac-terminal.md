---
title: How to create a shortcut in terminal for Mac for Sublime Text
layout: post
---

Copy and paste the code below in `~/.bash_profile`. You can edit the file by typing `nano ~/.bash_profile`

```bash
# Sublime Text alias
alias subl='/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl'
```

For Sublime Text 2 it is

```bash
# Sublime Text alias
alias subl='/Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl'
```
Restart terminal for changes to reflect

Now you can type (without the `$` of course)

```bash
$ subl ~/Downloads/some_file.txt
```
