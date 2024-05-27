---
description: >-
  This article provides some helpful tips to protect yourself when running intentionally vulnerable machines in your pentesting lab environment.
title: "Tips for securing your pentesting lab environment"
categories: [Tips]
tags: [tips, ctf, vulnhub, lab, cybersecurity]
date: 2024-05-27 12:00:00 -0500
---
When running intentionally vulnerable virtual machines to practice attacking in your lab environment, there are some inherent risks that come with these machines.  

If you are downloading and using a vulnerable virtual machine created by someone who isn't you, it is possible that the machine is backdoored or infected. For example, the machine could contain a cronjob that attempts to initiate a reverse shell connection to an attacker's infrastructure. The machine could also contain a guest-to-host privilege escalation vulnerability that gets intentionally triggered to infect your host machine.  
Even if the machine was not created by a malicious actor, it is vulnerable and should be treated as such.  

Below are some actions you can take to better protect yourself when you are doing your pentesting labs:  
* Keep your attacker machine OS and software up-to-date on patches
* Disable unnecessary services and set up firewall rules on your attacker machine
* Use strong passwords for the users on your attacker machine
* Patch and harden your host machine and the hypervisor software
* Utilize a modern AV/EDR on your host machine
* Implement the principle of least privilege on your attacker and host machines (i.e. only use a privileged user for administrative tasks)
* Take a snapshot of your attacker machine before each pentesting lab, revert when finished
* Remove unnecessary hardware from your virtual machines (USB controllers, audio devices, etc)
* Avoid using VM guest tools on your virtual machines
* Create and use a virtual network that is isolated and can only talk to other machines on the same network (no internet access) - place your attacker VM and vulnerable VM on this network
* Monitor for suspicious network traffic attempting to leave the isolated virtual network
* Shut down your lab infrastructure when it is not in use
* If you can, dedicate a physical machine to be the hypervisor host specifically for pentesting labs, then segment that host from your regular network

Happy hacking!

## Additional Resources
[https://www.vulnhub.com/faq/#security](https://www.vulnhub.com/faq/#security)  
[https://digi.ninja/blog/untrusted_vms.php](https://digi.ninja/blog/untrusted_vms.php)  
[https://kb.mit.edu/confluence/display/istcontrib/VMware+Security+Recommendations+and+Best+Practices](https://kb.mit.edu/confluence/display/istcontrib/VMware+Security+Recommendations+and+Best+Practices)  
