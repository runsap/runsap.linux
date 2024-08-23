s3fs
=========

Creates a s3 fuse filesystem meaning that you can mount a s3bucket as a filesystem. 

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

        - include_role: 
            name: s3fs
          vars:
            access_keys_id: "{{ s3_software_bucket_cred.access_key_id }}"
            secret_access_key: "{{ s3_software_bucket_cred.secret_access_key }}"
            s3fs_mounts: 
                /software:
                  bucket: 047623875516-dev-software


References
----------

The following references are found to improve speed for s3fs

https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-s3fs


License
-------

MIT

Author Information
------------------
S. Lubbers
