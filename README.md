system_ldap
=========

Install and configure SSSD for system-level LDAP authentication

Example Playbook
----------------

There is an example playbook in the [test directory](test/)

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
