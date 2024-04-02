---
title: What I do when I setup my new Mac
---

When I install my new Mac I do a few things to set it to my needs. Maybe you like those apps and settings and can use it as well. If I need more essentials, hit my up [on X](https://x.com/adriaandotcom).

## Install

- [Firefox](https://www.mozilla.org/en-US/firefox/new) for the dev tools (I tried Safari, but no)
- [Visual Studio Code](https://code.visualstudio.com/Download) for editing text
- [Paste](https://pasteapp.me/) for managing your clipboard
- [Brew](https://brew.sh) for packages
- [nvm](https://github.com/creationix/nvm) for Node versions

## Terminal

- [Inconsolata font](https://github.com/google/fonts/tree/master/ofl/inconsolata)

## Setup git

Add this to the settings file (`~/.gitconfig`):

> This is with rebasing enabled by default for branches and pulls.

```
[alias]
	co = checkout
	br = branch
	ci = commit
	st = status
[user]
	name = Adriaan van Rossum
	email = ...
[branch]
	autosetuprebase = always
[pull]
	rebase = true
[push]
	default = current
[color]
	branch = auto
	diff = auto
	status = auto
[color "branch"]
	current = yellow reverse
	local = yellow
	remote = green
[color "diff"]
	meta = yellow bold
	frag = magenta bold
	old = red bold
	new = green bold
[color "status"]
	added = yellow
	changed = green
	untracked = cyan
```

Maybe change your name :)

Create a file `~/.gitexcludes` with `.DS_Store` in it:

```
echo '.DS_Store' >> ~/.gitexcludes
git config --global core.excludesfile ~/.gitexcludes
```

My settings for Visual Studio Code (I will update when I change my settings):

```json
{
    "telemetry.enableTelemetry": false,
    "telemetry.enableCrashReporter": false,
    "workbench.colorTheme": "Default Light+",
    "workbench.iconTheme": "vs-minimal",
    "workbench.statusBar.visible": true,
    "workbench.activityBar.visible": true,
    "workbench.colorCustomizations": {
        "statusBar.background" : "#1A1A1A",
        "statusBar.noFolderBackground" : "#0A0A0D",
        "statusBar.debuggingBackground": "#511f1f"
    },
    "editor.minimap.enabled": false,
    "workbench.startupEditor": "newUntitledFile",
    "files.insertFinalNewline": true,
    "window.zoomLevel": -1,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "editor.tabSize": 2,
    "javascript.suggestionActions.enabled": false,
    "files.trimTrailingWhitespace": true,
    "extensions.ignoreRecommendations": true,
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.fontWeightBold": "normal",
    "terminal.integrated.lineHeight": 1.2,
    "terminal.integrated.fontFamily": "Inconsolata",
    "terminal.integrated.fontSize": 15
}
```

Disable the weird sound effects of Mac like the trash and screenshot sounds

![](/images/posts/disable-sound-effects.png)
