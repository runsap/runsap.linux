ansible role: os-crontab
========================

This role manages the crontabs of your SAP estate.

✅ creates and removes crontabs
✅ for any user on the system
✅ is sap user aware
✅ is state aware
✅ provides a check mode report

The `crontabs` are part of the `host_data` variable.

Crontabs can be defined in `host_defaults`, `host_templates` and in the `sap_groups`.

There are 2 ways to assign a user to the crontab job.

1. Assign a fixed `user`, for example `root` or `sapadm`.
2. Assign a `sap_type` where the code will lookup the user belonging to the sap_type. For example: if `sap_type: hdb` is set. The code will lookup the value of tag `Sap_hdb_sid` and postfixes `adm` behind it.

If you don't use either the user running the playbook will be used.

Below is an example for **host_templates**:

```yaml
host_templates:
  sap_hana_v1:
    ec2:
      tags:
        Sap_type:
          - hdb
    crontabs:
      hana_cleaner:
        sap_type: hdb
        name: "Run HANA Cleanerr"
        minute: "0"
        hour: "23"
        job: /usr/sap/local/bin/hana_cleaner.sh
```

In this example a hana cleaner job is scheduled every evening at 23:00. Because this host template is attached to all sap hana systems. the crontab will be applied there as well.

Statefile
---------

This role uses a local state file that is defined in variable `crontab_state_file`. It defaults to `/usr/sap/crontabs`.

Requirements
------------

The role requires the use of the `sap-inventory` and it will pick up the `host_data.crontabs` variable. You can define these variables on a `template`, `host` or `group` level.

Variables
---------

`cloud` declares on what cloud you are. **vm** or **ec2**
`crontab_state_file` is the file containing the state on the instance.
`manage_crontabs` is a dictionary containing crontabs

Dependencies
------------

Following Ansible roles are required:

- `sap-inventory`

Example playbook
----------------

```yaml
- name: create inventory
  hosts: localhost
  gather_facts: False
  roles:
    - role: sap-inventory

- hosts: ec2hosts
  roles:
    - role: os-crontabs
      when: 
        - host_data.crontabs is defined

- name: create inventory
  hosts: localhost
  gather_facts: False
  tasks:
    - include_tasks: custom-summary.yml

```

Author
------

Created by Sjoerd Lubbers june 2023
