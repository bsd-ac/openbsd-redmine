openbsd-redmine
===============

Ansible role to setup a simple and performant [redmine](https://www.redmine.org/) instance on an OpenBSD machine.

Role Variables
--------------

The significant variables are listed here for brevity. The full list of variables can be found in [defaults/main.yml].

| Variable            | Default         | Description                                                            |
|---------------------|-----------------|------------------------------------------------------------------------|
| **rm_version**      | `4.2.3`         | Redmine version to use.                                                |
| **rm_port**         | `8080`          | The port on which to run redmine.                                      |
| **rm_db_adapter**   | `postgresql`    | Which database service to use.                                         |
| **rm_db_user**      | `redmine`       | User for database access (must exist).                                 |
| **rm_db_pass**      | `''`            | Password for database access (must be given).                          |
| **rm_db_prod**      | `redmined`      | Database used for production environment (must exist).                 |
| **rm_db_dev**       | `redmine_devd`  | Database used for development environment (must exist).                |
| **rm_db_testd**     | `redmine_testd` | Database used for testing environment (must exist).                    |


RC scripts
----------

This role installs two [rc(8)](https://man.openbsd.org/rc.8) scripts:
- `redmine_puma`
- `redmine_unicorn`

Only use one of them at a time with rcctl:
```
root $ rcctl start redmine_puma
```

Running redmine commands
-------------------------------

Redmine gets installed to `{{ rm_home }}/app/`.

To run the commands as in their tutorials the proper environment variables need to be set, so that the ruby gems installed into the local directories are detected properly.

For example, to regenerate secrets for session storage, run the following commands as the user `{{ rm_user }}_`:

```
_redmine $ cd /var/redmine/app
_redmine $ env RAILS_ENV=production \
PATH=/var/redmine/ruby/bin:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin \
HOME=/var/redmine \
GEM_HOME=/var/redmine/ruby/2.7 \
bundle27 exec rake generate_secret_token
```

Sample playbook
---------------

An example of a playbook is available in [sample-playbook].

Bugs
----

- Only postgresql is supported for now. Should be updated to allow other databases.
