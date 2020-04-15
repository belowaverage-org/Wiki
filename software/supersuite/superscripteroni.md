---
title: Super Scripteroni
description: 
published: true
date: 2020-04-15T20:17:14.287Z
tags: 
---

<p align="center">
  <img height="150" src="/assets/software/supersuite/superscripteroni/superscripteroni.png">
</p>

---

### Super Scripteroni is a PowerShell script used to deploy software to domain joined computers via GPO. This script is meant to bypass the use of the built in software deployment GPO (Computer Configuration / Software Settings / Software installation). Super Scripteroni runs in the background via a scheduled task after Group Policies have applied. (Or whenever you configure it to run)

# How does Super Scripteroni Work?

## Super Scripteroni Folder Tree

![filetree.bmp](/assets/software/supersuite/superscripteroni/filetree.bmp)

## [SuperScripteroni.ps1](https://github.com/belowaverage-org/SuperScripteroni/blob/master/SuperScripteroni.ps1)

1. Group policies apply.
2. Scheduled task starts SuperScripteroni.ps1.
3. SuperScripteroni.ps1 searches the "Deploy" directory and sub-directories as shown above for any *.ps1 files.
4. SuperScripteroni.ps1 runs each *.ps1 file found in the directories they are found in. ([Working Directory](https://en.wikipedia.org/wiki/Working_directory))
5. SuperScripteroni.ps1 writes the PowerShell output to an event in the Applications event log.
    
Because of the way the SuperScripteroni.ps1 script works, each deployment script will need to perform the necessary checks to see if the application that is to be deployed already exists to prevent duplicate installs. See [ChromeInstall.ps1](https://github.com/belowaverage-org/SuperScripteroni/blob/master/ChromeInstall.ps1) for an example.

# How do I set up the Group Policy for Super Scripteroni?

## Adding the Super Scripteroni Base Script to a new GPO.

1. In a GPO of your choice, under Computer Configuration / Preferences / Windows Settings / Files, copy and paste the contents of [ScriptCopy.xml](https://github.com/belowaverage-org/SuperScripteroni/blob/master/ScriptCopy.xml) into the Group Policy Management Editor.
2. After pasted, edit the item to point where you want your SuperScripteroni instance to be installed.

![ss2.png](/assets/software/supersuite/superscripteroni/ss2.png)
![ss1.png](/assets/software/supersuite/superscripteroni/ss1.png)

## Adding the Super Scripteroni Scheduled Task to the new GPO.

In the new GPO you just created in the [first step](#adding-the-super-scripteroni-base-script-to-a-new-gpo), under Computer Configuration / Preferences / Control Panel Settings / Scheduled Tasks, copy and paste the contents of [ScheduledTaskGPO.xml](https://github.com/belowaverage-org/SuperScripteroni/blob/master/ScheduledTaskGPO.xml) into a text editor and edit the highlighted section shown in the picture below to point to where the SuperScripteroni.ps1 script is located, then copy the XML and paste into the GPO editor.

![scheduleedit.png](/assets/software/supersuite/superscripteroni/scheduleedit.png)

## Adding a deployment to Super Scripteroni.

1. In the SuperScripteroni GPO, under Computer Configuration / Preferences / Windows Settings / Files, copy and paste the contents of [StandardDeploymentGPO.xml](https://github.com/belowaverage-org/SuperScripteroni/blob/master/StandardDeploymentGPO.xml) into the Group Policy Management Editor.
2. After pasted, edit the file item to copy the contents of a deployment from a server to the client computer under the Deploy folder where your SuperScripteroni instance is located.

![gc.png](/assets/software/supersuite/superscripteroni/gc.png)
![gc1.png](/assets/software/supersuite/superscripteroni/gc1.png)

## Removing a deployment from Super Scripteroni.

When removing a deployment from a GPO you must remove the files policy for the deployment you are trying to remove and add a folder policy to remove the deployment to prevent the SuperScripteroni script from invoking the deployed script next time GPO is updated. For example:

![deletedeployment.png](/assets/software/supersuite/superscripteroni/deletedeployment.png)

## Targeting a deployment from Super Scripteroni.

Targeting a deployment from Super Scripteroni requires two additional items to be in place (2 file items and 1 folder item). The [first file item](https://github.com/belowaverage-org/SuperScripteroni/blob/master/TargetedDeploymentFilesGPO1.xml) will be set up the same as the items described in [Adding a deployment to Super Scripteroni.](https://github.com/belowaverage-org/SuperScripteroni#adding-a-deployment-to-super-scripteroni) The [second file item](https://github.com/belowaverage-org/SuperScripteroni/blob/master/TargetedDeploymentFilesGPO2.xml) will be a copy of the first item but set up in "Update" mode with "Apply once" disabled. The third [item (folder item)](https://github.com/belowaverage-org/SuperScripteroni/blob/master/TargetedDeploymentFoldersGPO.xml) will point to the targeted package on the local C: drive with the action set to "Delete" with the item level targeting set opposite to the first two items. For example:

### File Item 1
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/targeting1.png">
<h4>File Item 2</h4>
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/targeting2.png">
<h4>Folder Item</h4>
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/targeting3.png">
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/targeting4.png">
<p>When targeting, the reason there needs to be two file items and 1 folder item set up the way they are is to allow computers to move in and out of the targeting scope and update their deployments accordingly. The first file item initially adds and allows for updates of a deployment, file item 2 allows a computer object to re-obtain a deployment if it were to be deleted by the folder item GPO, and the folder item inverts the targeting to remove a deployment if it falls out of the targeting scope defined in the first two file items.</p>
<h3>Securing Super Scripteroni.</h3>
<p>Super Scripteroni runs as system (unless you edit the scheduled task otherwise); therefore, any scripts that are placed in the Deploy folder will run as system. To prevent normal users from creating their own scripts and placing them in the Deploy folder an additional policy needs to be enabled under: Computer Configuration / Policies / Windows Settings / Security Settings / File System</p>
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/gposecurity.png">
<p>Since both the scheduled task for Super Scripteroni and the GPO Client service run as system, the GPO needs to allow full control to system (already set by default when adding this folder) and deny normal users access to the folder (remove from the list).</p>
<h3>Updating a deployment for Super Scripteroni.</h3>
<p>If you change your deployment script (*.ps1) or change the files associated with the deployment, you must open the file item in group policy and navigate to the "Common" tab where you will uncheck "apply once and do not re-apply", press "Apply", then re-check "apply once and do not re-apply" and then press "Apply" once more. This will change the GUID on the file item and cause the GPO Service on the client computers to force re-download the files specified in the file item.</p>
<img src="https://raw.githubusercontent.com/belowaverage-org/SuperScripteroni/master/images/gc1.png">
<h1>How do I create a package for Super Scripteroni?</h1>
<p>To create a deployment, all that is needed is a script in a folder named however you like under the "Deploy" folder on a share and a file item in GPO (or more if using targeting). Here is an example for a deployment of LAPS:</p>
<a href="https://github.com/belowaverage-org/SuperScripteroni/blob/master/LapsInstallExample.ps1">install.ps1</a>
<img src="https://github.com/belowaverage-org/SuperScripteroni/blob/master/images/LapsEg.png">
