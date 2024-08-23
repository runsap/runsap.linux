Ansible Role: os-users
======================

Description
-----------

The `os-users` Ansible role is designed to create or update OS users on target systems. It utilizes the `ansible.builtin.user` module to manage user accounts based on the provided `user_data`.

Requirements
------------

- SAP Inventory Role

Role Variables
--------------

The `os-users` role requires the following variable to be defined:

- `user_data`: A dictionary that provides the necessary information for creating or updating OS users. The keys in the dictionary represent the user names, and the values hold the user-specific attributes.

The supported attributes for each user are as follows:

- `name` (optional): The name of the user. If not provided, the key (user name) will be used as the user's name.
- `id` (optional): The user ID (UID) for the user. If not provided, the system will assign a default UID.
- `description` (optional): A comment/description for the user. If not provided, the comment field will be omitted.
- `shell` (optional): The login shell for the user. The default shell is `/bin/bash` if not specified.
- `groups` (optional): A list of additional groups the user should be a member of. Provide this as a comma-separated string in the `groups` field. If not specified, the user will not be added to any additional groups.
- `groups[0]` (optional): The primary group for the user. If not provided, the user will be assigned a default primary group.

Example Playbook
----------------

To use this role, include it in your playbook along with the required variables:

```yaml
- name: Playbook to manage OS users
  hosts: target_hosts
  roles:
    - role: os-users
      vars:
        user_data: "{{ host_data.os.users }}"
```

In this example, the `user_data` variable is sourced from the `host_data` variable, which should be defined elsewhere in your playbook.

Example of `user_data`
----------------------

The `user_data` variable should be structured as a dictionary with user names as keys and user attributes as values. Here's an example:

```yaml
host_data:
  os:
    users:
      john_doe:
        name: john.doe
        id: 1001
        description: "John Doe - Developer"
        shell: /bin/bash
        groups:
          - developers
          - sudo
      jane_smith:
        id: 1002
        groups:
          - users
```

In this example, two users, `john_doe` and `jane_smith`, are defined with their respective attributes.

Note: Ensure that you have the necessary permissions to create or update users on the target systems. This may require running the playbook with elevated privileges or using a user with appropriate permissions.

Author Information
------------------

This Ansible role was created by Sjoerd Lubbers. If you have any questions or encounter issues, feel free to reach out or create an issue in the repository.