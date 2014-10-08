![Japanese](https://github.com/aki2o/e2wm-term/blob/master/README-ja.md)

# What's this?

This is a extension of Emacs that is a perspective of ![e2wm.el](https://github.com/kiwanami/emacs-window-manager) for work in terminal.

![demo](img/demo.gif)

# Feature

-   invoke command by not a newline key but a dedicated key for avoiding an unintended invocation.
-   command can be written as multiline because a newline key just inputs a linefeed.
-   show a help of the command, which you are typing to invoke, automatically into a dedicated window.
-   show any help command result into not a terminal window but a dedicated window.
-   show comand histories into a dedicated window and access them quickly.

# Install

### If use package.el

2014/10/09 Now application

### If use el-get.el

2014/10/09 Now application

### If use auto-install.el

```lisp
(auto-install-from-url "https://raw.github.com/aki2o/e2wm-term/master/e2wm-term.el")
```
-   In this case, you need to install each of the following dependency.

### Manually

Download e2wm-term.el and put it on your load-path.  
-   In this case, you need to install each of the following dependency.

### Dependency

-   ![e2wm.el](https://github.com/kiwanami/emacs-window-manager)
-   ![log4e.el](https://github.com/aki2o/log4e)
-   ![yaxception.el](https://github.com/aki2o/yaxception)

# Configuration

### First

```lisp
(require 'e2wm-term)
```

### Choice default backend

Backend means a kind of feature as terminal shown in main window.  
When this perspective is started at first, `e2wm-term:default-backend` is used (in default 'shell).  
For check available backend, do M-x `e2wm-term:show-backends`.  
For the detail, do M-x `e2wm-term:describe-backend`.  

### Define key to invoke command

In default, the keystroke is `C-RET`.  
If you want to change it, add key to `e2wm-term:input-mode-map` like the following.  

```lisp
(e2wm:add-keymap
 e2wm-term:input-mode-map
 '(("C-m" . e2wm-term:input-invoke-command)
   ) e2wm:prefix-key)
```

### Guess help command

this perspective handles the command to show a help by the value of `e2wm-term:help-guess-command`.  
-   It's `t` &#x2026; always put the result into help window without the invocation in main window.
-   It's ='ask= (default) &#x2026; ask whether to put the result into help window.
-   It's `nil` &#x2026; always invoke the command in main window.

Also, `e2wm-term:help-guess-regexp` is used for a judgment of whether the command is a help command.  
In default, the value matches the command includes "help" as a word like "git help status".  

Also, the command includes "&#x2013;help" option always is handled as a help command.  

### Show current command help

While user input a command string in input window,
a help of the active command is shown in help window automatically by `e2wm-term:command-helper`.  

### Current work directory

A header of input window shows a current work directory of self.  
A update of it is done by `e2wm-term:command-cwd-checker` / `e2wm-term:command-cwd-updaters`.  

### Pager

For pager configuration in main window, use `e2wm-term:command-pager` / `e2wm-term:command-pager-variables`.  
Values of environment variables of `e2wm-term:command-pager-variables` are replaced
with `e2wm-term:command-pager` at a start of thie perspective.  

### Other

For check all config items, do M-x `customize-group` "e2wm-term".  

# Usage

### Start perspective

M-x `e2wm-term:dp` or M-x `e2wm:pst-change-command` then select `term`.  

### Input and invoke command

Input a invoked command string without a escape of linefeed even if it's multiline.  
Then, push the key bound to `e2wm-term:input-invoke-command`.  

### Control terminal window

A terminal of active backend is shown in main window.  
You are able to control the terminal in input window by the same key as the terminal key map.  
For example, `comint-interrupt-subjob` runs in main window by pushing `C-c C-c` in input window.  
-   However, the `e2wm-term:input-mode-map` keys are excepted

### Access history window

You are able to access command histories by the following keys.  
`prefix` means `e2wm:prefix-key`.  
-   `e2wm-term:history-move-previous` ( `C-c C-p` / `prefix p` ) &#x2026; move to a previous history
-   `e2wm-term:history-move-next` ( `C-c C-n` / `prefix n` ) &#x2026; move to a next history
-   `e2wm-term:input-history-previous` ( `M-p` ) &#x2026; move to a previous history and past that into input window
-   `e2wm-term:input-history-next` ( `M-n` ) &#x2026; move to a next history and past that into input window
-   `e2wm-term:history-send-pt-point` ( `prefix i` ) &#x2026; yank a current history
-   `e2wm-term:history-grep` ( `prefix g` ) &#x2026; grep histories
-   `e2wm-term:history-show-all` ( `prefix a` ) &#x2026; show all histories ( for turn back from grep )

### Control help window

`e2wm-term:help-mode`, which is a major mode for help window, inherits `view-mode`.  
Then, for quit from help window, push `q`.  
Also, there are the following keys to control a help window.  
-   `e2wm-term:dp-help-toggle-command` ( `prefix h` ) &#x2026; toggle on/off of display of a help window
-   `e2wm-term:dp-help-maximize-toggle-command` ( `prefix H` ) &#x2026; toggle on/off of maximized of a help window

### Select terminal buffer

When the buffer of active backend is plural,
you are able to select them by `e2wm-term:dp-select-main-buffer` ( `prefix t` ).  

### Add backend

use `e2wm-term:regist-backend`.  

# Consideration

### Pager

In general, terminal uses a interactive program like "less" command
as the pager program which is used for a browse of long results of command.  
But, such interactive program can not be controled from input window.  
So, this perspective uses "cat" command, which is not a interactive program, as pager in default.  
About that configuration, see "Pager" section in "Configuration" above.  

### Command termination in multiline

A command termination, ( e.g. ";" in /bin/sh ), can be skipped in terminal like the following.  

```sh
~$ for e in `ls`
> do
> echo $e
> done
```

But, you have to input command with a command termination in input window like the following.  

```sh
for e in `ls`;
do
echo $e;
done
```

# Tested On

-   Emacs &#x2026; GNU Emacs 24.3.1 (i686-pc-linux-gnu, GTK+ Version 3.4.2) of 2014-02-22 on chindi10, modified by Debian
-   e2wm.el &#x2026; 1.2
-   log4e.el &#x2026; 0.2.0
-   yaxception.el &#x2026; 0.3.2

**Enjoy!!!**
