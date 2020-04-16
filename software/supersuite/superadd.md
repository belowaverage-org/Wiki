---
title: Super ADD
description: 
published: true
date: 2020-04-16T17:28:31.474Z
tags: 
---

![superadd.ico](/assets/software/supersuite/superadd/superadd.ico)

# About

![superaddimage.png](/assets/software/supersuite/superadd/superaddimage.png)

Super ADD is a MDT (Microsoft Deployment Toolkit) extension that displays a GUI (Graphical User Interface) to a user running a LTI (Lite Touch Installation) that allows them to select / generate a computer name and description for Active Directory.

Super ADD also creates the computer accounts as soon as the user selects "Save" to prevent computer objects from being over-written from parallel instances of LTI.

# Installation

# Command Line

![help.png](/assets/software/supersuite/superadd/help.png)

# Resources

## SuperADD.xml

```xml

```

## ts.xml

```xml
<group expand="true" name="SuperADD" description="A Customizable UI for joining computers to a domain / workgroup." disable="false" continueOnError="false">
    <step type="SMS_TaskSequence_RunCommandLineAction" name="Copy SuperADD.xml" description="Copies the SuperADD.xml from the deployment share to the tools folder where SuperADD will start in." disable="false" continueOnError="false" startIn="" successCodeList="0 3010" runIn="WinPEandFullOS">
        <defaultVarList>
            <variable name="PackageID" property="PackageID" />
            <variable name="RunAsUser" property="RunAsUser">false</variable>
            <variable name="SMSTSRunCommandLineUserName" property="SMSTSRunCommandLineUserName"></variable>
            <variable name="SMSTSRunCommandLineUserPassword" property="SMSTSRunCommandLineUserPassword"></variable>
            <variable name="LoadProfile" property="LoadProfile">false</variable>
        </defaultVarList>
        <action>cmd.exe /c copy %DeployDrive%\SuperADD\SuperADD.xml SuperADD.xml</action>
    </step>
    <action />
    <step type="SMS_TaskSequence_RunCommandLineAction" name="SuperADD - WinPE" description="Launches SuperADD using BDDRun in Windows PE." disable="false" continueOnError="false" startIn="" successCodeList="0 3010" runIn="WinPEandFullOS">
        <defaultVarList>
            <variable name="PackageID" property="PackageID"></variable>
            <variable name="RunAsUser" property="RunAsUser">false</variable>
            <variable name="SMSTSRunCommandLineUserName" property="SMSTSRunCommandLineUserName"></variable>
            <variable name="SMSTSRunCommandLineUserPassword" property="SMSTSRunCommandLineUserPassword"></variable>
            <variable name="LoadProfile" property="LoadProfile">false</variable>
        </defaultVarList>
        <action>bddrun.exe %DeployDrive%\SuperADD\SuperADD.exe</action>
        <condition>
            <expression type="SMS_TaskSequence_VariableConditionExpression">
                <variable name="Variable">DeploymentType</variable>
                <variable name="Operator">equals</variable>
                <variable name="Value">NEWCOMPUTER</variable>
            </expression>
        </condition>
    </step>
    <step type="SMS_TaskSequence_RunCommandLineAction" name="SuperADD - Windows" description="Launches SuperADD in normal Windows where BDDRun is not required." disable="false" continueOnError="false" startIn="" successCodeList="0 3010" runIn="WinPEandFullOS">
        <defaultVarList>
            <variable name="PackageID" property="PackageID" />
            <variable name="RunAsUser" property="RunAsUser">false</variable>
            <variable name="SMSTSRunCommandLineUserName" property="SMSTSRunCommandLineUserName"></variable>
            <variable name="SMSTSRunCommandLineUserPassword" property="SMSTSRunCommandLineUserPassword"></variable>
            <variable name="LoadProfile" property="LoadProfile">false</variable>
        </defaultVarList>
        <action>%DeployDrive%\SuperADD\SuperADD.exe</action>
        <condition>
            <expression type="SMS_TaskSequence_VariableConditionExpression">
                <variable name="Variable">DeploymentType</variable>
                <variable name="Operator">notEquals</variable>
                <variable name="Value">NEWCOMPUTER</variable>
            </expression>
        </condition>
    </step>
</group>
```
