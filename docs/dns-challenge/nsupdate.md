# Variables for nsupdate dns-challenge

| Variable                | Required | Default  | Description
|-------------------------|----------|----------|------------
| acme_dns_servers        | yes      |          | List of DNS nameservers that need to be updated during a DNS-01 challenge.
| acme_dns_user           | no       | nsupdate | nsupdate Key Name
| acme_dns_password       | yes      |          | nsupdate Key Secret

## Usage

### wildcard certificate

```yaml
- name: create the certificate for *.example.com
  hosts: localhost
  collections:
    - telekom_mms.acme
  roles:
    - acme
  vars:
    acme_domain:
      certificate_name: "wildcard.example.com"
      zone: "example.com"
      email_address: "ssl-admin@example.com"
      subject_alt_name:
        - "*.example.com"
    acme_dns_servers: ['ns1.example.com', 'ns2.example.com']
    acme_challenge_provider: nsupdate
    acme_use_live_directory: false
    acme_account_email: "ssl-admin@example.com"
    acme_dns_password: !vault |
              $ANSIBLE_VAULT;1.1;AES256
              ...
```

### SAN certificate

```yaml
- name: create the certificate for example.com
  hosts: localhost
  collections:
    - telekom_mms.acme
  roles:
    - acme
  vars:
    acme_domain:
      certificate_name: "wildcard.example.com"
      zone: "example.com"
      email_address: "ssl-admin@example.com"
      subject_alt_name:
        - "example.com"
        - "domain1.example.com"
        - "domain2.example.com"
    acme_dns_servers: ['ns1.example.com', 'ns2.example.com']
    acme_challenge_provider: nsupdate
    acme_use_live_directory: false
    acme_account_email: "ssl-admin@example.com"
    acme_dns_password: !vault |
              $ANSIBLE_VAULT;1.1;AES256
              ...
```
