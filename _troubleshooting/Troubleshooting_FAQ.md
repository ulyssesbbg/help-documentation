---
layout: page
title:  "Troubleshooting FAQ"
featured: true
weight: 8
tags: [troubleshooting, faq, reboot, instance, timestamp, console, logs]
author: Leslie Lundquist, Ulysses Kanigel
dateAdded: August 19, 2016

---

**Q. How Can I Tell When My Virtual Machine Last Rebooted?**

**A.** You can use this command 
```
zgrep BOOT_IMAGE /var/log/kern.log* 
```
to see when the host (VM) last booted, and then use that timestamp to look through the logs to check for any clues regarding issues that you may be troubleshooting. 



**Q. What should I do if my instance froze and stopped responding?**

**A.** We recommend that you check the console of the virtual machine and see if it's completely frozen, or if there's evidence of a kernel panic or another error. If you aren't doing so already, you can get the URL of the console via 
```
nova get-vnc-console UUID novnc
```
Note the time on the console, if any.

Capture a screenshot of the console if it's frozen and contains useful information. After you reboot, if you've enabled crash dumps on the virtual machine, you can check `/var/crash` for `vmcore` (on RedHat) or `.crash` (on Ubuntu) files and analyze the dump from there. Also check system activity (sar) stats around the time of the freeze.

Reboot the machine if you need it back up ASAP, or better yet, spin up a new one based on the same image. If you need further investigation done on the infrastructure, try to let us know the time of the freeze in UTC prior to rebooting it, and the UUID of the instance, and we'll see if there is anything to be found in the logs.

**Q. Why can’t I resize my instance from `m1.tiny` to `m1.medium`?**

It’s possible that the instance was actually created at a time when the `m1.tiny` flavor had a larger Ephemeral disk. The resize process is checking for that, and it will fail to resize if that value is not found.

You might need to [open a support ticket](https://github.com/IBM-Blue-Box-Help/help-documentation/blob/gh-pages/_commonadmin/report-issue.md) to get help in verifying that this is the cause of your failure. The support team would see an error in the logs, something like this:

```
Exception during message handling: Resize error: Unable to resize disk down.
```

Alternatively, some good options for moving forward without opening a ticket would be to set the `m1.medium` flavor to the same larger Ephemeral disk size it was before, or to follow the troubleshooting workaround here for creating a new instance from snapshot: http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commontech/Instance_Resize/
