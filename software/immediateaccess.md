---
title: Immediate Access
description: Always on VPN Service
published: true
date: 2020-06-23T22:28:28.326Z
tags: 
editor: markdown
---

# Introduction
**Immediate Access** is a replacement for Microsoft's Direct Access "Always On VPN" technology. This service behaves in the same manner as Microsoft's Direct Access, but instead of connecting via IPHTTPS or Teredo, the service will start a VPN connection of your choice. (Must be a Windows VPN profile: IPSec, SSTP, L2TP, PPTP...)

This service -- *along with starting a VPN connection* -- will automatically close the VPN connection if the computer is on the corporate network.

# GPO Policies / Registry Settings

* GPO Path: `Computer Configuration\Administrative Templates\Network\Immediate Access`

* Registry Key: `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Below Average\Immediate Access`

## Internal Probe
* **Group Policy Setting:** Immediate Access
* **Registry Item:** InternalProbe (REG_SZ)
* **Value Type:** HTTPS URL
* **Default Value:** NULL

## Vpn Profile List
* **Group Policy Setting:** Immediate Access
* **Registry Item:** VpnProfileList (REG_MULTI_SZ)
* **Value Type:** Windows VPN Profile Names
* **Default Value:** NULL

## Probe Timeout
* **Group Policy Setting:** Probe Timeout
* **Registry Item:** ProbeTimeoutS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 10

## Network Event Cooldown
* **Group Policy Setting:** Network Event Cooldown Timer
* **Registry Item:** NetEventCooldownS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 3

## Health Check Interval
* **Group Policy Setting:** Health Check Interval
* **Registry Item:** HealthCheckIntervalS (REG_DWORD)
* **Value Type:** Seconds
* **Default Value:** 300

## VPN Server Connect Attempts
* **Group Policy Setting:** VPN Server Connect Attempts
* **Registry Item:** VpnServerConnectAttempts (REG_DWORD)
* **Value Type:** Milliseconds
* **Default Value:** 1500

## VPN Server Ping Timeout
* **Group Policy Setting:** VPN Server Ping Timeout
* **Registry Item:** VpnServerPingTimeoutMS (REG_DWORD)
* **Value Type:** Milliseconds
* **Default Value:** 1500