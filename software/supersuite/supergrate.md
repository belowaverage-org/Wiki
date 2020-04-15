---
title: Super Grate
description: 
published: true
date: 2020-04-15T03:03:58.787Z
tags: 
---

# Introduction
**Super Grate** is a tool part of the Super Suite that can perform remote execution of Microsoft's USMT (User State Migration Tool) on any domain joined PC, or local execution on any non-joined PC.

## Features
* Simple GUI
* Configurable
* Works on Domain Joined and Non-Joined PCs
* Unified Backup Store
* Works remotely over a network

# Installation / Upgrade

## Quick Start

1. Simply download from one of the following sources:
    * [below average](https://belowaverage.org/software/supergrate/download/)
    * [GitHub](https://github.com/belowaverage-org/SuperGrate/releases)
2. Run the installer, or run the portable version.
3. Done!

## Unified Store Setup

Super Grate fully supports **unification** of its various components, for example:

* Unified Backup Store
* Unified Configuration
* Unified USMT Configuration
* Unified Logging

Super Grate supports all of this by simply supporting [UNC](https://en.wikipedia.org/wiki/Path_(computing)#Universal_Naming_Convention) paths.

Try placing SuperGrate.exe on a shared folder and configuring its settings and paths to also point at a shared folder.
You can also set permissions on the folders to lock down what Super Grate users can do. (for instance: write protecting the **SuperGrate.xml**)


# Using Super Grate

## Backing up a User Profile
**Backing up a user profile** using Super Grate is "Super Easy".

1. Enter the computer's host-name in the "Source Computer" text box.
2. (Optional) Enter a destination computer in the "Destination Computer" text box to perform a Full Migration.
3. Press "List Source" to generate a list of User Profiles available on the Source Computer.
4. Select the User(s) you would like to back up.
5. Press "Start".

![backing-up-a-user-profile.png](/software/backing-up-a-user-profile.png)

## Restoring a User Profile

**Restoring a User Profile** is just as easy as Backing up a User Profile.

1. Enter a Computer's Host-name in the "Destination Computer" text box.
2. Press "List Store" to display a list of Profiles available in the Backup Store.
3. Select the Users you would like to restore to the "Destination Computer".
4. Press "Start".

![restoring-a-user-profile.png](/software/restoring-a-user-profile.png)

# Development

## Debugging Super Grate

While using Super Grate, you might encounter errors. You may post any errors you get to [here](https://github.com/belowaverage-org/SuperGrate/issues).

Below are some notes about logging:

* To display Verbose logs in the output console, double press anywhere in the "black" log area.
* To save all logs including Verbose logs to a file, press "CTRL + S" at the main screen.
* To automatically save log files to disk whenever Super Grate is used:
    1. Select "Settings".
    2. Double click "DumpLogHereOnExit".
    3. Enter ".\Logs" & Press "Apply"
    4. Press "Save to Disk" then press "Close"

# Further Reading

## Portable Version

Super Grate supports being **"Portable"** meaning that Super Grate can be run in a stand-alone environment, like off of a USB thumb drive, or external HDD drive.

All you need to do is download the Super Grate installer, and de-select all components in the installer except "Super Grate", then in the next slide select the location where SuperGrate.exe will be copied. From there, you can move SuperGrate.exe to wherever you want and run it normally.

Super Grate will download any necessary dependencies when they are needed.

A portable version of Super Grate allows the operator to back up profiles to a USB thumb drive or USB external HDD easier, and quicker. It also helps in scenarios where the computer you are trying to back up has no disk space available for scratch space.

## SuperGrate.xml

The **SuperGrate.xml** is a file that is automatically generated if it doesn't exist in the "current working directory". This file contains the preferences from the "Settings" menu.

Below is an example of the SuperGrate.xml file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<SuperGrate>
  <!--The UNC or Direct path to the USMT directory. (E.g: .\USMT\X64)-->
  <USMTPathX64>.\USMT\X64</USMTPathX64>
  <USMTPathX86>.\USMT\X86</USMTPathX86>
  <!--Local path on source computer where Super Grate will run USMT from. (E.g: C:\SuperGrate)-->
  <SuperGratePayloadPath>C:\SuperGrate</SuperGratePayloadPath>
  <!--The UNC or Direct path to the USMT Migration Store (E.g: \\ba-share\s$ or .\STORE)-->
  <MigrationStorePath>.\STORE</MigrationStorePath>
  <!--ScanState.exe & LoadState.exe CLI Parameters. See: https://docs.microsoft.com/en-us/windows/deployment/usmt/usmt-command-line-syntax -->
  <ScanStateParameters>/config:Config_SettingsOnly.xml /i:MigUser.xml /c /r:3 /o</ScanStateParameters>
  <LoadStateParameters>/config:Config_SettingsOnly.xml /i:MigUser.xml /c /r:3 /lac /lae</LoadStateParameters>
  <!--Delete the user from the migration store after a restore? (store to destination)-->
  <AutoDeleteFromStore>false</AutoDeleteFromStore>
  <!--Delete the user from the source computer after a backup? (source to store)-->
  <AutoDeleteFromSource>false</AutoDeleteFromSource>
  <!--Prevent NT AUTHORITY & NT SERVICE accounts from being listed?-->
  <HideBuiltInAccounts>true</HideBuiltInAccounts>
  <!--Prevent unknown accounts from being listed?-->
  <HideUnknownSIDs>false</HideUnknownSIDs>
  <!--Write log to disk on exit. (Leave blank to disable) (E.g: \\ba-share\s$\Logs or .\Logs)-->
  <DumpLogHereOnExit></DumpLogHereOnExit>
  <!--List of columns to display for the Source or Store users.-->
  <ULSourceColumns>0,3,9</ULSourceColumns>
  <ULStoreColumns>0,1,5,6,4</ULStoreColumns>
</SuperGrate>
```

# Known Issues



















