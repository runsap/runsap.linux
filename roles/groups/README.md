**README: os-groups Ansible Role**

This Ansible role, named `os-groups`, is designed to manage the creation or update of groups on target hosts. The role utilizes the `ansible.builtin.group` module to perform the group management tasks. The role expects input data in the form of `group_data`, which should be provided by the playbook invoking this role.

**Role Tasks**

The primary task of this role is to create or update groups on the target hosts using the provided `group_data`.

**How to Use the Role**

To use the `os-groups` role in your playbook, follow these steps:

1. Include the role in your playbook's `roles` section. For example:

```yaml
- name: Example playbook
  hosts: target_hosts
  roles:
    - role: os-groups
      vars:
        group_data: "{{ host_data.os.groups }}"
```

2. Define the `group_data` variable in your playbook. The `group_data` should be a dictionary where keys represent the group names, and values are dictionaries with optional attributes for each group.

   For example, the `group_data` variable could be defined like this:

```yaml
host_data:
  os:
    groups:
      group1:
        id: 1001
      group2:
        name: my_custom_group
        id: 1002
```

In the above example, two groups are defined: `group1` and `group2`. The `group1` will be created with the default name (key), while `group2` will have its name overwritten with the value provided in the `name` attribute.

**Explanation of the Role Task**

The task in the `roles/os-groups/tasks/main.yml` file performs the group creation/update. It utilizes the `ansible.builtin.group` module to do this.

Here's an explanation of the task:

```yaml
- name: Create or update groups
  ansible.builtin.group:
    name: "{{ item.value.name | default(item.key) }}"
    gid: "{{ item.value.id }}"
    state: present
  with_dict: "{{ group_data }}"
```

- `name`: This is the name of the group to be created or updated. It is set using the expression `{{ item.value.name | default(item.key) }}`. This expression checks if a `name` attribute is defined for the group in `group_data`. If it is defined, the value of `name` will be used as the group's name. If not, the default name will be set to the key, which is the group's name defined in the `group_data`.

- `gid`: This is the Group ID (GID) for the group. It is set using `{{ item.value.id }}`, which retrieves the GID from the `group_data` dictionary.

- `state`: This parameter specifies whether the group should be created or updated. In this case, it is set to `present`, indicating that the group should be present on the target hosts.

- `with_dict`: This loop iterates over the `group_data` dictionary, creating or updating each group based on the provided data.

**Note on Overwriting Group Name**

As mentioned earlier, the `name` attribute in the `group_data` dictionary allows you to overwrite the group name defined in the key. If the `name` attribute is present for a group, it will be used as the group's name; otherwise, the key's value will be used as the group's name. This flexibility allows you to customize group names as needed.

Author Information
------------------

This Ansible role was created by Sjoerd Lubbers. If you have any questions or encounter issues, feel free to reach out or create an issue in the repository.