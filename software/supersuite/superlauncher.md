---
title: Super Launcher
description: 
published: true
date: 2020-04-19T00:47:42.437Z
tags: 
---

# About

![suplaunch.png](/assets/software/supersuite/superlauncher/suplaunch.png)

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

>Below are to examples on running SuperLauncher at startup.
>If you are unsure what the difference between "As your current user" and "As another user" in the context of running Super Launcher at startup, then only follow the "As your current user" portion of this guide.

### As your current user

1\. Find the Super Launcher shortcut or program.
2\. Right-click the shortcut / program and select Copy.

![copysg.png](/assets/software/supersuite/superlauncher/copysg.png)

3\. With the file location open, press the `Windows Key + R`, type `shell:startup`, then select OK. This opens the Startup folder.

![openstartup.png](/assets/software/supersuite/superlauncher/openstartup.png)

4\. In the Startup folder, paste the shortcut to Super Launcher.

![pasteshortcut.png](/assets/software/supersuite/superlauncher/pasteshortcut.png)

5\. Log off your computer then log in to test.

### As another user

Below is an example exported Scheduled Task. Import this into Scheduled Tasks and modify to your desire. This is where the magic happens: `runas /savecred /user:domain\username "cmd /c start C:\SuperLauncher"`

```xml
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.4" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Date>2019-01-03T22:05:47.1965864</Date>
    <Author>Dylan Bickerstaff</Author>
    <URI>\SuperLauncher</URI>
  </RegistrationInfo>
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
    </LogonTrigger>
  </Triggers>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <DisallowStartOnRemoteAppSession>false</DisallowStartOnRemoteAppSession>
    <UseUnifiedSchedulingEngine>true</UseUnifiedSchedulingEngine>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>runas</Command>
      <Arguments>/savecred /user:domain\username "cmd /c start C:\SuperLauncher"</Arguments>
    </Exec>
  </Actions>
</Task>
```

## Backing up shortcuts

1. Open Run dialog by pressing `Windows Key + R`.
2. In the dialog box, input `%appdata%\..\Local\Dylan_Bickerstaff` and then press `Enter`.
3. Browse to `.\SuperLauncher.exe_Url_[Random Letters and Numbers]`.
4. Browse to `#.#.#.#`.
5. Copy the `user.config` to a safe place.

> To restore the shorcuts, instead of doing step 5, paste a previously backed up `user.config` here by over-writing the current file.

## File management as another user

A quirky tip to managing / accessing shared or local folders via an admin account can be accomplished by right clicking the Super Launcher tray icon, and selecting "Add shortcut...". Instead of adding a shortcut, you can do all your file management from this dialog as whatever user is currently running Super Launcher.
There are plans to add a real file browser to Super Launcher for this exact reason, so stay tuned for that.

## Known limitations

* Super Launcher saves the shortcuts in the user's AppData folder. This means when running Super Launcher in a portable environment (for example: from a USB thumb drive), Super Launcher will not save the shortcuts in that environment. *See: [backing up shortcuts](#backing-up-shortcuts), for a guide on backing up the shortcuts.*