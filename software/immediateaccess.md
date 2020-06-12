---
title: Immediate Access
description: Always on VPN Service
published: true
date: 2020-06-12T04:20:03.253Z
tags: 
editor: markdown
---

# GPO Policies / Registry Settings

**GPO Path:** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Below Average\Immediate Access`
**Registry Key:** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Below Average\Immediate Access`

## Internal Probe
**Group Policy Setting:** Immediate Access
**Registry Item:** InternalProbe (REG_SZ)
**Value Type:** HTTPS URL
**Default Value:** NULL

## Vpn Profile List
**Group Policy Setting:** Immediate Access
**Registry Item:** VpnProfileList (REG_MULTI_SZ)
**Value Type:** Windows VPN Profile Names
**Default Value:** NULL

## Probe Timeout
**Group Policy Setting:** Probe Timeout
**Registry Item:** ProbeTimeoutS (REG_DWORD)
**Value Type:** Seconds
**Default Value:** 10

## Network Event Cooldown
**Group Policy Setting:** Network Event Cooldown Timer
**Registry Item:** NetEventCooldownS (REG_DWORD)
**Value Type:** Seconds
**Default Value:** 3

## Health Check Interval
**Group Policy Setting:** Health Check Interval
**Registry Item:** HealthCheckIntervalS (REG_DWORD)
**Value Type:** Seconds
**Default Value:** 300

## VPN Server Ping Timeout
**Group Policy Setting:** VPN Server Ping Timeout
**Registry Item:** VpnServerPingTimeoutMS (REG_DWORD)
**Value Type:** Milliseconds
**Default Value:** 1500