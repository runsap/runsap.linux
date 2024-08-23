linux-firewall
==============
 
Introduction
------------
Small role that inputs a list of ports that need to be open.
 
Parameters
----------

**fw_ports** 
A list of firewall ports with the following options:

```
fw_ports:
  - name: Functional Description
    port: port/tcp|udp
    state: disabled # default is enabled
```

Requirements
------------
Playbook needs become root rights.

Example playbook
----------------

```
- name: Play to install SAP Routers
  hosts: sap_routers
  gather_facts: False
  become: true
  roles:    
    - role: sap-firewall
      vars:
        fw_ports:
          - name: SAP Router
            port: 3299/tcp
          - name: SAP Message Server
            port: 3600/tcp
            state: disabled
```

Improvements
------------
- [ ] ports can only be disabled not removed. Seems by design. Maybe for further investigation. For us this is fine now.
- [ ] State of fw, etc
- [ ] Fw zones? Port foreward?

Author
------
Initial version March 2023 by Sjoerd Lubbers
