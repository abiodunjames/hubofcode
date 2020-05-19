---
title: How We Built an Intrusion Detection System on AWS using Open Source Tools
date: 2017-11-06 07:22:42 Z
categories:
- Security
tags:
- aws
- ids
- nsm
- security
layout: post
comments: true
author: Samuel
---

It’s roughly a year now that we built an intrusion detection system on AWS cloud infrastructure that provides security intelligence across some selected instances using open source technologies.

As more instances were spun, real-time security monitoring became necessary. We wanted the capability to detect when someone attempts an SQL injection, an SSH brute force, a port scan and so on. I forgot; we didn’t even want a ping request to go unnoticed if it was possible to ping any of the instances from the public and finally, centralize security logs from multiple EC2 instances which would then be visualized with Kibana.

AWS supports third-party IDS/IPS tools like [Trend Micro](about:invalid#zSoyz), [Alert logic](https://www.alertlogic.com) to name a few, which are pretty good, however, we wanted to try and explore the possibility of getting close to what they offer using open source tools available at our disposal. Some of the best IDS and HIDS available (Snort, Suricata, Ossec) are open source and are actively supported by a large [community](https://snort.org/community). We could install them separately on each EC2 instance, this would have defeated our aim of having a centralized log of all security events and also would bring some maintainability issues.

### Implementation

[Security onion](https://securityonion.net/) has been around for a while, a project started by Doug Burks and finds good use in monitoring home network but its usage in the cloud is an area that has not been fully explored. It’s a Linux distro based on Ubuntu and comes with [Snort](http://snort.org/), [Suricata](https://suricata-ids.org/), [Bro](https://www.bro.org/), [OSSEC](http://ossec.github.io/), Sguil, [Squert](http://www.squertproject.org/), [ELSA](https://github.com/mcholste/elsa), Xplico, NetworkMiner. In short, it’s bundled with all the tools one would need for a powerful and free network monitoring system. I will not dwell on how to setup security onion because of already existing and compressive documentation which can be found on [security onion wiki page](https://github.com/Security-Onion-Solutions/security-onion/wiki).

Having read SANS paper titled [Logging and Monitoring to Detect Intrusions and Compliance Violation](https://www.sans.org/reading-room/whitepapers/detection/logging-monitoring-detect-network-intrusions-compliance-violations-environment-33985) with [Security Onion](https://securityonion.net/), we decided to try it out by selecting a VPC with 2 subnets and some few EC2 instances. Basically, these instances power public-facing websites, and often witness low to moderate traffic a day.

Since security onion works by analyzing traffic and logs on the host machine, in other to get analysis for an instance, one must first find a way to mirror traffic from all the instances to security onion sensor.

AWS does not provide a tap or span port, at least none that I know of. So, we made use of [*netsniff-ng*](http://netsniff-ng.org/) for the virtual tap which copies traffic from the instance to an OpenVPN bridge and transports the traffic to security onion sensor where it is then analyzed.

On each instance there is an OSSEC agent and a virtual tap. The purpose of OSSEC agent is to provide host-instrusion detection system (HIDS) that is, monitors events happening at the host level and reports back to the security onion server via the OSSEC encrypted message protocol, while the virtual tap mirrors traffic at the interface level and forwards that via an open VPN bridge to security onion server for analysis, serving as network-intrusion detection system (NIDS).

![](http://res.cloudinary.com/samueljames/image/upload/v1524270406/New-VPC-with-Public-and-Private-Subnets1_mkd26h.png)

 

### Reporting

Security Onion does not only support analyst tools like [squert](http://www.squertproject.org/), [squil](https://bammv.github.io/sguil/index.html),[elsa, ](https://github.com/mcholste/elsa)that can be used to access realtime events, session data, and raw packet captures but also [ELK as at the time of writing this post](http://blog.securityonion.net/2017/09/elastic-stack-alpha-release-and.html).

 

![](https://cdn-images-1.medium.com/max/1000/1*dtWDTta-RHTkw-kbu7u-Zw.png)

> Note: This post was also posted on[ medium ](https://medium.com/@samuelabiodun/how-we-built-an-intrusion-detection-system-on-aws-using-open-source-tools-8b755e965d54)by me

 
