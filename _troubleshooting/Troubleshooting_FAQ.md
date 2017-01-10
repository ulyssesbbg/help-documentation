---
layout: page
title:  "Troubleshooting FAQ"
featured: true
weight: 8
tags: [troubleshooting, faq, reboot, instance, timestamp, console, resize, stuck, port, depends_on]
author: Leslie Lundquist, Ulysses Kanigel
dateAdded: August 19, 2016

---
#### Table of Contents

 * [How Can I Tell When My Virtual Machine Last Rebooted?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#how-can-i-tell-when-my-virtual-machine-last-rebooted)
 * [What should I do if my instance froze and stopped responding?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#what-should-i-do-if-my-instance-froze-and-stopped-responding)
 * [Why can’t I resize my instance from `m1.tiny` to `m1.medium`?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#why-cant-i-resize-my-instance-from-m1tiny-to-m1medium)
 * [What causes stuck ports in Neutron?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#what-causes-stuck-ports-in-neutron)
 * [What ports need to be open for OpenStack?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#what-ports-need-to-be-open-for-openstack)
 * [What does this error mean? "Error: Unable to retrieve volume limit information."](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#what-does-this-error-mean-error-unable-to-retrieve-volume-limit-information)
 * [Why is access to port 25 not working for my outbound email?](http://ibm-blue-box-help.github.io/help-documentation/troubleshooting/Troubleshooting_FAQ.md/#why-is-access-to-port-25-not-working-for-my-outbound-email)
 
#### Q. How Can I Tell When My Virtual Machine Last Rebooted?

**A.** You can use this command:

```
zgrep BOOT_IMAGE /var/log/kern.log* 
```
to see when the host (VM) last booted, and then use that timestamp to look through the logs to check for any clues regarding issues that you may be troubleshooting. 


#### Q. What should I do if my instance froze and stopped responding?

**A.** We recommend that you check the console of the virtual machine and see if it's completely frozen, or if there's evidence of a kernel panic or another error. If you aren't doing so already, you can get the URL of the console via 
```
nova get-vnc-console UUID novnc
```
Note the time on the console, if any.

Capture a screenshot of the console if it's frozen and contains useful information. After you reboot, if you've enabled crash dumps on the virtual machine, you can check `/var/crash` for `vmcore` (on RedHat) or `.crash` (on Ubuntu) files and analyze the dump from there. Also check system activity (sar) stats around the time of the freeze.

Reboot the machine if you need it back up ASAP, or better yet, spin up a new one based on the same image. If you need further investigation done on the infrastructure, try to let us know the time of the freeze in UTC prior to rebooting it, and the UUID of the instance, and we'll see if there is anything to be found in the logs.

#### Q. Why can’t I resize my instance from `m1.tiny` to `m1.medium`?

It’s possible that the instance was actually created at a time when the `m1.tiny` flavor had a larger Ephemeral disk. The resize process is checking for that, and it will fail to resize if that value is not found.

You might need to [open a support ticket](https://github.com/IBM-Blue-Box-Help/help-documentation/blob/gh-pages/_commonadmin/report-issue.md) to get help in verifying that this is the cause of your failure. The support team would see an error in the logs, something like this:

```
Exception during message handling: Resize error: Unable to resize disk down.
```

Alternatively, some good options for moving forward without opening a ticket would be to set the `m1.medium` flavor to the same larger Ephemeral disk size it was before, or to follow the troubleshooting workaround here for creating a new instance from snapshot: http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commontech/Instance_Resize/

#### Q. What causes stuck ports in Neutron?

Stuck ports can occur in Neutron when instantiating VMs using Heat. This issue is believed to be related to a defect in OpenStack Neutron (Mitaka release) due to a race condition in Neutron. Therefore, we are treating it as a known limitation. This condition may also cause metadata failure along with stuck ports.

Several workarounds exist:

**Workaround #1.** If possible, instances can be deployed without using Heat. Manual deployment seems to circumvent this condition.

**Workaround #2.** Another workaround is to introduce a delay in Heat, using `WaitCondition` from the API.  However, `WaitCondition` may not be ideal in this scenario.

 * `WaitCondition` works in conjunction with `WaitConditionHandler`.This handler creates a URL/URI that is monitored for a string of text. The presence of this string denotes success of a deployed VM, and the absence of that string denotes failure. Once a VM is instantiated it can be instructed to populate the URL/URI with the appropriate string of text via a `PUT` API call.

 * Currently, no mechanism exists in our OpenStack deployment for an L3 router to make that API call. Our engineering team suggests using 'depends_on'. (See option 3, which follows).

**Workaround #3.** The best workaround is to use `depends_on`. It creates a scenario in orchestration so that the deployment of a VM "depends on" the existence of other, definable, resources: for example an L3 Router and Network. 

A brief `depends_on` example:

```
add_router_interface:
   type: OS::Neutron::RouterInterface
   properties:
     router: { get_resource: router}
     subnet: { get_resource: subnet}


server1:
    type: OS::Nova::Server
    depends on: add_router_interface
    properties:
      image: { get_param: image }
```

More details on Explicit Dependencies are available at the following link:

 * https://wiki.openstack.org/wiki/Heat/Blueprints/hot-software-config-spec


#### Q. What ports need to be open for OpenStack?

You will need to open at least ports 67/68 for DHCP and port 80 for metadata. Additionally, if you want VMs to be able to access OpenStack API services, include the ports documented here:

[http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commontech/FAQ/#which-portsurls-etc-are-publicly-accessible-on-my-bluemix-private-cloud-and-for-what-purpose](http://ibm-blue-box-help.github.io/help-documentation/gettingstarted/commontech/FAQ/#which-portsurls-etc-are-publicly-accessible-on-my-bluemix-private-cloud-and-for-what-purpose)

If the minimum required set of ports is not open, you may encounter trouble with `cloud_init`so that your instances may not be able to start successfully.

#### Q. What does this error mean? "Error: Unable to retrieve volume limit information."

Often when I load a page in the Horizon dashboard, I see a little red pop-up that says, "Error: Unable to retrieve volume limit information." What is this error?

This error is harmless, but it happens because you do not have any Cinder block storage in your deployment.

The error can be resolved by removing the Cinder endpoint from the Keystone service. This could require an update to your automation config files, and the Support team will need to check. Please open a support ticket if you need help to resolve this error message.

Here is an example of the steps that the support technician would need to follow:

```
# openstack endpoint list | grep cinder 
| 19a90853883049ed92cf4af82a24c0db | RegionOne | cinderv2     | volumev2       | True    | name  | https://... | 
| 2c3e8d0a15304788a9dc1806917b61ee | RegionOne | cinderv2     | volumev2       | True    | other     | https://... | 
| 39434faf90484a208b61a517926042b2 | RegionOne | cinderv2     | volumev2       | True    | name2    | https://... | 

# openstack endpoint delete 19a90853883049ed92cf4af82a24c0db 
# openstack endpoint delete 2c3e8d0a15304788a9dc1806917b61ee 
# openstack endpoint delete 39434faf90484a208b61a517926042b2 

# openstack service list | grep cinder 
| ef7a982aaf224c62a09c6e813ac66141 | cinderv2   | volumev2       | 

# openstack service delete ef7a982aaf224c62a09c6e813ac66141
```
Check that automation is correct:

```
$ grep -A2 ^cinder ../all.yml 
cinder:  
  enabled: False 
cinderv2:  
  enabled: False
```

#### Q. Why is access to port 25 not working for my outbound email?

**A.** SoftLayer no longer allows outbound connections through TCP port 25 (SMTP) on new accounts. Please refer to the following article for more information:

[https://knowledgelayer.softlayer.com/procedure/outbound-email-port-25](https://knowledgelayer.softlayer.com/procedure/outbound-email-port-25)
