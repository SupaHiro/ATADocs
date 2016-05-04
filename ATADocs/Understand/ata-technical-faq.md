---
# required metadata

title: ATA technical FAQ | Microsoft Advanced Threat Analytics
description: Provides a list of frequently asked questions about ATA and the associated answers
keywords:
author: rkarlin
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-ata
ms.service: advanced-threat-analytics
ms.technology: security
ms.assetid: a7d378ec-68ed-4a7b-a0db-f5e439c3e852

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: bennyl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ATA technical FAQ
This article provides a list of frequently asked questions about ATA and provides insight and answers.


## What should I do if the ATA Gateway won’t start?
Look at the most recent error in the current error log (Where ATA is installed under the "Logs" folder).
## How can I test ATA?
You can simulate suspicious activities which is an end to end test by doing one of the following:

1.  DNS reconnaissance by using Nslookup.exe
2.  Remote execution by using psexec.exe


This needs to run remotely against the domain controller being monitored and not from the ATA Gateway.

## How do I verify Windows Event Forwarding?
You can run the following from a command prompt in the directory:  **\Program Files\Microsoft Advanced Threat Analytics\Center\MongoDB\bin**:

        mongo ATA --eval "printjson(db.getCollectionNames())" | find /C "NtlmEvents"`
## Does ATA work with encrypted traffic?
Encrypted traffic will not be analyzed (for example: LDAPS, IPSEC ESP).
## Does ATA work with Kerberos Armoring?
Enabling Kerberos Armoring, also known as Flexible Authentication Secure Tunneling (FAST), is supported by ATA, with the exception of over-pass the hash detection which will not work.
## How many ATA Gateways do I need?

First, it is recommended that you use ATA Lightweight Gateways on any domain controllers that can accommodate it; to determine this, see [ATA Capacity Planning](/advanced-threat-analytics/plan-design/ata-capacity-planning#ATA Lightweight Gateway Sizing). 

If all domain controllers can be covered by ATA Lightweight Gateways then no ATA Gateways are needed.

For any domain controllers that can't be covered by the ATA Lightweight Gateway, consider the following when deciding how many ATA Gateways you need:

 - The total amount of traffic your domain controllers produce, as well as the network architecture (in order to configure port-mirroring). To read more on how to determine how much traffic your domain controllers produce see [ATA Capacity Planning](/advanced-threat-analytics/plan-design/ata-capacity-planning#Domain controller traffic estimation).
 - The operational limitations of port mirroring also determine how many ATA Gateways you need to support your domain controllers, for example: per switch, per datacenter, per region - each environment has its own considerations. 

## How much storage do I need for ATA?
For every one full day with an average of 1000 packets/sec you need 0.3 GB of storage.<br /><br />For more information about ATA Center sizing see, [ATA Capacity Planning](/advanced-threat-analytics/plan-design/ata-capacity-planning).


## Why are certain accounts considered sensitive?
This happens when an account is a member of certain groups which we designate as sensitive (for example: "Domain Admins").

To understand why an account is sensitive you can review its group membership to understand which sensitive groups it belongs to (the group that it belongs to can also be sensitive due to another group, so the same process should be performed until you locate the highest level sensitive group).

## How do I monitor a virtual domain controller using ATA?
Most virtual domain controllers can be covered by the ATA Lightweight Gateway, to determine whether the ATA Lightweight Gateway is appropriate for your environment, see [ATA Capacity Planning](/advanced-threat-analytics/plan-design/ata-capacity-planning).

If a virtual domain controller can't be covered by the ATA Lightweight Gateway, you can have either a virtual or physical ATA Gateways as described in [Configure port mirroring](/advanced-threat-analytics/plan-design/configure-port-mirroring).  <br />The easiest way is to have a virtual ATA Gateway on every host where a virtual domain controller exists.<br />If your virtual domain controllers move between hosts, you need to perform one of the following:

-   When the virtual domain controller moves to another host, preconfigure the ATA Gateway in that host to receive the traffic from the recently moved virtual domain controller.
-   Make sure that you affiliate the virtual ATA Gateway with the virtual domain controller so that if it is moved, the ATA Gateway moves with it.
-   There are some virtual switches that can send traffic between hosts.

## How do I back up ATA?
There are 2 things to back up:

-   The traffic and events stored by ATA, which can be backed using any supported database backup procedure, for more information see [ATA database management](/advanced-threat-analytics/deploy-use/ata-database-management). 
-   The configuration of ATA, which is stored in the database and is automatically backed up every hour. 

## What can ATA detect?
ATA detects known malicious attacks and techniques, security issues, and risks.
For the full list of ATA detections, see [What is Microsoft Advanced Threat Analytics?](/advanced-threat-analytics/understand-explore/what-is-ata).

## What kind of storage do I need for ATA?
We recommend fast storage (7200 RPM disks are not recommended) with low latency disk access (less than 10 ms). The RAID configuration should support heavy write loads (RAID-5/6 and their derivatives are not recommended).

## How many NICs does the ATA Gateway require?
The ATA Gateway needs a minimum of two network adapters:<br>1. A NIC to connect to the internal network and the ATA Center<br>2. A NIC that will be used to capture the domain controller network traffic via port mirroring.<br>* This does not apply to the ATA Lightweight Gateway, which natively uses all of the network adapters that the domain controller uses.

## What kind of integration does ATA have with SIEMs?
ATA has a bi-directional integration with SIEMs as follows:

1. ATA can be configured to send a Syslog alert in the event of a suspicious activity to any SIEM server using the CEF format.
2. ATA can be configured to receive Syslog messages for each Windows event with the ID 4776, from [these SIEMs](/advanced-threat-analytics/plan-design/configure-event-collection#SIEMsupport).

## Can ATA monitor domain controllers visualized on your IaaS solution?

Yes, you can use the ATA Lightweight Gateway to monitor domain controllers that are in any IaaS solution.



## See Also
- [ATA prerequisites](/advanced-threat-analytics/plan-design/ata-prerequisites)
- [ATA capacity planning](/advanced-threat-analytics/plan-design/ata-capacity-planning)
- [Configure event collection](/advanced-threat-analytics/plan-design/configure-event-collection)
- [Configuring Windows event forwarding](/advanced-threat-analytics/plan-design/configure-event-collection#ATA_event_WEF)
- [Check out the ATA forum!](https://social.technet.microsoft.com/Forums/security/en-US/home?forum=mata)
