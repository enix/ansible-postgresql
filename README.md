enix.postgresql
=================

A role for deploying and configuring [postgresql](http://postgresql.com) and extensions on unix hosts using [Ansible](http://www.ansible.com/).


Requirements
------------

Supported targets:

- Ubuntu 14.04 "Trusty"
- Ubuntu 16.04 "Xenial"
- Debian 8 "Jessie"
- Debian 9 "Stretch"
- RedHat EL / CentOS 6
- RedHat EL / CentOS 7
- ...


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `postgresql__`.

- `postgresql__` - desc

Dependencies
------------

- None

Extra
-----

Available extensions (under a switch):

- Extension - `postgresql_extension`
- ...

Callable tasks:

- `task`: ...


Usage
-----

Use Ansible galaxy requirements.yml

    # postgresql from enix
    # private role
    - src: git+ssh://git@gitlab.enix.org/ansible/ansible-postgresql.git
      name: enix.postgresql

    # public role
    - src: enix.postgresql

And add it to your play's roles:

    - hosts: all
      roles:
        - role enix.postgresql:
            postgresql__var: true

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

    $ ansible-playbook -i inventory --extra-vars='{...}' main.yml

Still to do
-----------

- Write the role itself, for one


Changelog
---------

### 0.1

Initial version.

License
-------

GPLv2

Author Information
------------------

your name <your.name@enix.fr> - http://www.enix.io