---
title: Super Start
description: 
published: true
date: 2020-04-19T03:38:16.359Z
tags: 
---

# About
> Super Start is a program designed to [bootstrap](https://en.wikipedia.org/wiki/Bootstrapping) other programs like Point of Sale software or Kiosk software. Super Start acts as a fail safe in the event Kiosk or Point of Sale software crashes or is accidentally exited by showing a splash screen with a timer (giving the user an oportunity to enter an over-ride pin) then re-launching the Kiosk / Point of Sale software.
# Setup

A standard setup with Super Start can be as simple as adding the executable to the [windows startup folder](https://en.wikipedia.org/wiki/Windows_startup_process), or by configuring Super Start to launch as a shell application via [Group Poliy](https://en.wikipedia.org/wiki/Group_Policy) in a [Windows Active Directory Domain](https://en.wikipedia.org/wiki/Active_Directory). Below we will outline how to do the latter. Before begining, please familiarize yourself with the note and XML file below.

> To begin setting up Super Start, first you must run the program; doing so will generate a `SuperStart.xml` file. To exit Super Start with defaults applied, exit the CMD window that says "Start Process", press anywhere on the splash screen, and enter the pin `1234`.

## SuperStart.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<SuperStart>
  <!--The process that will be launched.-->
  <StartProcessFileName>C:\Windows\System32\cmd.exe</StartProcessFileName>
  <StartProcessWorkingDirectory>C:\</StartProcessWorkingDirectory>
  <StartProcessArguments>/K echo Start Process</StartProcessArguments>
  <!--What to do if the start process wont start. Choices: StartExitProcessAndClose, DoNothing, KeepTrying, Close-->
  <StartProcessFailBehavior>DoNothing</StartProcessFailBehavior>
  <!--The process that will be launched on exit.-->
  <ExitProcessFileName>C:\Windows\System32\cmd.exe</ExitProcessFileName>
  <ExitProcessWorkingDirectory>C:\</ExitProcessWorkingDirectory>
  <ExitProcessArguments>/K echo Exit Process</ExitProcessArguments>
  <!--The delay that the program will be launched with the first time.-->
  <StartDelay>1</StartDelay>
  <!--The delay that the program will be launched with the consecutive times.-->
  <RestartDelay>5</RestartDelay>
  <!--The image that will be shown full screen while the program is closed.-->
  <BackgroundImage>C:\Windows\Web\Screen\img100.jpg</BackgroundImage>
  <!--The message displayed when the PIN screen appears.-->
  <UnlockMessage>Enter Unlock PIN</UnlockMessage>
  <!--The PIN used to quit out after double clicking the background if the program is closed.-->
  <UnlockPin>1234</UnlockPin>
  <!--The amount of time in seconds to enter the PIN. (To disable timer, set to nothing or a string)-->
  <UnlockTimeout>10</UnlockTimeout>
</SuperStart>
```
## Group Policy

Below are the recommendations for setup via GPO (Group Policy). You can follow these however you would like.

> Set the following settings under the `User Configuration` section in the GPO object.

**GPO Settings to Enable:**

* Policies
    * Administrative Templates
        * System
            * **Custom User Interface**: Enabled: `C:\PathToSuperStart\SuperStart.exe`
            * Crl+Alt+Del
                * **Remove Change Password**: Enabled
                * **Remove Lock Computer**: Enabled
                * **Remove Task Manager**: Enabled
                * **Remove Logoff**: Enabled
* Preferences
    * Windows Settings
        * Files
            * **SuperStart.exe**
                * Source file(s): `\\SOMESERVER\PathToSuperStart\SuperStart.exe`
                * Destination File: `C:\PathToSuperStart\SuperStart.exe`
            * **SuperStart.xml**
                * Source file(s): `\\SOMESERVER\PathToSuperStart\SuperStart.xml`
                * Destination File: `C:\PathToSuperStart\SuperStart.xml`

> **Please note**: When using GPO to copy files, the `Action` field can be misleading. Using the action type `Update` does not update the files copied if they changed on the file share. Using the `Replace` option may not be ideal either since every time GPO applies updates, the files will be re-downloaded from the file share. Using the action type `Update` on `SuperStart.exe` and using action type `Replace` on the `SuperStart.xml` may be a better approch for your environment.

# Known Limitations

* Multiple Displays