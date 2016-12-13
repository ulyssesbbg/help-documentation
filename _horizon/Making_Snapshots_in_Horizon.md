---
layout: page
title: Using Horizon for Snapshots
author: Leslie Lundquist
dateAdded: December 12, 2016
tags: [Horizon, snapshots, OpenStack, Cinder, image]
featured: true
weight: 4
---

When running either the 3.0 or 3.1 release using Horizon, snapshot errors can be occur if there is not sufficient space to store volumes.  

**Best Practice:**
 When using Horizon it is best to detach the data volume, perform the instance snapshot, and then reattach the data volume.

Here’s why:

**In Horizon:** When performing an instance snapshot operation (**Instance/Create Snaphot**) using Horizon, be sure that there is enough storage capacity for all the attached volumes. If there is not enough quota available to store snapshots of all attached volumes, the instance snapshot fails. The quota issue may not be visible as the root cause of the problem. A workaround is to detach the data volume, perform the instance snapshot, and then reattach the data volume.  

![instance_snapshot_error.png]({{site.baseurl}}/img/Instance_Snapshot_Error.png)

If you are having this trouble, you could [open a support ticket](http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commonadmin/report-issue/), and our support team could help you diagnose it by looking at logs.

**In the CLI:** When using the `cinder-snapshot-create command` in the CLI, you can specify to perform the snapshot on a single volume only, for example, the the root volume. In this case, the snapshot is likely to be successful because the quota would be able to accommodate the volume capacity of a single volume.
