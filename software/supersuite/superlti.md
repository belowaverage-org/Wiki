---
title: Super LTI
description: 
published: true
date: 2020-04-17T01:48:17.244Z
tags: 
---

# About

Super LTI is a one click install wrapper powered by PowerShell.

## How does it work?

1. SuperLTI launches.
2. SuperLTI self elevates with administrator privileges.
3. SuperLTI looks in the current working directory for a file named `SuperLTI.zip`.
4. SuperLTI then extracts that ZIP to `C:\SuperLTI\*`
5. SuperLTI changes the current working directory to `C:\SuperLTI\`
6. SuperLTI then runs a PowerShell script located at `C:\SuperLTI\SuperLTI.ps1`
7. The PowerShell script then runs an install file, or copies a file or whatever.
8. The PowerShell script can run the `Write-Progress` cmdlet to update SuperLTI's GUI with new information.
9. The PowerShell script ends.
10. SuperLTI removes the `C:\SuperLTI\` directory.
11. SuperLTI quits.

## Why?

Because MDT (Microsoft Deployment Toolkit) sucks when working with powershell. And random scripts all over the Deployment Share's software folder can begin to get messy. This makes it hard to determine what PowerShell files need to be run and if they need elevation or not.

# Creating a SuperLTI package.

1\. Download [SuperLTI.exe](https://github.com/belowaverage-org/SuperLTI/releases)
2\. Create a PS1 file named `SuperLTI.ps1`
3\. Create a script that does a thing like so:

*Example:*

![1.png](/assets/software/supersuite/superlti/1.png)

*Tip: Use the "Write-Progress" function as shown above to display a progress indicator in the SuperLTI window.*

4\. Test the script then copy the files in your package into a ZIP file named `SuperLTI.zip`

*Example:*

![2.png](/assets/software/supersuite/superlti/2.png)

5\. Test the package as a whole by running the `SuperLTI.exe`. *Make sure both the SuperLTI.exe and SuperLTI.zip files are in the same working directory.*

*Demonstration:*

![superlti.gif](/assets/software/supersuite/superlti/superlti.gif)