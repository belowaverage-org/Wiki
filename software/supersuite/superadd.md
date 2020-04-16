---
title: Super ADD
description: 
published: true
date: 2020-04-16T17:26:12.863Z
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