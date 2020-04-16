# [Proof of concept for CVE-2020-3952](https://www.guardicore.com/2020/04/pwning-vmware-vcenter-cve-2020-3952/)

This is a short piece of code that exploits of CVE-2020-3952, which is described in detail at the Guardicore Labs post over [here](https://www.guardicore.com/2020/04/pwning-vmware-vcenter-cve-2020-3952/). 
This vulnerability was [published](https://www.vmware.com/security/advisories/VMSA-2020-0006.html) by VMware in April 2020 with a maximum CVSS score of 10.0. It allows an attacker with a network connection to take control of the vCenter Directory (and thus to the vSphere deployment).

VMware released a fix for this bug in vCenter Server 6.7 Update 3f. Any unpatched vCenter 6.7 that has been upgraded from a previous version is vulnerable to this attack. (Clean installs of vCenter 6.7 are not affected.)

We recommend reading the post to understand how this exploit works, but in short, it does three things:
1) Attempts an ldap bind request to the vmdird process. This should fail with invalid credentials.
2) Adds a new user with the requested username and password under the domain 'cn=NEW_USERNAME,cn=Users,dc=vsphere,dc=local'.
3) Adds the new user to the 'cn=Administrators,cn=Builtin,dc=vsphere,dc=local' group.

## Requirements
```sh
pip3 install python-ldap
```

## Usage
```sh
python3 exploit.py <VCENTER_IP> <NEW_USERNAME> <NEW_PASSWORD>
```
