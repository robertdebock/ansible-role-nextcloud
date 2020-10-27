# [nextcloud](#nextcloud)

Install and configure Nextcloud on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-nextcloud.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-nextcloud)|[![github](https://github.com/robertdebock/ansible-role-nextcloud/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-nextcloud/actions)|[![quality](https://img.shields.io/ansible/quality/50634)](https://galaxy.ansible.com/robertdebock/nextcloud)|[![downloads](https://img.shields.io/ansible/role/d/50634)](https://galaxy.ansible.com/robertdebock/nextcloud)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-nextcloud.svg)](https://github.com/robertdebock/ansible-role-nextcloud/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.httpd
    - role: robertdebock.nextcloud
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.buildtools
    - role: robertdebock.epel
    - role: robertdebock.python_pip
    - role: robertdebock.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: robertdebock.httpd
    - role: robertdebock.redis
    - role: robertdebock.remi
      remi_enabled_repositories:
        - php74
    - role: robertdebock.php
    - role: robertdebock.php_fpm
    - role: robertdebock.mysql
      mysql_databases:
        - name: nextcloud
          encoding: utf8
          collation: utf8_bin
      mysql_users:
        - name: nextcloud
          password: N3x4Cl0ud
          priv: "nextcloud.*:ALL"
```

For verification `molecule/resources/verify.yml` runs after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if connection still works
      ping:
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for nextcloud

# The version of nextcloud to install.
nextcloud_version: 19.0.2

# The domain under which this server will be available. For example:
# "localhost" or "nextcloud.example.com". Does not include protocol identifier,
# (https://) or directories. (/nextcloud)
nextcloud_domain_url: "{{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0]) }}"

# Database connection details.
nextcloud_database_name: nextcloud
nextcloud_database_user: nextcloud
nextcloud_database_pass: N3x4Cl0ud
nextcloud_database_host: 127.0.0.1
nextcloud_admin_user: admin
nextcloud_admin_pass: N3x4Cl0ud
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.buildtools
- robertdebock.core_dependencies
- robertdebock.epel
- robertdebock.httpd
- robertdebock.mysql
- robertdebock.openssl
- robertdebock.php
- robertdebock.php_fpm
- robertdebock.python_pip
- robertdebock.redis
- robertdebock.remi

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/nextcloud.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|debian|all|
|el|7, 8|
|fedora|all|
|opensuse|all|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.



## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-nextcloud) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-nextcloud/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
