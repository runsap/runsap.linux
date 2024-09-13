runsap.linux: azure_blobfuse
============================

Introduction
------------
This role creates a filesystem from a blob in a storage account.

This implementation is build on this documentation:
https://learn.microsoft.com/en-us/azure/storage/blobs/storage-how-to-mount-container-linux

Example
-------

```yaml
- name: CIFS mounts
  include_role: 
    name: runsap.linux.azure_cifs
  vars:
    blobs: 
      /mnt/blob_share:
        container_name: share
        storage_account_name: runsapshared
        storage_account_key: secretkey
```            

Optionally you can add path to mount the container on a different location.
