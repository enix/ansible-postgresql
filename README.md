enix.postgresql
=================

A role for deploying and configuring [postgresql](http://www.postgresql.org) upstream release on unix hosts using [Ansible](http://www.ansible.com/).

Requirements
------------

Supported targets:

- Debian 9 "Stretch"
- Debian 10 "Buster"
- Debian 11 "Bullseye"
- Ubuntu 20.04 "Focal"
- Ubuntu 22.04 "Jammy"

Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration.

- `postgresql__version` - postgresql branch to install. defaults to 14. available: 10, 11, 12, 13, 14.
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

- `postgresql__install_pgbackrest` - install pgbackrest software <https://pgbackrest.org>.
- `postgresql__install_pg_auto_failover` - install pg_auto_failover software <https://github.com/citusdata/pg_auto_failover>.
- `postgresql__disable_initdb` - disable creation of the default main cluster. Usefull in case of pg_auto_failover cluster creation, or just package installation.

Dependencies
------------

- None

Usage
-----

Use Ansible galaxy requirements.yml

```yaml
    - src: enix.postgresql
```

And add it to your play's roles:

```yaml
    - hosts: all
      roles:
        - role enix.postgresql:
            postgresql__var: true
```

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

```bash
ansible-playbook -i inventory --extra-vars='{...}' main.yml
```

Still to do
-----------

- Check if we are deploying on a replica. In this case do no createdb, createuser
- Add CI tests using molecule

Changelog
---------

### 2.1.0

Add installation of pgbackrest and pg_auto_failover software.
Add disable_initdb option. This prevent installation to setup default main postgresql clusterdb.
### 2.0.0

Add support for postgresl 13 and 14.
Add Debian 10 Buster and Debian 11 Bullseye support.
Add Ubuntu 20.04 Focal and Ubuntu 22.04 Jammy support.
Remove debian jessie support.
Switch to molecule tests.
Use new ansible FQDN tasks

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

Laurent CORBES <laurent.corbes@enix.fr> - <http://www.enix.io>
