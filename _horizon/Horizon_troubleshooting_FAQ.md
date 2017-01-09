---
layout: page
title:  "Horizon Troubleshooting FAQ"
featured: true
weight: 8
tags: [troubleshooting, faq, server, cookie, timezone, quotas, filter, volume limit]
author: Leslie Lundquist, Ulysses Kanigel
dateAdded: December 16, 2016
dateUpdated: January 9, 2017

---

**Q. Why am I getting the error "Can't create server" in Horizon?**

**A.** You might be running low on one of your project quotas, potentially the port quota.  You can view quotas by running `neutron quota-show --tenant-id <UUID>`  Try adjusting your project quotas.

**Q. Why don't my timezone settings stick in Horizon after I clear my cookies?**

**A.** Horizon stores your timezone in a cookie on your computer that persists for 1 year.  It does not store the timezone on the server.  If you clear your cookies or use a new browser, the timezone will be set to UTC, and you will need to recreate the cookie by reconfiguring your timezone in Horizon.

**Q. When I filter my volumes or projects list, why does Horizon only show results from the current page?**

**A.** This is due to an upstream bug; please see https://bugs.launchpad.net/horizon/+bug/1333240 and consider adding your vote to it to get it fixed.

Two workarounds exist:

1. Increase your items shown per page in your Horizon user settings to a number greater or equal to the amount of volumes you have, for example, 120.  The caveat here is that if you have a long list of say 120 volumes, it's going to take around ~39 seconds to load the list.  But the filtering will work across the whole set of 120 volumes instead of just the first 20 displayed.

2. A much faster way of displaying / searching / filtering volumes is to run `openstack volume list --all | grep NAME` (as cloud_admin) or `openstack volume list | grep NAME` (as a user of the project the volumes are in).  You can get more help on managing volumes quickly via the command line at http://docs.openstack.org/user-guide/common/cli-manage-volumes.html

**Q.Why am I getting an error "Unable to retrieve volume limit information." in Horizon?**
**A.** This error is harmless.  It happens because you do not have any Cinder block storage nodes ordered for your cluster.

The error can be resolved by removing the Cinder endpoint from the Keystone service.  This may or may not require an automation config file update so the endpoint doesn't appear again during the next upgrade.  Please open a ticket with our support team requesting removal of the Cinder endpoint.
