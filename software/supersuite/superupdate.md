---
title: Super Update
description: A customizable installer / update system for any software project.
published: true
date: 2020-09-21T01:05:50.351Z
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

## Download Super Update

To begin, fist download the latest SuperUpdate executable from https://github.com/belowaverage-org/SuperUpdate/releases

## Determine your integration strategy

> Each one of these strategies should have an example documented.

There are many different ways to utilize Super Update with your project:

* __Stand Alone__: If you want an un-intrusive method of updating your application, you can simply just create a shortcut to the Super Update binary in the start menu under your application's folder. Or create a link or button inside your application that launches Super Update.

* __Referencing__: If you are building a .NET application, you may be able to reference the Super Update binary to check for updates in the background of your main application, and then prompt to install an update to the user by then executing the binary via your application.

* __Click Once__: If you want to check for an update each time your application is launched, what you can do is utilize the "AutoRun" setting node. Once the XML is set up in this fashion, simply use Super Update as the entrypoint to your application. When the user wants to open your program, they will really be opening Super Update, once Super Update launches, it will auto-run the latest update script, the PowerShell script will then perform the update (or not if there is no update) and then launch your program.

## Determine how to host the update repository

After you determine how you are going to integrate Super Update into your application, you next need to determine how you are going to host the XML document(s), your application binaries, and the PowerShell scripts that perform the update.

Super Update can retreive the XML using a few different methods:

* __HTTP / HTTPS Server__: The XML can be hosted on a webserver of your choice (GitHub pages even works well). Using HTTPS is recommended since Super Update will check for valid certificates when downloading the XML.

* __SMB Share__: The XML can also be hosted on a Windows SMB Share on a private server in an organization.

* __Local__: The XML can also be available to Super Update locally if you have another proprietary way of getting the updated XMLs deployed to your users.

## Determine how to pass the XML path to Super Update

The last thing that needs to be addressed before continuing further is the method you choose to tell Super Update where the XML lives.

> The embedded method needs an automated powershell solution.

There are currently two methods to "passing" the XML path off to Super Update.

* __CLI Argument__: The CLI argument always takes precedence, simply pass the XML URI to Super Update when launching either from inside your application, a shortcut, or from the command line for testing / debugging.

* __Embedded__: 


