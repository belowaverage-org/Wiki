---
title: Immediate Access
description: Always on VPN Service
published: true
date: 2020-06-24T03:48:33.229Z
tags: 
editor: markdown
---

<p align="center">
	<img src="/assets/software/immediateaccess/logo.svg" width="75">
</p>

# Introduction
**Immediate Access** is a replacement for Microsoft's Direct Access "Always On VPN" technology. This service behaves in the same manner as Microsoft's Direct Access, but instead of connecting via IPHTTPS or Teredo, the service will start a VPN connection of your choice. (Must be a Windows VPN profile: IPSec, SSTP, L2TP, PPTP...)

This service -- *along with starting a VPN connection* -- will automatically close the VPN connection if the computer is on the corporate network.

## How does Immediate Access work?

In a nutshell, this service will check if the current computer is on the corporate network by attempting to reach an internal only "probe". The probe is a web server that responds via HTTPS with any content (a blank page is best). The HTTPS site needs to present a trusted certificate to the client, or the Immediate Access service will not except the probe attempt. If the probe attempt is successfull, the Immediate Access service will sleep until the next "network change event" (aka: an IP address changes) or until the [Health Check Interval](#health-check-interval) lapses.

After an event or the Health Check Interval lapses, the Immediate Access service will re-check for a probe connection.

If the Immediate Access service cannot reach the probe, the service will connect to the GPO specified VPN profiles. If Immediate Access can once again reach the probe, the service will disconnect the GPO specified VPN profiles.

# Setup

## Download

https://github.com/belowaverage-org/ImmediateAccess/releases

## Install Service

Simply run the MSI install file, and the service should immediately start.
If you wish to push this installer via GPO, create a GPO policy and add the MSI to the software installation section of GPO. You can find more information on how to do that here:

https://support.microsoft.com/en-us/help/816102/

## Setting up the Internal Probe

The internal probe is a vital part of how Immediate Access works. The internal probe is simply a web server that is available only from the internal network, and has a valid HTTPS certificate.

Here is an article on how to set up a web server using Windows:

https://docs.microsoft.com/en-us/iis/install/installing-iis-85/installing-iis-85-on-windows-server-2012-r2

Once the web server is installed, make sure you create an HTTPS binding with a valid certificate:

https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis

## Install GPO Templates

By default, when installing via the MSI file, GPO templates are installed on the local template store located at `C:\Windows\PolicyDefinitions`.

If you wish to install these templates on the domain wide policy store copy the following files from:

* `C:\Windows\PolicyDefinitions\ImmediateAccess.admx`
* `C:\Windows\PolicyDefinitions\en-US\ImmediateAccess.adml`

to:

* `\\contoso-dc1\SYSVOL\ad.contoso.com\Policies\PolicyDefinitions\*`

# GPO Policies / Registry Settings

* GPO Path: `Computer Configuration\Administrative Templates\Network\Immediate Access`

* Registry Key: `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Below Average\Immediate Access`

## Internal Probe

The **Internal Probe** is the HTTPS enabled URL that the Immediate Access VPN service will use to determine if the comptuer is currently connected to the corporate network. The probe must only be accessible within the corporate network and have a valid certificate.

* **Group Policy Setting:** Immediate Access
* **Registry Item:** InternalProbe (REG_SZ)
* **Value Type:** HTTPS URL
* **Default Value:** NULL

## Vpn Profile List

The **Vpn Profile List** is a list of VPN profiles that Immediate Access VPN service will dial when the Internal Probe is not available. Seperate each entry by a return. (Enter in order of most priority to least priority)

* **Group Policy Setting:** Immediate Access
* **Registry Item:** VpnProfileList (REG_MULTI_SZ)
* **Value Type:** Windows VPN Profile Names
* **Default Value:** NULL

## Probe Timeout

The **Probe Timeout** is a time in seconds that the Immediate Access service will timeout when trying to reach the internal probe.

* **Group Policy Setting:** Probe Timeout
* **Registry Item:** ProbeTimeoutS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 10

## Network Event Cooldown

After a network event, Immediate Access will start testing the network to see if the Internal Probe is available.
This option will create a delay between when Immediate Access will start the probe test, and the last network change event.

* **Group Policy Setting:** Network Event Cooldown Timer
* **Registry Item:** NetEventCooldownS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 3

## Health Check Interval

The **Health Check Interval** is a time in seconds that the Immediate Access service will test for the Internal Probe on an interval.

* **Group Policy Setting:** Health Check Interval
* **Registry Item:** HealthCheckIntervalS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 300

## VPN Server Connect Attempts

This settings changes the ammount of attempts Immediate Access will make to connect to a VPN profile.

* **Group Policy Setting:** VPN Server Connect Attempts
* **Registry Item:** VpnServerConnectAttempts (REG_DWORD)
* **Value Type:** Count
* **Default Value:** 3

## VPN Server Ping Timeout

This setting represents a timeout in milliseconds for when Immediate Access pings a VPN server.

* **Group Policy Setting:** VPN Server Ping Timeout
* **Registry Item:** VpnServerPingTimeoutMS (REG_DWORD)
* **Value Type:** Milliseconds
* **Default Value:** 1500