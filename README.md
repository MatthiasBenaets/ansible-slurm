Example execution:

```bash
ansible-playbook \
--ask-become-pass \
--inventory hosts \
--extra-vars="domain_name=example.com ad_password=secret" \
site.yml
```

To run only specific parts of the playbook, add `--tags="<space-separated-tags>"` parameter.
