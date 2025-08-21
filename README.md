Example execution:

```bash
ansible-playbook \
--ask-become-pass \
--inventory hosts \
--extra-vars="domain_name=example.com ad_password=secret ldap_password=secret mysql_password=secret slurmdb_password=secret" \
site.yml
```

To run only specific parts of the playbook, add `--tags="<space-separated-tags>"` parameter.

Lint checking (with Homebrew):\
It's required to install ansible-galaxy collections forcefully, so that ansible-lint can find them with homebrew.\
`ansible-galaxy collection install -f community.general`\
and then run:\
`ANSIBLE_COLLECTIONS_PATH=~/.ansible/collections ansible-lint site.yml`
