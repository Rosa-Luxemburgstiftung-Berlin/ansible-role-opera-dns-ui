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
