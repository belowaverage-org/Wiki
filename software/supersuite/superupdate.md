---
title: Super Update
description: A customizable installer / update system for any software project.
published: true
date: 2020-09-21T23:51:00.053Z
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

## Integration methods

> Each one of these strategies should have an example documented.

There are many different ways to utilize Super Update with your project:

* __Stand Alone__: If you want an un-intrusive method of updating your application, you can simply just create a shortcut to the Super Update binary in the start menu under your application's folder. Or create a link or button inside your application that launches Super Update.

* __Referencing__: If you are building a .NET application, you may be able to reference the Super Update binary to check for updates in the background of your main application, and then prompt to install an update to the user by then executing the binary via your application.

* __Click Once__: If you want to check for an update each time your application is launched, what you can do is utilize the "AutoRun" setting node. Once the XML is set up in this fashion, simply use Super Update as the entrypoint to your application. When the user wants to open your program, they will really be opening Super Update, once Super Update launches, it will auto-run the latest update script, the PowerShell script will then perform the update (or not if there is no update) and then launch your program.

## Hosting methods for updates

This section goes over the different methods of hosting the XML document(s), your application binaries, and the PowerShell scripts that perform the update.

Super Update can retreive the XML using a few different methods:

* __HTTP / HTTPS Server__: The XML can be hosted on a webserver of your choice (GitHub pages even works well). Using HTTPS is recommended since Super Update will check for valid certificates when downloading the XML.

* __SMB Share__: The XML can also be hosted on a Windows SMB Share on a private server in an organization.

* __Local__: The XML can also be available to Super Update locally if you have another proprietary way of getting the updated XMLs deployed to your users.

## Passing the XML URI to Super Update

This section goes over the different methods of passing the XML URI to Super Update when it is launched.

> The embedded method needs an automated powershell solution.

There are currently two methods to "passing" the XML path off to Super Update.

* __CLI Argument__: The CLI argument always takes precedence, simply pass the XML URI to Super Update when launching either from inside your application, a shortcut, or from the command line for testing / debugging. (You can even drag and drop an XML file on-top of the Super Update binary to launch it using the XML)

* __Embedded__: If no XML is passed via a CLI argument, Super Update will read its own binary to see if there is a blank line and then the XML URI (in UTF8) at the end. This method works great when trying to use Super Update as a "Click Once" installer. Or any "initial" installation of your project / application.

## PowerShell techniques

What makes Super Update so powerful is the fact that the update scripts themselves are simply PowerShell scripts. Super Update is very flexible on how you build your scripts and reference them in the XML.

Below are the two methods Super Update supports for referencing your PowerShell scripts:

* __Individual Scripts__: With this method, each "Update node" in the XML references a different PowerShell script. This allows you to write simple scripts that just work.

* __A Single Script__: With this method, each "Update node" in the XML references the same PowerShell script. The script will look at the data passed by Super Update and perform the update using its own logic.

# Creating the XML

## Example XML

```xml
  
<?xml version="1.0" encoding="utf-8"?>
<SuperUpdate
    UpdaterVersion="0"
    xmlns="http://belowaverage.org/schemas/superupdate/0.0.0.2"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://belowaverage.org/schemas/superupdate/0.0.0.2 https://raw.githubusercontent.com/belowaverage-org/SuperUpdate/master/SuperUpdate/UpdateSchema.xsd"
>
  <Settings>
    <Redirect RedirectURL="https://static.belowaverage.org/software/SuperUpdate/SuperUpdate/1.1.1.1/Update.xml" UpdaterVersion="0" />
    <WindowSize Size="100x100"/>
    <WindowIcon URL="https://asdf.com/icon.ico" />
    <WindowIconLarge URL="https://asdf.com/icon.ico" />
    <WindowIconLargeAnimated URL="https://asdf.com/icon.gif" />
    <MessageNoUpdate Text="You are already up to date!"/>
    <RequireElevation Value="false" />
    <AutoRun Value="false" />
  </Settings>
  <Updates>
    <Update Version="1.1.1.1" DateTime="2000-01-01T00:00:00" Channel="Release" ReleaseInfoURL="https://github.com/belowaverage-org/SuperUpdate/releases/tag/1.1.1.1" ScriptURL="https://static.belowaverage.org/software/SuperUpdate/SuperUpdate/1.1.1.1/Update.ps1" UpdateMessage="2019 Update Available!">
      <File SHA1="0DDD90457B7FC4F3A429665F1271E31E515E6A75" Path=".\TestPayload.txt" />
    </Update>
  </Updates>
</SuperUpdate>
```

## XML Schema

The XML schema file is publicly located on the internet here: 
`https://raw.githubusercontent.com/belowaverage-org/SuperUpdate/master/SuperUpdate/UpdateSchema.xsd`

### Settings Node

#### Redirect

This 
    
    