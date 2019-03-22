# Ansible Role: sssd-ldap

An Ansible Role that installs and configure sssd, nsswitch, pam and sshd to get user accounts from LDAP

Currently tested on

- Ubuntu 18.04.2 LTS (Bionic Beaver)

## Requirements

You have to define some variables required to connect to LDAP server

Of course you can define credentials used to bind to your LDAP server in a plain
text - but I strongly recommend using ansible Vault.

```yaml
ldap_search_base: "cn=accounts,dc=example,dc=com"
ldap_user_search_base: "cn=users,cn=accounts,dc=example,dc=com"
ldap_group_search_base: "cn=groups,cn=accounts,dc=example,dc=com"
ldap_pam_filter: "&(objectclass=posixAccount)(!(nsaccountlock=True))"
ldap_bind_dn: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  ( ... )
ldap_bind_pw: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  ( ... )

```

## Other role variables

You can find available options in `defaults/main.yml` and `templates/sssd.conf.j2`

Most important are

```yaml
use_sssd: true   #  why would anyone want to change it?
sssd_use_ssh_keys_from_ldap: true
ldap_user_ssh_public_key: "ipaSshPubKey"

sshd_AuthorizedKeysCommand: /usr/bin/sss_ssh_authorizedkeys
sshd_AuthorizedKeysCommandUser: root

sssd_create_homedir: true
sssd_cache_credentials: true
```

## Dependencies

None.

## Example Playbook

```yaml
    - hosts: servers
      roles:
         - role: weehal.sssd
```

## License

BSD

## Author Information

This role was created in 2019 by [Michał 'warf' Łuczak](https://warf.ml/)
