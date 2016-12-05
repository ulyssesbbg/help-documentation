---
layout: page
title: Customer 3.1.1 Point Release Notes
author: Leslie Lundquist
featured: true
dateAdded: December 16, 2016
tags: [release notes, 3.1.1]

---

### IBM Bluemix Private Cloud Customer

### Point Release 3.1.1

**December 16, 2016**


This 3.1.1 Point Release is created primarily to fix some bugs that were affecting a few of our customers. Here are the changes that may affect your customer experience:

#### Bug Fixes Related to Curated Images
 
 * **248875** CentOS 6.8 20160910 image: only 2.1G disk space is seen in VM while the flavor has 10G disk [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=248875]
 * **249988** Windows images assume that hardware clock is set to local timezone instead of UTC [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=249988]
 
#### Corrections to Customer-facing RBAC Documentation

 * **251310** [Glance_RBAC] `cloud_admin` can publicize image [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=251310]
 * **239943** [bbc238_RBAC] `member` is allowed to list project [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=239943]
 * **252259** [swift_RBAC] `member` could create container [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=252259]
 * **260704** [Nova_RBAC] `cloud_admin` is allowed to get host [https://jazzop27.rtp.raleigh.ibm.com:9443/ccm/web/projects/BlueBox%20Cloud#action=com.ibm.team.workitem.viewWorkItem&id=260704]
