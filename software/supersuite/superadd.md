---
title: Super ADD
description: 
published: true
date: 2021-09-18T13:44:05.612Z
tags: 
editor: markdown
dateCreated: 2020-04-16T17:23:47.795Z
---

![superadd.ico](/assets/software/supersuite/superadd/superadd.ico)

# About

![superaddimage.png](/assets/software/supersuite/superadd/superaddimage.png)

> Super ADD is a MDT (Microsoft Deployment Toolkit) extension that displays a GUI (Graphical User Interface) to a user running a LTI (Lite Touch Installation) that allows them to select / generate a computer name and description for Active Directory.
> 
> Super ADD also creates the computer accounts as soon as the user selects "Save" to prevent computer objects from being over-written from parallel instances of LTI.

# Installation

## Standard Installation  

1\. Include .NET Framework in WinPE image.

![net.jpg](/assets/software/supersuite/superadd/net.jpg)

2\. Add the following rules to CustomSettings.ini

```ini
SkipDomainMembership=YES
SkipComputerName=YES
JoinDomain=ad.contoso.com
JoinWorkgroup=WORKGROUP
```

![ini.jpg](/assets/software/supersuite/superadd/ini.jpg)

3\. "Update" the deployment share / rebuild the WinPE image.
4\. Insert the [ts.xml from below](#tsxml) to your desired task sequnce's ts.xml file.
5\. Copy SuperADD.exe to you deployment share in a folder at the root named "SuperADD". For example:

`%DeploymentShare%\SuperADD\SuperADD.exe`

6\. Configure the SuperADD.xml to your liking. If the SuperADD.xml does not exist, run SuperADD.exe once to generate it.

***Optional:***

*7\. Proceed to "Join domain at very end of LTI" if you wish to have LTI join the PC to the domain after LTI finishes.*


## Join domain at very end of LTI

1\. Download this [file](https://github.com/belowaverage-org/SuperADD/blob/master/Scripts/Start-Join.ps1).
2\. Place this file (**Start-Join.ps1**) in the SuperADD directory located on your deployment share.

`%DeploymentShare%\SuperADD\Start-Join.ps1`

3\. Paste the following XML into the ts.xml of your choosing after the SuperADD sequence group.

```xml
<group expand="true" name="SuperADD - Delay Domain Join" description="This group will decide weather or not to delay a domain join based on SuperADD's outcome." disable="false" continueOnError="false">
  <action />
  <condition></condition>
  <step type="SMS_TaskSequence_SetVariableAction" name="Set DelayJoinDomain" description="" disable="false" continueOnError="false" successCodeList="0 3010">
    <defaultVarList>
      <variable name="VariableName" property="VariableName">DelayJoinDomain</variable>
      <variable name="VariableValue" property="VariableValue">%JoinDomain%</variable>
    </defaultVarList>
    <action>cscript.exe "%SCRIPTROOT%\ZTISetVariable.wsf"</action>
  </step>
  <step type="SMS_TaskSequence_SetVariableAction" name="Clear JoinDomain" description="" disable="false" continueOnError="false" successCodeList="0 3010">
    <defaultVarList>
      <variable name="VariableName" property="VariableName">JoinDomain</variable>
      <variable name="VariableValue" property="VariableValue"></variable>
    </defaultVarList>
    <action>cscript.exe "%SCRIPTROOT%\ZTISetVariable.wsf"</action>
  </step>
</group>
```

4\. Paste the following XML into the ts.xml of your choosing as the last step in the task sequence.

```xml
<step type="BDD_RunPowerShellAction" name="SuperADD - Start Join &amp; Reboot" description="This item will wait for MDT to exit, start another process that clears all NET connections, joins the PC to the domain and re-boots the PC." disable="false" continueOnError="false" successCodeList="0 3010">
  <defaultVarList>
    <variable name="ScriptName" property="ScriptName">%DeployDrive%\SuperADD\Start-Join.ps1</variable>
    <variable name="Parameters" property="Parameters">-DomainName "%DelayJoinDomain%" -ComputerName "%OSDComputerName%" -UserName "%DomainAdmin%" -Password "%DomainAdminPassword%"</variable>
    <variable name="PackageID" property="PackageID"></variable>
  </defaultVarList>
  <action>cscript.exe "%SCRIPTROOT%\ZTIPowerShell.wsf</action>
  <condition></condition>
</step>
```

# Command Line

![help.png](/assets/software/supersuite/superadd/help.png)

# Resources

## SuperADD.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<SuperADD>
  <!--The hostname of the domain for authentication.-->
  <DomainName>ad.contoso.com</DomainName>
  <!--The hostname of the domain controller to connect to.-->
  <DomainController>condc1.ad.contoso.com</DomainController>
  <!--The search base for name generation / unique verification.-->
  <BaseDN>DC=ad,DC=contoso,DC=com</BaseDN>
  <OrganizationalUnits>
    <OrganizationalUnit>
      <!--The description of this OU that will appear in the UI.-->
      <Name>Computers</Name>
      <!--The OU DN where computer objects will be placed.-->
      <DistinguishedName>CN=Computers,DC=ad,DC=contoso,DC=com</DistinguishedName>
      <!--Auto Name Generating Patterns:-->
      <!--#: Increment with a lefthand padding (Append Mode). E.g: #### = 0001-->
      <!--+: Increment with a lefthand padding (Fill Mode). E.g: ++++ = 0001-->
      <!--*: Random Alpha Numeric Lowercase. E.g: **** = j3g2-->
      <!--$: Random Alpha Numeric Uppercase. E.g: $$$$ = J3G2-->
      <!--@: Random Alpha Numeric Both Case. E.g: @@@@ = J3g2-->
      <!--%: Random Numeric. E.g: %%%% = 5473-->
      <!--!: Random Alpha Lowecase. E.g: !!!! = jwkx-->
      <!--?: Random Alpha Uppercase. E.g: ???? = JWKX-->
      <!--~: Random Alpha Both Case. E.g: ~~~~ = JwKx-->
      <AutoName>CONTOSOPC####</AutoName>
      <!--The security groups that will be auto applied. (the DNs must exist below in 'SecurityGroups')-->
      <SecurityGroupsDNs>
        <SecurityGroupDN>CN=COMP_Wifi,OU=Groups,OU=Computers,DC=ad,DC=contoso,DC=com</SecurityGroupDN>
      </SecurityGroupsDNs>
    </OrganizationalUnit>
  </OrganizationalUnits>
  <SecurityGroups>
    <SecurityGroup>
      <!--The description of this Security Group that will appear in the UI.-->
      <Name>Computer Wifi Access</Name>
      <!--The distinguished name of the security group.-->
      <DistinguishedName>CN=COMP_Wifi,OU=Groups,OU=Computers,DC=ad,DC=contoso,DC=com</DistinguishedName>
    </SecurityGroup>
  </SecurityGroups>
  <DescriptionItems>
    <DescriptionItem>
      <Name>Department</Name>
      <Type>List</Type>
      <ListItems>
        <ListItem>Accounting</ListItem>
        <ListItem>Finance</ListItem>
        <ListItem>Human Resources</ListItem>
      </ListItems>
    </DescriptionItem>
    <DescriptionItem>
      <Name>Person / Location</Name>
      <Type>Text</Type>
    </DescriptionItem>
  </DescriptionItems>
</SuperADD>
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
