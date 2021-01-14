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

- `postgresql__version` - postgresql branch to install. defaults to 10. available: 9, 10, 11, 12.
- `postgresql__extensions` - postgresql extensions packages to install.
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
- `postgresql__hba_entries` - Host Based Authentification entries to configure. will override PostgreSQL default ones. Default is not defined. Mandatory fields are `type, database, user, auth_method`, optional are `address, ip_address, ip_mask, auth_options`. To replicate package provided configuration:
```
postgresql_hba_entries:
  - {type: local, database: all, user: postgres, auth_method: peer}
  - {type: local, database: all, user: all, auth_method: peer}
  - {type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: md5}
  - {type: host, database: all, user: all, address: '::1/128', auth_method: md5}
# starting version 10 there is replication role
  - {type: local, database: replication, user: all, auth_method: peer}
  - {type: host, database: replication, user: all, address: '127.0.0.1/32', auth_method: md5}
  - {type: host, database: replication, user: all, address: '::1/128', auth_method: md5}
```
- `postgresql__users` - Users to be crated on postgresql server and associated credentials:
```
postgresql__users:
  - {name: "foobar",
    password: "supersecure",
    encrypted: yes,
    expires: 'infinity',
    state: 'present',
    conn_limit: none,
    priv: none,
    role_attr_flags: none,
    db: none
```
- `postgresql__databases` - state of databases to be installed on the server. only mandatory parameter is name:
```
postgresql__databases:
  - {name: test,
    lc_collate: 'en_US.UTF-8',
    lc_ctype: 'en_US.UTF-8',
    encoding: 'UTF-8',
    template: 'template0',
    owner: postgres,
    extension: [], # check that you have installed then needed extension module before
    state: 'present'
   }
```

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

- Check if we are deploying on a replica. In this case do no createdb, createuser
- Add CI tests using molecule


Changelog
---------

### 1.2.0

Add support for postgresql 12

### 1.1.0

Add support for an extension list in databases definition and configuration.

### 1.0.0

Initial version.

License
-------

GPLv2

Author Information
------------------

Laurent CORBES <laurent.corbes@enix.fr> - http://www.enix.io
