Role Name
=========

nfs-server

Requirements
------------
None

Role Variables
--------------


Controls creation of directories

nfs_group: nfsusers

public_directory: /exp/nfs/pub
private_directory: /exp/nfs/priv

nfs_server_share_dirs:           
  - directory_name: "{{ public_directory }}" 
  - directory_name: "{{ private_directory }}"


nfs_server_exports:
  - export_line: "{{ public_directory }}" localhost(ro,sec=sys,no_root_squash)
  - export_line: "{{ private_directory }}" localhost(rw,sec=sys,no_root_squash)


# port for lockd
port={{ nfs_server_lockd_port }}

# port for mountd
port={{ nfs_server_mountd_port }}

# port for statd
port={{ nfs_server_statd_port }}

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - nfs_server

or
  
     - hosts: servers
       tasks:
         - name: import_role
           import_role:
             name: nfs_server
           vars: 
             nfs_group: nfsusers
             nfs_server_exports:
               - export_line: /exp/nfs/pub node[2-3].example.com(ro,sec=sys,no_root_squash)
               - export_line: /exp/nfs/priv node[2-3](rw,sec=sys,no_root_squash)

             nfs_server_share_dirs:           
               - directory_name: /exp/nfs/pub 
               - directory_name: /exp/nfs/priv


License
-------

BSD

Author Information
------------------


