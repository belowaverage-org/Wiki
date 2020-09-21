---
title: Super Update
description: A customizable installer / update system for any software project.
published: true
date: 2020-09-21T00:44:30.693Z
tags: 
editor: markdown
dateCreated: 2020-08-30T19:40:53.398Z
---

<p align="center">
	<img width="100" src="/assets/software/supersuite/superupdate/logo.svg"/>
</p>
<h2 align="center">Super Update</h2>

<p align="center">
	<img src="/assets/software/supersuite/superupdate/mainpage.png"/>
</p>

> Super Update is an online update tool tailored to fit the needs of any software project. Super Update can be branded and extended using PowerShell.

# The basics

Super Update uses an XML file (whether it be located on the web or a local file) to determine how Super Update should behave and also to determine the updates available for installation. Once an update is determined, Super Update will launch a PowerShell script of your choice to perform the update.

# How does it work?

Below is the general flow of how Super Update performs an update. This chart does not account for every little condition and XML setting. These will be described elsewhere in the documentation.

<br><br>

![flowchart.svg](/assets/software/supersuite/superupdate/flowchart.svg)

# How to begin.

## Downloading Super Update

To begin, fist download the latest SuperUpdate executable from https://github.com/belowaverage-org/SuperUpdate/releases

## Determining your integration strategy

> Each one of these strategies should have an example documented.

There are many different ways to utilize Super Update with your project:

* __Stand Alone__: If you want an un-intrusive method of updating your application, you can simply just create a shortcut to the Super Update binary in the start menu under your application's folder. Or create a link or button inside your application that launches Super Update.

* __Referencing__: If you are building a .NET application, you may be able to reference the Super Update binary to check for updates in the background of your main application, and then prompt to install an update to the user by then executing the binary via your application.

* __Click Once__: If you want to check for an update each time your application is launched, what you can do is utilize the "AutoRun" setting node. Once the XML is set up in this fashion, simply use Super Update as the entrypoint to your application. When the user wants to open your program, they will really be opening Super Update, once Super Update launches, it will auto-run the latest update script, the PowerShell script will then perform the update (or not if there is no update) and then launch your program.


