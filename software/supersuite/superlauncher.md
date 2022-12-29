---
title: Super Launcher
description: An admin launcher that minimizes to the system tray.
published: true
date: 2022-12-29T08:55:21.588Z
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

1. Install Super Launcher by clicking here: <a href="ms-windows-store://pdp/?ProductId=9N269JD69VCX&mode=mini&referrer=storeforweb">Open in Windows Store.</a>
3. Open Super Launcher in the Windows Start Menu.

*Optional:*

* Setup [Launch at startup](#launch-at-startup).

# Adding Shortcuts

1. Right click the Super Launcher icon located at the [Windows Notification Area](https://en.wikipedia.org/wiki/Taskbar#Taskbar_elements)

![sl_tray.png](/assets/software/supersuite/superlauncher/newtaskbar.png)

2. Select the plus symbol.

![sl_menu.png](/assets/software/supersuite/superlauncher/newaddicon.png)

3. Browse to a shortcut or application you would like to pin to Super Launcher.

![addshortcutwindow.png](/assets/software/supersuite/superlauncher/addshortcutwindow.png)

# Tips

## Launching as another user

1. Right click the Super Launcher icon.
2. Select "Run as..."

![sl_menu.png](/assets/software/supersuite/superlauncher/sl_menu.png)

3. Enter the credentials of the account you would like Super Launcher to "run as".

## Launching with administrator rights

1. Right click the Super Launcher icon.
2. Select "Elevate..."

![sl_menu.png](/assets/software/supersuite/superlauncher/sl_menu.png)

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

## Auto start with administrator rights

1. Right click the Super Launcher tray icon and select "Settings..."
2. Check the "Auto run as admin?" check box.
3. Press "Save"
4. Exit Super Launcher and re-open to test.

## Auto start as another user

1. Right click the Super Launcher tray icon and select "Settings..."
2. Enter the "User Name" and "Domain" you would like Super Launcher to attempt to start with.
	* If you do not know what your "Domain" is, simply enter a period: "."
3. Press "Save"
4. Exit Super Launcher and re-open to test.

## Auto start as another user with administrator rights

Selecting "Auto Run As Admin?" and entering a username / domain into the Super Launcher settings will not give you the desired effect. Follow the steps below to get Super Launcher to start as another user with administrator rights.

1. Follow the [Auto start as another user](#auto-start-as-another-user) step above.
2. Follow the [Auto start with administrator rights](#auto-start-with-administrator-rights) step above.
3. Exit one last time and re-open to test.

> It is important you follow these steps in this order exactly.

## Enable hidden folders and file extensions.

1. Right click the Super Launcher tray icon.
2. Select "Open Explorer"
3. In the new window, open the "Options..." dropdown as shown below and select the desired options.

![show_file_ext_and_hidden.png](/assets/software/supersuite/superlauncher/show_file_ext_and_hidden.png)

4. Press the `F5` key on your keyboard to refresh the window with the new options. 

# Frequently Asked Questions

## Why is Super Launcher a better solution than a folder on the desktop?

This issue is commonly asked into question: *"...just make a folder on the desktop with your shortcuts..."* or *"...just pin the shortcuts to the start-menu or the task-bar..."*

If you are asking these questions, then this application is probably not meant for you.

Super Launcher is designed to run applications in a separate user-context, either in an elevated context, a different user-account's context, or both. To do this task natively in Windows, the user is required to hold **Shift** and **Right-click** the program they want to start to force an option to appear in the context menu called "Run as another user..." This can be very time consuming, especially for people who need to run programs as different users like System Administrators and I.T. Technicians.

To read more about how the Windows security system works, see: https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/access-control

## Why does Super Launcher have to be installed, why can't it be portable?

Super Launcher is designed with I.T. Administrators and Technicians in mind. Since Super Launcher can run in multiple security contexts, it is important that the Super Launcher files are installed in a directory that can be accessed by multiple security contexts and users.

Another reason why Super Launcher cannot be portable is because the configuration files need to be accessible by all user / security contexts on the PC. Super Launcher will save all configuration files to `C:\Users\Public\Documents\Below Average\Super Launcher\%DOMAIN%\%USERNAME%\SuperLauncherConfig.xml`.

If you really want to run Super Launcher in a portable mode, install the program to a PC you have administrator rights on, and browse to `C:\Program Files\Below Average\Super Launcher\` and copy the files to wherever you like. Super Launcher should be able to run, but you might encounter some un-expected errors, especially if you try to use the "Elevate" or "Run-as" feature.

## Why can't Super Launcher support a drag-and-drop interface for adding shortcuts?

Super Launcher is designed to run in different security contexts and Windows does not allow applications running in different contexts to interact. Since the Windows Desktop (explorer.exe) runs under your logged in security context, Super Launcher is not able to read the clipboard or drag-and-drop data from the desktop or file explorer.

This means that the only option to add shortcuts in Super Launcher is by opening a new file explorer window under the same security context that Super Launcher is running as.