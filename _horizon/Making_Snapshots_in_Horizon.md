---
layout: page
title: Using Horizon for Snapshots
author: Leslie Lundquist
dateAdded: December 12, 2016
tags: [Horizon, snapshots, OpenStack, Cinder, image]
featured: true
weight: 4
---

When running either the 3.0 or 3.1 release using Horizon, snapshotting behavior can be confusing.

Here’s why:

**In Horizon:** When using the snapshot command via Horizon, all attached volumes are snapshotted. However, if there is not enough quota to snapshot all attached volumes, the instance snapshot fails. Horizon suppresses error messages, so the quota issue may not be visible as the root cause of the problem. 

If you are having this trouble, you could [open a support ticket](http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commonadmin/report-issue/), and our support team could help you diagnose it by looking at logs.

**Best Practice:**
 When using Horizon it is best to detach the data volume, perform the instance snapshot, and then reattach the data volume.

**In the CLI:** When using the `cinder-snapshot-create command` in the CLI, you can specify to perform the snapshot on a single volume only, for example, the the root volume. In this case, the snapshot is likely to be successful because the quota would be able to accommodate the volume capacity of a single volume.
