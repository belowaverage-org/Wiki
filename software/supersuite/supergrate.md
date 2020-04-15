---
title: Super Grate
description: 
published: true
date: 2020-04-15T04:15:55.193Z
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
2. Run the installer, or run the [portable version](#portable-version).
3. Done!

## Unified Store Setup

Super Grate fully supports **unification** of its various components, for example:

* Unified Backup Store
* Unified Configuration
* Unified USMT Configuration
* Unified Logging

Super Grate supports all of this by simply supporting [UNC](https://en.wikipedia.org/wiki/Path_(computing)#Universal_Naming_Convention) paths.

Try placing SuperGrate.exe on a shared folder and configuring its settings and paths to also point at a shared folder.
You can also set permissions on the folders to lock down what Super Grate users can do. (for instance: write protecting the [SuperGrate.xml](#supergratexml))

## 1.2.0.0 to 1.3.0.0 Upgrade Guide

#### Upgrading [SuperGrate.xml](#supergratexml).

1. Open the new Super Grate after replacing the old executable.
2. Navigate to "File", then "Settings..." or press the "F12" on your keyboard.
3. Press "Save to Disk" to add the new settings to your [SuperGrate.xml](#supergratexml)

#### Upgrading the [Backup Store](#backup-store).

1. Download the following PowerShell script from here: [store_upgrade_1.2.0.0_1.3.0.0.ps1](https://github.com/belowaverage-org/SuperGrate/blob/master/SuperGrateTools/store_upgrade_1.2.0.0_1.3.0.0.ps1).
2. Copy the script to your SuperGrate directory where the STORE folder is located. (Do not copy into the STORE folder).
3. Execute the .ps1 script, there should be no errors.

#### Conclusion

After running the steps above, Super Grate should run without a hitch.

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

## Command Line Switches

Super Grate supports configuration over **CLI** (Command Line Interface) *switches*. This means you can create custom shortcuts and launch Super Grate with specific settings.

Note: *CLI Switches override [SuperGrate.xml](#supergratexml) settings.*

To use a CLI switch, do the following:

`C:\>SuperGrate.exe -NameOfSetting:Value`

or:

`C:\>SuperGrate.exe -NameOfSetting:"Value"`

for example:

`C:\>SuperGrate.exe -HideBuiltInAccounts:"false"`

For a list of settings, see: [SuperGrate.xml](#supergratexml)

# Development

## Compiling Super Grate

**Compiling Super Grate** is easy to do; firstly, you need to download and install a copy of [Microsoft's Visual Studio (VS)](https://visualstudio.microsoft.com/vs/). -- *The community edition will do* -- Once done, clone this repository using VS' "Get Started" page. Lastly, open the solution and press the green "Start / Play" button. This will compile and start Super Grate.

To make a release build of Super Grate & create a NSIS installer, do the following:

1. Select "Batch Build" from the "Build" tab in the top menu bar.
2. Check "Release" & "Release_64" then press "Rebuild".
3. Right Click the Solution in the Solution Explorer, and select "Open in Explorer".
4. In explorer, navigate to the "SuperGrateInstaller" folder.
5. Run the "precompile.ps1" file in Power Shell (You may need to [set the ExecutionPolicy to Bypass mode](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6))
6. Download and install [NSIS](https://nsis.sourceforge.io).
7. Right click and select "Compile" on the "installer.nsi" file.
8. A file labled "SuperGrateInstaller.exe" should now exist in the "bin" folder.

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

## Backup Store

The backup **store** *or just "store" when using Super Grate* is a directory on a share drive or local drive that "stores" the user profiles after they have been backed-up. Weather you performed a [full migration](#full-migration) or just a simple [profile backup](#backing-up-a-user-profile), the data transferred is stored here.

The file structure of the store is as follows:

* **Base Folder** (Specified in the [SuperGrate.xml](#supergratexml) or Settings Menu)
    * **User Profile Folder** (Named a generic GUID, Can be any name)
        * **data**: A single file containing the backed-up profile.
        * **destination**: A file containing the name of the destination computer where the profile last was exported too.
        * **exportedby**: A file containing the name of the user who last exported the stored user profile.
        * **exportedon**: A file containing the date of when the profile was last exported in [windows file time](https://docs.microsoft.com/en-us/dotnet/api/system.datetimeoffset.tofiletime?view=netframework-4.8)
        * **importedby**: A file containing the name of the user who imported the user profile.
        * **importedon**: A file containing the date of when the profile was imported in windows file time.
        * **ntaccount**: A file containing the name of the user profile that was imported.
        * **sid**: A file containing the [SID](https://en.wikipedia.org/wiki/Security_Identifier) of the user profile that was imported.
        * **source**: A file containing the name of the source computer the profile originated from.

An example of the above may look like this below:

* **C:\Program Files\Super Suite\Super Grate\STORE** (Folder)
    * **bf31d643-27a7-4e5d-a4e5-277a4c5cc9d0** (Folder)
        * **data** (File | RAW Data)
        * **destination** (File | "DOMAIN\BA-PC02")
        * **exportedby** (File | "DOMAIN\Dylan.Bickerstaff")
        * **exportedon** (File | "132312220838407695")
        * **importedby** (File | "DOMAIN\Dylan.Bickerstaff")
        * **importedon** (File | "132312220838407695")
        * **ntaccount** (File | "DOMAIN\Administrator")
        * **sid** (File | "S-1-5-21-3623811015-3361044348-30300820-1013")
        * **source** (File | "DOMAIN\BA-PC01")

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

## Full Migration

A **Full Migration** is a Super Grate sequence where both the "Source Computer" and "Destination Computer" are occupied.
When the backup / migration is started, Super Grate will back up the user profiles selected from the "Source Computer" to the store, then connect to the "Destination Computer" and apply the user profiles selected from the "Source Computer".

There are options in the **SuperGrate.xml** and settings menu that can change the behavior presented above.

## How Super Grate performs a Migration

![migration-flowchart.svg](/software/migration-flowchart.svg)

# Known Issues

* Sleep Mode
    * Super Grate may fail if the remote PC or executing PC falls asleep.
* WMI
    * Super Grate may fail if WMI is corrupt or disabled on the remote PC.
    * To fix: `winmgmt /resetrepository`
* Remote Registry
    * Super Grate may fail if the Remote Registry service is stopped or disabled on the remote PC.
    

















