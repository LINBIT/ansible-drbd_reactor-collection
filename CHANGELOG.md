# Linbit\.Drbd\_Reactor Release Notes

**Topics**

- <a href="#v0-9-9">v0\.9\.9</a>
    - <a href="#minor-changes">Minor Changes</a>
    - <a href="#breaking-changes--porting-guide">Breaking Changes / Porting Guide</a>
    - <a href="#bugfixes">Bugfixes</a>
- <a href="#v0-9-8">v0\.9\.8</a>
    - <a href="#release-summary">Release Summary</a>
- <a href="#v0-9-7">v0\.9\.7</a>
    - <a href="#release-summary-1">Release Summary</a>

This changelog describes changes after version 0\.9\.6\.

<a id="v0-9-9"></a>
## v0\.9\.9

<a id="minor-changes"></a>
### Minor Changes

* Documentation \- rewrite the collection and role READMEs\, and point the Galaxy repository\, documentation\, and issue links at the public GitHub repository\.
* The collection now depends on <code>community\.general</code> for the SUSE package tasks in <code>resource\_agents\_distro</code>\.
* ganesha\_install \- new role that installs the NFS\-Ganesha userspace NFS server for the <code>ganesha\-nfs</code> OCF resource agent\. Include it from <code>reactor\_install</code> with <code>reactor\_install\_ganesha\: true</code>\.
* reactor\_install \- add <code>reactor\_install\_package\_state</code> \(default <code>present</code>\)\. Set it to <code>latest</code> to upgrade DRBD Reactor on reruns\.
* reactor\_install \- set <code>reactor\_install\_prometheus\: true</code> to open the DRBD Reactor Prometheus exporter port \(9942/tcp\) in the host firewall\. Choose the firewalld zone with <code>reactor\_install\_firewalld\_zone</code>\, or skip firewall handling with <code>reactor\_install\_firewall\_rules\: false</code>\.
* reactor\_install\, resource\_agents\_upstream \- add Ubuntu 26\.04 to the supported platforms and drop SLES 15 releases older than SP4\, which are out of support\.
* reactor\_install\, resource\_agents\_upstream \- retry package installation up to three times so transient repository failures do not fail the role\.
* resource\_agents\_distro \- new role that installs the distribution <code>resource\-agents</code> package and enables the High Availability repository it needs on each distribution\.
* resource\_agents\_upstream \- <code>resource\_agents\_upstream\_version</code> accepts branch names and commit SHAs\. Files installed from a branch are refreshed on every run\.
* resource\_agents\_upstream \- add <code>ganesha\-nfs</code> to the default agent list\. It is not in an upstream release yet\, so it installs only when the version is a branch such as <code>main</code>\.
* resource\_agents\_upstream \- skip agents missing from the selected version with a warning instead of failing\, so one agent list works across versions\.
* scst\_install \- new role\, moved from <code>linbit\.linstor\.gateway\_satellite</code>\, that builds and installs the SCST iSCSI target from source with DKMS\. It works standalone\, without DRBD Reactor or LINSTOR\. Include it from <code>reactor\_install</code> with <code>reactor\_install\_scst\: true</code>\.

<a id="breaking-changes--porting-guide"></a>
### Breaking Changes / Porting Guide

* resource\_agents\_upstream \- download the upstream files from GitHub on the Ansible control node and copy them to the target nodes\. The control node now needs GitHub access and the target nodes no longer do\. This avoids GitHub rate limits on larger clusters\.
* resource\_agents\_upstream \- the default <code>resource\_agents\_upstream\_version</code> is now the latest upstream release instead of <code>v4\.16\.0</code>\. Set it to <code>v4\.16\.0</code> to keep the previous version\.

<a id="bugfixes"></a>
### Bugfixes

* reactor\_install \- ship the drbd\-reactor\-reload units with the role\. Enabling automatic reload no longer fails on minimal or nodocs installs that strip the package documentation directory\.
* reactor\_install\, resource\_agents\_upstream \- gather minimal OS facts when they are missing\, so the roles work under <code>gather\_facts\: false</code> or with tag filters that skip fact gathering\.
* resource\_agents\_upstream \- install <code>gawk</code> on the Debian OS family\. Without it\, <code>IPaddr2</code> failed to start because <code>findif\.sh</code> needs gawk extensions that <code>mawk</code> lacks\.

<a id="v0-9-8"></a>
## v0\.9\.8

<a id="release-summary"></a>
### Release Summary

Version bump for sync across the LINBIT Ansible collections\. No user\-facing changes since 0\.9\.7\.

<a id="v0-9-7"></a>
## v0\.9\.7

<a id="release-summary-1"></a>
### Release Summary

Version bump only\, no user\-facing changes\. Released as part of the coordinated 0\.9\.7 cut across the LINBIT Ansible collections\.
