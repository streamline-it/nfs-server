Role Name
=========

nfs-server

Requirements
------------
None

Role Variables
--------------

# defaults file for nfs-server
nfs_group: nfsusers

public_directory: /exp/nfs/pub
private_directory: /exp/nfs/priv

nfs_server_share_dirs:
  - directory_name: "{{ public_directory }}"
  - directory_name: "{{ private_directory }}"

# following defaulted for RHEL hosts, FreeBSD will be slightly different
nfs_server_exports:
  - export_line: "{{ public_directory }} localhost(ro,sec=sys,no_root_squash)"
    export_path: "{{ public_directory }}"
  - export_line: "{{ private_directory }} localhost(rw,sec=sys,no_root_squash)"
    export_path: "{{ private_directory }}"

# Control the ports nfs uses server-side
nfs_server_lockd_port: 4045
nfs_server_mountd_port: 892
nfs_server_statd_port: 1110

# FreeBSD specific rc.conf variables

nfs_server_rc_vars:
  - line: 'nfs_server_enable="YES"'
  - line: 'rpc_bind_enable="YES"'
  - line: 'mountd_enable="YES"'
  - line: 'rpc_lockd_enable="YES"'
  - line: 'rpc_statd_enable="YES"'


#
#
# contents of vars/RedHat.yml
#

nfs_svc: nfs-server

#
#
# contents of vars/FreeBSD.yml
#


nfs_svc: nfs_server
nfs_lockd_svc: lockd
nfs_statd_svc: statd


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

