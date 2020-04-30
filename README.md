# forensics

Install and configure forensics on your system.

|Travis|GitHub|Quality|Downloads|
|------|------|-------|---------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-forensics.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-forensics)|[![github](https://github.com/robertdebock/ansible-role-forensics/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-forensics/actions)|[![quality](https://img.shields.io/ansible/quality/45300)](https://galaxy.ansible.com/robertdebock/forensics)|[![downloads](https://img.shields.io/ansible/role/d/45300)](https://galaxy.ansible.com/robertdebock/forensics)|

## Example Playbook

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.forensics
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
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

## Role Variables

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for forensics

# A directory where collected data can be stored locally.
forensics_local_storage_path: /tmp/forensics

# A list of commands to run.
forensics_command_list:
  - journalctl -xe
  - ps -ef
  - lsof
  - systemctl status
  - netstat -an
  - netstat -tulpen

# A list of directories to collect all files from.
forensics_directory_list:
  - /var/log
  - /tmp
  - /var/tmp
  - /var/spool/cron
  - /var/spool/anacron
  - /etc/cron.d
  - /etc/cron.daily
  - /etc/cron.hourly
  - /etc/cron.monthly
  - /etc/cron.weekly
  - /var/spool/at

# A list of files to collect.
forensics_file_list:
  - /etc/passwd
  - /etc/group
  - /etc/shadow

# A list of directories and patterns to collect.
forensics_specific_file_list:
  - path: /root
    pattern: .authorized_keys
  - path: /root
    pattern: .bash_history
  - path: /root
    pattern: .history
  - path: /home
    pattern: .authorized_keys
  - path: /home
    pattern: .bash_history
  - path: /home
    pattern: .history
```

## Requirements

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap

```

## Context

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/forensics.png "Dependency")

## Compatibility

This role has been tested on these [container images](https://hub.docker.com/):

|container|tags|
|---------|----|
|alpine|all|
|debian|all|
|el|7, 8|
|fedora|all|
|opensuse|all|
|ubuntu|bionic|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.



## Testing

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-forensics) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-forensics/issues)

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

## License

Apache-2.0


## Author Information

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
