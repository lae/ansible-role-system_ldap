[![Build Status](https://travis-ci.org/fireeye-ops/ansible-role-system_ldap.svg?branch=master)](https://travis-ci.org/fireeye-ops/ansible-role-system_ldap)
[![Galaxy Role](https://img.shields.io/badge/ansible--galaxy-system_ldap-blue.svg)](https://galaxy.ansible.com/fireeye-ops/system_ldap/)

system_ldap
=========

Install and configure SSSD for system-level LDAP authentication

Example Playbook
----------------

There is an example playbook in the [test directory](tests/)

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
