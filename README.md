[![Build Status](https://travis-ci.org/lae/ansible-role-system_ldap.svg?branch=master)](https://travis-ci.org/lae/ansible-role-system_ldap)
[![Galaxy Role](https://img.shields.io/badge/ansible--galaxy-system_ldap-blue.svg)](https://galaxy.ansible.com/lae/system_ldap/)

lae.system_ldap
=========

Install and configure SSSD for system-level LDAP authentication against an 
LDAP-enabled Active Directory server.

## Role Variables

TBD, but some explanation below.

## Example Playbook

The following is typically what we use in a multi-tenant playbook:

```
---
- hosts: all
  user: ansible
  roles:
    - lae.system_ldap
  become: True
```

There is also an example playbook in the [test directory](tests/)

### Extended usage

For this section, the playbook in the code block above is `system_ldap.yml`. 
Let's look at the following playbook layout:

    - system_ldap.yml
    - inventory
    - group_vars/
        - all/
            - main.yml
        - starlight/
            - main.yml
    - host_vars/
        - research-node01
    - roles/
        - requirements.yml

In this layout, we're typically able to group access control per hostgroup or 
per host. There are some variables that you likely want to set across all hosts, 
in `group_vars/all/main.yml` (or just `group_vars/all` if not using a directory):

    ---
    system_ldap_domain: aikatsu.net
    system_ldap_bind_dn: CN=Naoto Suzukawa,OU=Service Accounts,OU=Idol Schools,DC=Aikatsu,DC=net
    system_ldap_bind_password: sunrise
    system_ldap_search_base: OU=Idol Schools,DC=Aikatsu,DC=net
    system_ldap_uris:
      - ldaps://ldap-tyo.example.aikatsu.net:636
      - ldaps://ldap-ngo.example.aikatsu.net:636
    system_ldap_access_filter_groups:
      - CN=operations,OU=Security Groups,OU=Idol Schools,DC=Aikatsu,DC=net
    system_ldap_access_filter_users: []
    system_ldap_access_unix_groups:
      - operations
    system_ldap_sudo_groups:
      - operations
    system_ldap_sudo_users: []

Here we're using a search user account and password (`system_ldap_bind_*`) to 
keep in sync with an LDAP server over SSL (with a failover LDAPS server), 
allowing an "operations" group to authenticate as well as root privileges.

The `starlight` group's variables file may look like this:

    ---
    system_ldap_allow_passwordauth_in_sshd: true
    system_ldap_access_filter_users:
      - hoshimiya.ichigo
    system_ldap_sudo_users:
      - hoshimiya.ichigo

This allows the user name `hoshimiya.ichigo` to login to the machines in the 
`starlight` hostgroup, as well as use sudo on them. The variables above are 
matched against the `sAMAccountName` value from your LDAP-enabled AD server for 
any users in the `system_ldap_search_base` group.

You can also specify groups, but you will need to provide the full DN for the 
group filter variable. You'll also probably want to copy the group-related 
variables from `all`. For the other variables you can just use the CN. E.g:

    system_ldap_access_filter_groups:
      - CN=operations,OU=Security Groups,OU=Global,OU=Idol Schools,DC=Aikatsu,DC=net
      - CN=starlight-students,OU=Security Groups,OU=Starlight Academy,OU=Idol Schools,DC=Aikatsu,DC=net
    system_ldap_access_unix_groups:
      - operations
      - starlight-students
    system_ldap_sudo_groups:
      - operations

Here we add a `starlight-students` LDAP group, but only allow them to login.

Developing
----------

First clone and branch, or fork, this repo, make your changes, commit and submit
a pull request.

To keep track of ansible vault changes, include .gitconfig in your git config:

    echo -e "[include]\n\tpath = ../.gitconfig" >> .git/config

Testing
-------

    vagrant box add centos/7
    vagrant up
    vagrant provision

License
-------

MIT
