---
title: Super Launcher
description: An admin launcher that minimizes to the system tray.
published: true
date: 2021-02-17T15:37:26.291Z
tags: 
editor: markdown
dateCreated: 2020-04-17T16:29:12.544Z
---

<p align="center">
	<img src="/assets/software/supersuite/superlauncher/logo.svg" width="75">
</p>
<h2 align="center">Super Launcher</h2>

<p align="center">
	<img src="/assets/software/supersuite/superlauncher/superlaunchermain.png">
</p>

> Super Launcher is an admin launcher that persists in the system tray that can launch programs quickly; optionally, with administrator rights, as another user, or both.

# Setup

1. Download Super Launcher from [here](https://github.com/belowaverage-org/SuperLauncher/releases).
2. Run the installer, or run the portable executable.
3. Start adding shortcuts.

*Optional:*

* Setup [Launch at startup](#launch-at-startup).

# Tips

## Launching as another user

![runasanotheruser.png](/assets/software/supersuite/superlauncher/runasanotheruser.png)

## Launching with administrator rights

![runasadmin.png](/assets/software/supersuite/superlauncher/runasadmin.png)

## Launch at startup

1\. Find the Super Launcher shortcut or program.
2\. Right-click the shortcut / program and select Copy.

![copysg.png](/assets/software/supersuite/superlauncher/copysg.png)

3\. With the file location open, press the `Windows Key + R`, type `shell:startup`, then select OK. This opens the Startup folder.

![openstartup.png](/assets/software/supersuite/superlauncher/openstartup.png)

4\. In the Startup folder, paste the shortcut to Super Launcher.

![pasteshortcut.png](/assets/software/supersuite/superlauncher/pasteshortcut.png)

5\. Log off your computer then log in to test.

## Backing up shortcuts

1. Right click on the Super Launcher tray icon.
2. Select "View Config..."
3. In whatever default XML editor, click "file" and then "save as..."

> To restore, simply overwrite the current config with your saved config from steps 1-3.

## File management as another user

1. Right click on the Super Launcher tray icon.
2. Select "Open Explorer".

![superlauncherexplorer.png](/assets/software/supersuite/superlauncher/superlauncherexplorer.png)