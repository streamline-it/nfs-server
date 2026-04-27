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

RHEL example

- name: setup nfs server
  hosts: node5.rocky93.example.com
  become: true

  vars:

    nfs_group: nfsusers

    public_directory: /exp/nfs/pub
    private_directory: /exp/nfs/priv

    nfs_server_share_dirs:
      - directory_name: "{{ public_directory }}"
      - directory_name: "{{ private_directory }}"

    # following defaulted for RHEL hosts, FreeBSD will be slightly different
    nfs_server_exports:
      - export_line: "{{ private_directory }} 172.16.20.86(rw,sec=sys,no_root_squash)"
        export_path: "{{ private_directory }}"
      - export_line: "{{ public_directory }} 172.16.20.86(ro,sec=sys,no_root_squash)"
        export_path: "{{ public_directory }}"


  roles:

    - name: nfs-server

FreeBSD example

- name: setup nfs server
  hosts: node8.freebsd13.example.com
  become: true

  vars:

    nfs_group: nfsusers

    public_directory: /exp/nfs/pub
    private_directory: /exp/nfs/priv

    nfs_server_share_dirs:
      - directory_name: "{{ public_directory }}"
      - directory_name: "{{ private_directory }}"

    # following defaulted for RHEL hosts, FreeBSD will be slightly different
    nfs_server_exports:
      - export_line: "{{ private_directory }} sec=sys -maproot=root 172.16.20.86"
        export_path: "{{ private_directory }}"

    # Control the ports nfs uses server-side
    nfs_server_lockd_port: 4045
    nfs_server_mountd_port: 892
    nfs_server_statd_port: 1110

  roles:

    - name: nfs-server


License
-------

BSD

Author Information
------------------

