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

#### Changes that are updates and upgrades



  Update Horizon logo and splash page to new IBM Bluemix branding
  Urban Code Deploy[UCD] Heat plug-in currency for 3.1.1


#### Bug Fixes Related to Curated Images
 
 * CentOS 6.8 20160910 image: only 2.1G disk space is seen in VM while the flavor has 10G disk 
 * Windows images assume that hardware clock is set to local timezone instead of UTC
 
#### Corrections to Customer-facing RBAC Documentation

 * [Glance_RBAC] `cloud_admin` can publicize image 
 * [bbc238_RBAC] `member` is allowed to list project
 * [Swift_RBAC] `member` could create container 
 * [Nova_RBAC] `cloud_admin` is allowed to get host
