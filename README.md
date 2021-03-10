# [molecule_test](#molecule_test)

Prepare your system to be managed by Ansible.

|GitHub|Version|Ansible Galaxy|Quality|Downloads|
|------|-------|--------------|-------|---------|
|[![github](https://github.com/ucomesdag/ansible-role-molecule_test/workflows/Ansible%20Molecule/badge.svg)](https://github.com/ucomesdag/ansible-role-molecule_test/actions)|[![Version](https://img.shields.io/github/release/ucomesdag/ansible-role-molecule_test.svg)](https://github.com/ucomesdag/ansible-role-molecule_test/releases/)|[![role](https://img.shields.io/ansible/role/53578)](https://galaxy.ansible.com/ucoucomesdag/molecule_test)|[![quality](https://img.shields.io/ansible/quality/53578)](https://galaxy.ansible.com/ucoucomesdag/molecule_test)|[![downloads](https://img.shields.io/ansible/role/d/53578)](https://galaxy.ansible.com/ucoucomesdag/molecule_test)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: ucomesdag.molecule_test
```

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for molecule_test

# The user to use to connect to machines.
molecule_test_user: root

# Do you want to wait for the host to be available?
molecule_test_wait_for_host: no

# The number of seconds you want to wait during connection test before failing.
molecule_test_timeout: 3
```

## [Dependencies](#Ddpendencies)

Overview of role dependencies:

![dependencies](https://raw.githubusercontent.com/ucomesdag/ansible-role-molecule_test/png/requirements.png "Dependencies")

## [Requirements](#role-requirements)

- pip packages listed in [requirements.txt](https://github.com/ucomesdag/ansible-role-molecule_test/blob/master/requirements.txt).

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://quay.io/user/ucomesdag):

|container|tags|
|---------|----|
|amazon|Candidate|
|el|7, 8|
|debian|all|
|fedora|all|
|opensuse|all|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| alpine:edge | Failed to create temporary directory. |


If you find issues, please register them in [GitHub](https://github.com/ucomesdag/ansible-role-molecule_test/issues)

## [License](#license)

Apache-2.0

