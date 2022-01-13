Requirements
------------

Working postgresql setup.

In postgresql:
- `redmine` user
- following databases owned by `redmine`
  - `redmined`
  - `redmine_devd`
  - `redmine_testd`

```
$ psql -U postgres

postgres=# CREATE ROLE redmine LOGIN ENCRYPTED PASSWORD '<cool-password>' NOINHERIT VALID UNTIL 'infinity';

postgres=# CREATE DATABASE redmined WITH ENCODING='UTF8' OWNER=redmine;

postgres=# CREATE DATABASE redmine_devd WITH ENCODING='UTF8' OWNER=redmine;

postgres=# CREATE DATABASE redmine_testd WITH ENCODING='UTF8' OWNER=redmine;

postgres=# \q
```

Usage
-----

Copy `vars-sample.yml` to `vars.yml` and paste the postgresql password for the `redmine` user.

```
rm_db_pass: "<cool-password>"
```

Setup host in `inventory.ini` by changing `ansible_host` to the desired IP or machine name and run the playbook.

```
$ ansible-playbook site-install.yml
```

Start the `redmine` service on the host machine

```
root # rcctl start redmine
```
