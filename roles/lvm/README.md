# Ansible Role for Creating Logical Volumes and Filesystems

This Ansible role allows you to automate the creation of logical volumes, filesystems, and swap spaces using LVM. It is idempotent, meaning it only makes changes when necessary, and it can safely be run multiple times without changing the outcome after the first successful run.

## Requirements

This role requires the `community.general` Ansible collection. You can install it using the following command:

```bash
ansible-galaxy collection install community.general
```

## Variables

This role uses the following main variables:

- `vgs`: This variable is a dictionary where each key is a volume group name and each value is another dictionary with a list of associated physical volumes (disks). An example:

```yaml
vgs:
  appvg:
    disks:
      - /dev/xvdf
      - /dev/xvdg
  datavg:
    disks:
      - /dev/xvdh
      - /dev/xvdi
```

- `mounts`: This variable is a dictionary where each key is a mount point, and each value is another dictionary with the size, volume group name, logical volume name, and filesystem type for that mount point. It supports both normal filesystems and swap space. An example:

```yaml
mounts:
  /usr/sap:
    size: 3G
    vg_name: appvg
    lv_name: lv_usrsap
    fstype: ext4
  /sapmnt:
    size: 4G
    vg_name: appvg
    lv_name: lv_sapmnt
    fstype: ext4
  /swapfile:
    size: 16G
    vg_name: appvg
    lv_name: lv_swap
    fstype: swap
```

## Role Tasks

This role performs the following main tasks:

1. Create physical volumes as specified in the `vgs` variable.
2. Create volume groups using the newly created physical volumes.
3. Create logical volumes within these volume groups as specified in the `mounts` variable.
4. Create the appropriate filesystem on each logical volume. For swap spaces, it initializes the swap.
5. If the filesystem type is not swap, it ensures the mount point exists and mounts the logical volume there. It also ensures this mount is present in `/etc/fstab` so it persists after reboot.
6. If the filesystem type is swap, it enables the swap space if it is not already enabled and ensures this swap space is present in `/etc/fstab` so it persists after reboot.

## Running the Role

You can run this role with a playbook like the following:

```yaml
- hosts: servers
  roles:
    - your_role_name
```

Please remember to replace `your_role_name` with the actual name of your role.

## Important Note

Please test this role in a controlled environment first. It is advised to take a backup of any important data on the disks involved as operations like this can lead to data loss if there is an error or if incorrect parameters are provided.

Resizing of swap is not dynamic. The swap logical volume is enlarged but you have to make the filesystem grow by hand.

```
sudo swapoff --all
sudo lvremove -f /dev/vg_sap/lv_swap
```