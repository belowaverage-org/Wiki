---
title: Super Update
description: A customizable installer / update system for any software project.
published: true
date: 2020-09-27T19:05:34.998Z
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

There are currently two methods to "passing" the XML path off to Super Update.

* __CLI Argument__: The CLI argument always takes precedence, simply pass the XML URI to Super Update when launching either from inside your application, a shortcut, or from the command line for testing / debugging. (You can even drag and drop an XML file on-top of the Super Update binary to launch it using the XML)

* __Embedded__: If no XML is passed via a CLI argument, Super Update will read its own binary to see if there is a blank line and then the XML URI (in UTF8) at the end. This method works great when trying to use Super Update as a "Click Once" installer. Or any "initial" installation of your project / application.

### How to build an embedded EXE

To build one of these "Embedded EXE" files, download SuperUpdate.exe from above, and run this PowerShell command. Make sure you replace the URL below with your URL for your project's "Update XML", and make sure to keep the `` `n`n `` at the begining of the URL: 

```PowerShell
"`n`nhttp://contsoso.com/update.xml" | Out-File -Append -NoNewline -Encoding utf8 -FilePath ".\SuperUpdate.exe"
```

## PowerShell techniques

What makes Super Update so powerful is the fact that the update scripts themselves are simply PowerShell scripts. Super Update is very flexible on how you build your scripts and reference them in the XML.

Below are the two methods Super Update supports for referencing your PowerShell scripts:

* __Individual Scripts__: With this method, each "Update node" in the XML references a different PowerShell script. This allows you to write simple scripts that just work.

* __A Single Script__: With this method, each "Update node" in the XML references the same PowerShell script. The script will look at the data passed by Super Update and perform the update using its own logic.

### Individual Scripts: Example

In this example, the XML should be referencing seperate PowerShell scripts, each written in a simple manner to download and then install a file. Or something...

```PowerShell
Write-Information "Downloading Super Grate v1.0.0.0..."
Write-Verbose "Download Location: https://static.belowaverage.org/software/SuperGrate/Updates/Versions/1.0.0.0/SuperGrate.exe"
Invoke-WebRequest -Uri "https://static.belowaverage.org/software/SuperGrate/Updates/Versions/1.0.0.0/SuperGrate.exe" -OutFile ".\SuperGrate.exe"
```

### A Single Script: Example

In this very basic example, a single script is responsible for copying a file from the web to the local disk. And then prompts the user if they would like to execute the downloaded file.

```PowerShell
try {
    if (-not ($SuperUpdate.SelectedVersion -eq $SuperUpdate.CurrentVersion)) {
        Write-Information "Downloading Super Grate..."
        Write-Verbose "Download Location: https://static.belowaverage.org/software/SuperGrate/Updates/Versions/$($SuperUpdate.SelectedVersion.Version)/SuperGrate.exe"
        Invoke-WebRequest -Uri "https://static.belowaverage.org/software/SuperGrate/Updates/Versions/$($SuperUpdate.SelectedVersion.Version)/SuperGrate.exe" -OutFile ".\SuperGrate.exe"
    }
    Write-Information "Version: $($SuperUpdate.SelectedVersion.Version) is now installed!"
} catch {
    Write-Error -Exception $_.Exception
    Write-Information "An error occurred during installation."
}
$SuperUpdate.WindowVisible = $false
$result = [System.Windows.Forms.MessageBox]::Show("Would you like to start Super Grate?", "All Done!", [System.Windows.Forms.MessageBoxButtons]::YesNo)
if ($result -eq [System.Windows.Forms.DialogResult]::Yes) {
    $SuperUpdate.CloseWindowWhenDone = $true
    Start-Process ".\SuperGrate.exe"
    return
}
$SuperUpdate.WindowVisible = $true
```

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

## XML Schema Location

The XML schema file is publicly located on the internet here: 
`https://raw.githubusercontent.com/belowaverage-org/SuperUpdate/master/SuperUpdate/UpdateSchema.xsd`

## XML Settings Node

This section outlines what each setting does under the "Settings" node in the XML.

### Redirect

This settings will tell Super Update to read the specified file (URI) for more settings and updates.

*This setting only accepts URI values.*
    
### WindowSize

This setting tells Super Update what to make the main window size.

This setting accepts the following formats and values:

* "*width***x***height*" in pixels, for example: "**500x500**"
* "**Expanded**" which tells translates to: "800x500"
* "**Contracted**" which is default.

### WindowIcon, WindowIconLarge, WindowIconLargeAnimated

These settings provide themeing for the Super Update window. Each of these settings only accepts a URI to an icon file, except for "WindowIconLargeAnimated" which accepts PNG, JPG, or GIF.

### MessageNoUpdate

This settings specifies the text that is displayed to the user when there is no update available.

### RequireElevation

If the value of this setting is true, Super Update will ensure that it is running in an elevated state as an administrator. If Super Update is not "elevated", Super Update will request elevation from the OS and re-launch in an elevated state (using the same CLI parameters that started the original process).

### AutoRun

If the value of this setting is true, and the process of discovering updates has finished, Super Update will start the installation (or re-installation) the latest update automatically.

## XML Updates Node

This section outlines what each element and attribute (Element: Attribute) mean and do under the "Updates" node.

### Update

This element defines a software update version. See below how it is configured.

### Update: Version

This attribute can take any text value. This value is displayed to the user in the "Version" column in the Super Update interface. It is purely visual, and does not affect any internal logic.

### Update: DateTime

This attribute specifies the date and time the software version was released. This value only accepts a date and time in this format: [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601).

This value is displayed to the user in the "Release Date" column in the Super Update interface. it is purely visual, and does not affect any internal logic.

### Update: Channel

This attribute specifies which "software branch" or "development channel" this update belongs to, for example you might specify "Beta", "Alpha", "PreRelease", "Release","LTS" or more...
This value is displayed to the user in the Update Selection menu and groups updates together based on the value of their "Channel"

This value also affects how Super Update chooses the next available update. If Super Update determines that the current software version or "Update" is in a particular branch, Super Update will only pick the latest update in that "Channel". This is done to keep normal "Release" users in the "Release" branch, and beta testers in the "Beta" channel, or whatever Channel names are specified.

### Update: ReleaseInfoURL

This attribute specifies a URI to a website containing the release info on the particular update.

This value is displayed to the user in the "Release Notes" column in the Super Update interface. it is purely visual, and does not affect any internal logic.

### Update: ScriptURL

# PowerShell

When the installation script is ran from SuperUpdate, there is a hook that is injected into the script that you can access to perform actions in SuperUpdate, or access update information.

The hook is located in the global variable: ` $SuperUpdate `.
Here is the source for the hook: [Classes/PSRunspace.cs](https://github.com/belowaverage-org/SuperUpdate/blob/master/SuperUpdate/Classes/PSRunspace.cs)
Inside this variable are a few methods and properties listed below:

## Properties

### WindowState

Get / set the window state of the main window.

* ` {get; set;} `
* **Returns**: [System.Windows.Forms.FormWindowState](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.formwindowstate)

### WindowText

Get / set the title of the main window.

* ` {get; set;} `
* **Returns**: String

### WindowVisible

Gets / sets the visibility status of the main window. Setting this false will hide the main window.

* ` {get; set;} `
* **Returns**: Boolean

### WindowExpanded

Gets / sets the expansion status of the main window. True means that the main window is expanded.

* ` {get; set;} `
* **Returns**: Boolean

### CloseWindowWhenDone

Get / set this variable to specify whether or not to close SuperUpdate once the PowerShell script finishes.

* ` {get; set;} `
* **Returns**: Boolean

### RelaunchWhenDone

Get / set this variable to specify whether or not to re-launch SuperUpdate once the PowerShell script finishes.

* ` {get; set;} `
* **Returns**: Boolean

### Elevated

Gets / sets the elevation status of SuperUpdate. Setting this true will immediatly elevate the process.

* ` {get; set;} `
* **Returns**: Boolean

### SuperUpdateArguments

Gets / sets the SuperUpdateArguments that were passed to SuperUpdate on launch.

* ` {get; set;} `
* **Returns**: String[]

### ScriptArguments

Gets the arguments passed to the PowerShell script via the update XML node.

* ` {get;} `
* **Returns**: String[]

### CurrentVersion

Gets the currently detected version of the software to be updated.

* ` {get;} `
* **Returns**: [System.Xml.XmlNode](https://docs.microsoft.com/en-us/dotnet/api/system.xml.xmlnode)

### LatestVersion

Gets the latest possible version of the software to be updated.

* ` {get;} `
* **Returns**: [System.Xml.XmlNode](https://docs.microsoft.com/en-us/dotnet/api/system.xml.xmlnode)

### SelectedVersion

Gets the selected update version that the user selected on the updates screen.

* ` {get;} `
* **Returns**: [System.Xml.XmlNode](https://docs.microsoft.com/en-us/dotnet/api/system.xml.xmlnode)

### LogItems

Gets a list of log items in current log.

* ` {get;} `
* **Returns**: [System.Collections.Generic.List](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1)<[Logger.LogItems](https://github.com/belowaverage-org/SuperUpdate/blob/b3e7e64a8a67eeb4cb10b2b4a5b232d109124a46/SuperUpdate/Classes/Logger.cs#L218-L247)>

### SuperUpdateVersion

Gets the SuperUpdate program version.

* ` {get;} `
* **Returns**: [System.Version](https://docs.microsoft.com/en-us/dotnet/api/system.version)

## Methods

### Relaunch

This method re-launches the program with the SuperUpdateArguments.

* `void ReLaunch()`
* **Parameters**
	* None
* **Returns**
	* Void
