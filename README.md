enix.postgresql
=================

A role for deploying and configuring [postgresql](http://www.postgresql.org) upstream release on unix hosts using [Ansible](http://www.ansible.com/).


Requirements
------------

Supported targets:

- Debian 8 "Jessie"
- Debian 9 "Stretch"


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration.

- `postgresql__version` - postgresql branch to install. defaults to 10. available: 9, 10, 11.
- `postgresql__global_config_options` - global configuration options to set into postgresql.conf. common options are:
```
postgresql__global_config_options:
  - option: listen_addresses
    value: '*'
  - option: log_min_duration_statement
    value: 1000
  - option: bonjour
    value: off
    state: absent
```
- `postgresql__includeconf` - list of configuration files to template and push into conf.d/.

Dependencies
------------

- None

Usage
-----

Use Ansible galaxy requirements.yml

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

Laurent CORBES <laurent.corbes@enix.fr> - http://www.enix.io
