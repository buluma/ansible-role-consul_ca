# Ansible role [consul_ca](https://galaxy.ansible.com/ui/standalone/roles/buluma/consul_ca/documentation)

Install and configure Consul CA on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-consul_ca/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-consul_ca/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-consul_ca.svg)](https://github.com/buluma/ansible-role-consul_ca/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-consul_ca.svg)](https://github.com/buluma/ansible-role-consul_ca/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-consul_ca.svg)](https://github.com/buluma/ansible-role-consul_ca/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/consul_ca)](https://galaxy.ansible.com/ui/standalone/roles/buluma/consul_ca/documentation)|

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

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.go](https://galaxy.ansible.com/buluma/go)|[![Ansible Molecule](https://github.com/buluma/ansible-role-go/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-go/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-go.svg)](https://github.com/shadowwalker/ansible-role-go)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ca_certificates.svg)](https://github.com/shadowwalker/ansible-role-ca_certificates)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

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

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-consul_ca/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-consul_ca/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-consul_ca/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)
