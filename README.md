[![](https://ucspython.herokuapp.com/badge.svg)](https://ucspython.herokuapp.com)

# ucsm-ansible

* Apache License, Version 2.0 (the "License") 

## News

Ansible 2.5 will ship with several UCS modules and we are working to include more in upcoming releases.  This repo represents the working copy of modules submitted to Ansible or in development for future submission.  This repo can be used to provide UCS Ansible modules before their inclusion in official Ansible releases.

Modules in the library directory that are prefixed with ucs (and not cisco) are submitted or in development.  utils/remote_management is the module_utils location for modules being actively maintained.

There is currently not support for scripted install/uninstall to avoid collision with Ansible hosted modules and ongoing maintenance.  You can specfiy this repo as a library and module_utils location with env variables or command line options (e.g., ANSIBLE_LIBRARY=./library ANSIBLE_MODULE_UTILS=./utils ansible-playbook ..).  Alternatively, your .ansible.cfg file can be updated to use this repo as the library and module_utils path with the following:
```
[defaults]
library = <path to ucsm-ansible clone>/library
module_utils = <path to ucsm-ansible clone>/utils
```

Modules submitted to Ansible are also used in roles now available on Ansible Galaxy: https://galaxy.ansible.com/list#/roles?autocomplete=ucs

Cisco's demo Cloud (dcloud.cisco.com) can be used to test and demo the Ansible modules and corresponding roles.  dCloud provides both a Hardware based environment (https://dcloud2-rtp.cisco.com/content/catalogue?search=unified%20computing%20system ) and an emulated environment (https://dcloud2-sjc.cisco.com/content/catalogue?search=ucs%20programmability ) that can be used with the UCS Ansible modules.

### Current Development Status

| Configuration Category | Configuration Task | Module Name | Status (part of 2.5, Proof of Concept, TBD |
| ---------------------- | ------------------ | ----------- | ------ |
| LAN                    | IP Addresses for KVM Access | ucs_ip_pool | in 2.5 |
|                        | MAC Address Pools  | ucs_mac_pool | in 2.5 |
|                        | VLANs              | ucs_vlans   | in 2.5 |
|                        | vNIC Templates     | ucs_vnic_template | in 2.5 |
|                        | LAN Connectivity Policy | ucs_lan_connectivity | in 2.5 |
|                        | Network Control Policy | TBD     | TBD    |
|                        | Port Channels      | TBD         | PoC: see https://github.com/dsoper2/ucsm-ansible |
| SAN                    | WWNN Pools         | ucs_wwn_pool | in 2.5 |
|                        | WWPN Pools         | ucs_wwn_pool | in 2.5 |
|                        | VSANs              | ucs_vsans   | in 2.5 |
|                        | vHBA Templates     | ucs_vhba_template | in 2.5 |
|                        | SAN Connectivity Policy | ucs_san_connectivity | in 2.5 |
| Server                 | UUID Suffix Pool   | ucs_uuid_pool | Planned for 2.6 |
|                        | Server Pool        | TBD         | PoC: see https://github.com/dsoper2/ucsm-ansible |
|                        | Server BIOS Policy | TBD         | PoC: see https://github.com/dsoper2/ucsm-ansible |
|                        | FW/Power/Maintenance Polices | TBD | TBD  |
|                        | Boot Policy        | TBD         | PoC: see https://github.com/dsoper2/ucsm-ansible |
|                        | Service Profile Template | ucs_service_profile_template | Planned for 2.6 |
|                        | Service Profile from Template | ucs_service_profile_from_template | Planned for 2.6 |
|                        | Service Profile assoc/disassoc | ucs_service_profile_association | Planned for 2.6 |
| Admin                  | NTP                | TBD         | PoC: cisco_ucs_ntp module |

### Ansible Development Notes

ucs_ modules in development follow processes documented at http://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html.  The modules support ansible-doc, and when submitted to Ansible they must pass Ansible CI testing and have integration tests.

When developing ucs_ modules in this repository, here are a few helpful commands to sanity check the code and documentation (replace module_name with your module (e.g., ucs_vlans)).  Ansible modules won't generally be pylint or pycodestyle (PEP8) clean without disabling several of the checks:
  ```
  pylint --disable=invalid-name,no-member,too-many-nested-blocks,redefined-variable-type,too-many-statements,too-many-branches,broad-except,line-too-long,missing-docstring,wrong-import-position,too-many-locals <module_name>.py
  
  pycodestyle --max-line-length 160 --config /dev/null --ignore E402 <module_name>.py
  
  ansible-doc <module_name>
  ```

### install
- ansible must be installed
 ```
 sudo pip install ansible
 ```
- you will also need the ucsmsdk.
  - `sudo pip install ucsmsdk`
  - If you would like the latest ucsmsdk from GitHub, here is an alternate install method:
    - `sudo pip install git+https://github.com/CiscoUcs/ucsmsdk.git`
  - It is a good idea to verify that the ucsmsdk can connect to the domains you want to manage with Ansible.  Here is an example connection test using python:
```
# python
Python 2.7.14 (default, Apr 27 2018, 14:31:56) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> from ucsmsdk import ucshandle
>>> handle = ucshandle.UcsHandle(ip='172.22.250.236', username='admin', password='password')
>>> handle.login()
True
```
- clone this repository 
```
git clone https://github.com/ciscoucs/ucsm-ansible
```
- Specfiy this repository as a library and module_utils location in your .ansible.cfg file (module_utils line is not needed for Ansible 2.5 and later)
```
[defaults]
library = <path to ucsm-ansible clone>/library
module_utils = <path to ucsm-ansible clone>/utils
```

# Community:

* We are on Slack - slack requires registration, but the ucspython team is open invitation to
  anyone to register [here](https://ucspython.herokuapp.com)
