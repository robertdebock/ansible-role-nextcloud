# [nextcloud](#nextcloud)

Install and configure Nextcloud on your system.

|Travis|GitHub|GitLab|Quality|Downloads|Version|
|------|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-nextcloud.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-nextcloud)|[![github](https://github.com/robertdebock/ansible-role-nextcloud/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-nextcloud/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-nextcloud/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-nextcloud)|[![quality](https://img.shields.io/ansible/quality/50634)](https://galaxy.ansible.com/robertdebock/nextcloud)|[![downloads](https://img.shields.io/ansible/role/d/50634)](https://galaxy.ansible.com/robertdebock/nextcloud)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-nextcloud.svg)](https://github.com/robertdebock/ansible-role-nextcloud/releases/)|

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

The machine needs to be prepared in CI this is done using `molecule/resources/prepare.yml`:
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

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-nextcloud/blob/master/requirements.txt).

## [Status of requirements](#status-of-requirements)

The following roles are used to prepare a system. You may choose to prepare your system in another way, I have tested these roles as well.

| Requirement | Travis | GitHub |
|-------------|--------|--------|
| [robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-bootstrap.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-bootstrap) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions) |
| [robertdebock.buildtools](https://galaxy.ansible.com/robertdebock/buildtools) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-buildtools.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-buildtools) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-buildtools/actions) |
| [robertdebock.core_dependencies](https://galaxy.ansible.com/robertdebock/core_dependencies) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-core_dependencies.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-core_dependencies) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-core_dependencies/actions) |
| [robertdebock.epel](https://galaxy.ansible.com/robertdebock/epel) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-epel.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-epel) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-epel/actions) |
| [robertdebock.httpd](https://galaxy.ansible.com/robertdebock/httpd) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-httpd) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-httpd/actions) |
| [robertdebock.mysql](https://galaxy.ansible.com/robertdebock/mysql) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-mysql.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-mysql) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-mysql/actions) |
| [robertdebock.openssl](https://galaxy.ansible.com/robertdebock/openssl) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-openssl.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-openssl) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-openssl/actions) |
| [robertdebock.php](https://galaxy.ansible.com/robertdebock/php) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-php.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-php) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-php/actions) |
| [robertdebock.php_fpm](https://galaxy.ansible.com/robertdebock/php_fpm) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-php_fpm.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-php_fpm) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-php_fpm/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-php_fpm/actions) |
| [robertdebock.python_pip](https://galaxy.ansible.com/robertdebock/python_pip) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-python_pip.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-python_pip) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-python_pip/actions) |
| [robertdebock.redis](https://galaxy.ansible.com/robertdebock/redis) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-redis.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-redis) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-redis/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-redis/actions) |
| [robertdebock.remi](https://galaxy.ansible.com/robertdebock/remi) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-remi.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-remi) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-remi/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-remi/actions) |

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
|fedora|33|
|opensuse|all|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| fedora:32 | nothing provides libzip(x86-64) >= 1.7.3 needed by php-pecl-zip-1.19.1-1.fc32.remi.7.4.x86_64 |
| fedora:rawhide | dependent role "remi" does not support fedora:rawhide. |


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
