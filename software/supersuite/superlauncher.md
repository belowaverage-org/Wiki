---
title: Super Launcher
description: An admin launcher that minimizes to the system tray.
published: true
date: 2022-12-29T09:26:19.531Z
tags: 
editor: markdown
dateCreated: 2020-04-17T16:29:12.544Z
---

<p align="center">
	<img src="/assets/software/supersuite/superlauncher/logo.svg" width="75">
</p>
<h2 align="center">Super Launcher</h2>

<p align="center">
	<img src="/assets/software/supersuite/superlauncher/newlauncher.png">
</p>

> Super Launcher is an admin launcher that persists in the system tray that can launch programs quickly; optionally, with administrator rights, as another user, or both.

# Setup

1. Install Super Launcher by clicking here: <a href="https://cutt.ly/n2qJrss">Open in Windows Store.</a>
3. Open Super Launcher in the Windows Start Menu.

# Adding Shortcuts

1. Right click the Super Launcher icon located at the [Windows Notification Area](https://en.wikipedia.org/wiki/Taskbar#Taskbar_elements)

![sl_tray.png](/assets/software/supersuite/superlauncher/newtaskbar.png)

2. Select the plus symbol.

![sl_menu.png](/assets/software/supersuite/superlauncher/newaddicon.png)

3. Browse to a shortcut or application you would like to pin to Super Launcher.

![addshortcutwindow.png](/assets/software/supersuite/superlauncher/addshortcutwindow.png)

# Tips

## Launching as another user and/or with administrator rights

1. Open Super Launcher.
2. Click the ellipsis "..." icon to open the menu.
3. Select "Run as".

![newcontextmenu.png](/assets/software/supersuite/superlauncher/newcontextmenu.png)

3. Fill in the options shown below and select "Apply".

![newrunas.png](/assets/software/supersuite/superlauncher/newrunas.png)

>If "Use credentials at startup" is selected, next time Super Launcher starts, the account entered in the fields above will be used automatically.<br><br>If "Elevate at startup" is selected, Super Launcher will attempt elevate itself automatically at startup.

## Backing up shortcuts

1. Open Super Launcher and select the ellipses "..." icon.
2. In the menu shown below, select "View config".

![newcontextmenu.png](/assets/software/supersuite/superlauncher/newcontextmenu.png)

3. In whatever default XML editor, click "file" and then "save as..."

> To restore, simply overwrite the current config with your saved config from steps 1-3.

## File management as another user

1. Open Super Launcher.
2. Select the folder icon.

![newaddicon.png](/assets/software/supersuite/superlauncher/newaddicon.png)

3. A new explorer window will appear running as your specified admin user.

![newexplorer.png](/assets/software/supersuite/superlauncher/newexplorer.png)

## Enable hidden folders and file extensions.

1. Open explorer as described in the section above.
3. In the new window, open the "Options..." dropdown as shown below and select the desired options.

![show_file_ext_and_hidden.png](/assets/software/supersuite/superlauncher/show_file_ext_and_hidden.png)

>"Super Hidden Items" are operating system protected files.

# Frequently Asked Questions

## Why is Super Launcher a better solution than a folder on the desktop?

This issue is commonly asked into question: *"...just make a folder on the desktop with your shortcuts..."* or *"...just pin the shortcuts to the start-menu or the task-bar..."*

If you are asking these questions, then this application is probably not meant for you.

Super Launcher is designed to run applications in a separate user-context, either in an elevated context, a different user-account's context, or both. To do this task natively in Windows, the user is required to hold **Shift** and **Right-click** the program they want to start to force an option to appear in the context menu called "Run as another user..." This can be very time consuming, especially for people who need to run programs as different users like System Administrators and I.T. Technicians.

To read more about how the Windows security system works, see: https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/access-control

## Why can't Super Launcher support a drag-and-drop interface for adding shortcuts?

Super Launcher is designed to run in different security contexts and Windows does not allow applications running in different contexts to interact. Since the Windows Desktop (explorer.exe) runs under your logged in security context, Super Launcher is not able to read the clipboard or drag-and-drop data from the desktop or file explorer.

This means that the only option to add shortcuts in Super Launcher is by opening a new file explorer window under the same security context that Super Launcher is running as.