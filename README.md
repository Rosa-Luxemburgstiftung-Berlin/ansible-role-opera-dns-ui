[![ansible-lint](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/lint.yml/badge.svg)](https://github.com/Rosa-Luxemburgstiftung-Berlin/ansible-role-opera-dns-ui/actions/workflows/lint.yml)

# ansible-role-opera-dns-ui
setup [opera dns ui](https://github.com/operasoftware/dns-ui) on debian based hosts via ansible

## run

```
- hosts: pdns
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

## run without LDAP
set vars:
```
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
