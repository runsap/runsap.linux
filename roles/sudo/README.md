## Readme: Sudo Configuration Playbook

### Overview
This Ansible playbook allows users to configure sudo rules for various user groups. The provided example shows how to add custom sudo rules for an `ec2host` server. The playbook works in tandem with two included task files, `main.yml` and `sudo-group.yml`, to apply the sudo rules as specified in the `sudo_rules` variable.

### Prerequisites
- Ansible installed on the control node.
- Access to target nodes where the sudo configuration needs to be done. In this example, it's `ec2hosts`.
- Password-less SSH or appropriate authentication set up between the Ansible control node and target nodes.

### Structure
1. **main.yml**: 
    - This is the main task file that loops over each sudo group provided in the `sudo_data` variable.
    - For each sudo group, it includes the tasks in `sudo-group.yml`.
    
2. **sudo-group.yml**: 
    - For each sudo group, this file adds the appropriate sudo rules.
    - Uses the `lineinfile` module to ensure the desired sudo rule line exists in the sudo file.

### Usage

1. Place the `main.yml` and `sudo-group.yml` in your Ansible project's tasks directory (or the appropriate directory).
2. In your playbook, ensure you include the role and variables as shown in the provided example playbook.

#### Example Playbook:

```yml
- name: Create Sudo rules
  hosts: ec2hosts
  gather_facts: False
  become: true
  roles:
    - role: os-sudo
      vars: 
        sudo_data: "{{ sudo_rules }}"
        sudo_rules:
            sapadmin:
                file: 10-sap-sudo
                rules:
                become_sapadm_user:
                    sudo_user: sapadm
                mount_and_unmount_disks:
                    commands: "/usr/sbin/mount *, /usr/sbin/umount *"
```

In the example above:
- We are configuring sudo rules for hosts under `ec2hosts`.
- The `sapadmin` group is given rules to become the `sapadm` user and also to mount and unmount disks. These rules will be added to the `10-sap-sudo` file under `/etc/sudoers.d/`.

### Customization:

- You can easily extend or modify the `sudo_rules` variable to add more user groups or commands as needed.

### Notes:

- Ensure that the `/etc/sudoers.d/` directory exists on the target nodes and that the files within it are included by the main `/etc/sudoers` file.
- Always be cautious when editing sudo configuration. Incorrect rules or syntax can lock users out of administrative privileges.
- It's advisable to test any changes in a safe environment before applying them to production systems.

### Conclusion:
This Ansible playbook provides a structured way to manage sudo configurations across your infrastructure. Make sure to customize the `sudo_rules` variable as needed to fit your specific requirements.