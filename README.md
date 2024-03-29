[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)
[![ansible-lint](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/lint.yml/badge.svg)](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/lint.yml)
[![molecule test](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/molecule.yml/badge.svg)](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/molecule.yml)

# ansible-role-opera-dns-ui
setup [opera dns ui](https://github.com/operasoftware/dns-ui) on debian based hosts via ansible

## requirements
tested with ansible `core >= 2.13.2`

## run

```yaml
- hosts: pdns
  vars:
    postgresql_users:
      - name: dnsui-user
        password: !vault  |
              $ANSIBLE_VAULT....
    postgresql_databases:
      - name: dnsui
        owner: "{{ postgresql_users.0.name }}"
    opera_dnsui_ini_db_dsn: "pgsql:host=localhost dbname={{ postgresql_databases.0.name }}"
    opera_dnsui_ini_db_username: "{{ postgresql_users.0.name }}"
    opera_dnsui_ini_db_password: "{{ postgresql_users.0.password }}"
    ...
  roles:
  - role: geerlingguy.postgresql #https://github.com/geerlingguy/ansible-role-postgresql
    tags:
      - pdns
      - postgresql
  - role: pdns-ansible #https://github.com/PowerDNS/pdns-ansible
    tags: pdns
  - role: opera-dns-ui
    tags:
      - pdns
      - opera-dns-ui
```

### install from fork

If you like to use a diferent install source, i.e. a fork, just set something like
```yaml
opera_dnsui_github_url: https://github.com/Rosa-Luxemburgstiftung-Berlin/dns-ui
opera_dnsui_repo_version: v0.2.7-203
```

## run without LDAP
set vars:
```yaml
# opera_dnsui_ldapurl: # unset this!
opera_dnsui_ini_ldap_enabled: 0
opera_dnsui_ini_php_auth_enabled: 1
```

create http auth user:
```
htpasswd -c /srv/opera-dnsui.httpauth test
```
and create php auth user:
```
root@ns:/srv/opera-dnsui# php scripts/create_admin_account.php
This script creates a new admin account in DNS UI.
If you are using a central authentication directory (eg. LDAP) then
you probably don't need to use this.

User ID of new admin account:
test

Full name of user:
testosteron

Email address of user:
test@test.tset

Administrative user test created.
```
