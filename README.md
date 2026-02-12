# [Ansible role consul_ca](#ansible-role-consul_ca)

Install and configure Consul CA on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-consul_ca/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-consul_ca/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-consul_ca/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-consul_ca)|[![downloads](https://img.shields.io/ansible/role/d/buluma/consul_ca)](https://galaxy.ansible.com/buluma/consul_ca)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-consul_ca.svg)](https://github.com/buluma/ansible-role-consul_ca/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-consul_ca/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  tasks:
    - name: "Include buluma.consul_ca"
      ansible.builtin.include_role:
        name: "buluma.consul_ca"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-consul_ca/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: buluma.bootstrap
    - role: buluma.ca_certificates
    - role: buluma.core_dependencies
    - role: buluma.go
      go_version: "1.22.5"

  tasks:
    - name: verify that Go is installed and available in the $PATH.
      command: go version
      environment:
        PATH: /usr/local/go/bin:{{ ansible_env.PATH }}
      changed_when: false
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-consul_ca/blob/master/defaults/main.yml):

```yaml
---
# defaults file for consul_ca
# Where to store the CA and certificate files. By default this
# will expand to user's LOCAL $HOME (the user that run's "ansible-playbook ..."
# plus "/consul/ssl". That means if the user's $HOME directory is e.g.
# "/home/da_user" then "consul_ca_conf_directory" will have a value of
# "/home/da_user/consul/ssl".
consul_ca_conf_directory: "{{ '~/consul/ssl' | expanduser }}"
# The user who own's the certificate directory and files
consul_ca_certificate_owner: "root"
# The group which own's the certificate directory and files
consul_ca_certificate_group: "root"

# Expiry for Consul root certificate
ca_consul_expiry: "87600h"

#
# Certificate authority for Consul certificates
#
ca_consul_csr_cn: "Consul"
ca_consul_csr_key_algo: "rsa"
ca_consul_csr_key_size: "2048"
ca_consul_csr_names_c: "DE"
ca_consul_csr_names_l: "The_Internet"
ca_consul_csr_names_o: "Consul"
ca_consul_csr_names_ou: "BY"
ca_consul_csr_names_st: "Bayern"

#
# CSR parameter for Consul certificate
#
consul_csr_cn: "server.dc1.consul"
consul_csr_key_algo: "rsa"
consul_csr_key_size: "2048"
consul_csr_names_c: "DE"
consul_csr_names_l: "The_Internet"
consul_csr_names_o: "Consul"
consul_csr_names_ou: "BY"
consul_csr_names_st: "Bayern"

# Specifies the version of CFSSL toolkit we want to download and use
cfssl_version: "1.6.5"

# Checksum file
cfssl_checksum_url: "https://github.com/cloudflare/cfssl/releases/download/v{{ cfssl_version }}/cfssl_{{ cfssl_version }}_checksums.txt"

# The directory where CFSSL binaries will be installed
cfssl_bin_directory: "/usr/local/bin"

# Owner of the cfssl binaries
cfssl_owner: "root"

# Group of cfssl binaries
cfssl_group: "root"

# Operarting system on which "cfssl/cfssljson" should run on
cfssl_os: "linux"  # use "darwin" for MacOS X, "windows" for Windows

# Processor architecture "cfssl/cfssljson" should run on
cfssl_arch: "amd64"  # the only supported architecture at the moment
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-consul_ca/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.go](https://galaxy.ansible.com/buluma/go)|[![Build Status GitHub](https://github.com/buluma/ansible-role-go/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-go/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-go/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-go)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Build Status GitHub](https://github.com/buluma/ansible-role-ca_certificates/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-ca_certificates/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-ca_certificates)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-consul_ca/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Amazon](https://hub.docker.com/r/buluma/amazonlinux)|Candidate|
|[Debian](https://hub.docker.com/r/buluma/debian)|bullseye|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-consul_ca/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-consul_ca/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

