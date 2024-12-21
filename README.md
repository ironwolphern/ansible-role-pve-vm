# **Ansible Role PVE VM**

![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=flat&logo=ansible&logoColor=white)
![Proxmox](https://img.shields.io/badge/Proxmox-%23E57000?style=flat&logo=proxmox&logoColor=white)
![GitHub License](https://img.shields.io/github/license/ironwolphern/ansible-role-pve-vm)
![GitHub release (with filter)](https://img.shields.io/github/v/release/ironwolphern/ansible-role-pve-vm)
![GitHub pull requests](https://img.shields.io/github/issues-pr/ironwolphern/ansible-role-pve-vm)
![GitHub closed pull requests](https://img.shields.io/github/issues-pr-closed/ironwolphern/ansible-role-pve-vm)
![GitHub issues](https://img.shields.io/github/issues/ironwolphern/ansible-role-pve-vm)
[![Ansible Lint](https://github.com/ironwolphern/ansible-role-pve-vm/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/ironwolphern/ansible-role-pve-vm/actions/workflows/ansible-lint.yml)
![Dependabot](https://badgen.net/github/dependabot/ironwolphern/ansible-role-pve-vm)

This is an Ansible role to manage Proxmox VE virtual machines with the Proxmox VE API. For more information of Proxmox VE please visit the official website: https://www.proxmox.com

## *Requirements*

This role requires the ***community.general*** Ansible collection to be installed, as it utilizes the following modules:

- `proxmox_kvm`: For managing Proxmox VE KVM virtual machines
- `proxmox_vm_info`: For gathering information about Proxmox VE virtual machines
- `proxmox_disk`: For managing storage and disks of Proxmox VE virtual machines

You can install this collections with requirements file:
```shell
  ansible-galaxy collection install -r requirements.yml
```

These modules also require the following libraries to be installed:

- `proxmoxer`
- `requests`

## *Role Variables*

This is a list of required and optinal variables and parameters for this role:

| **Parameter** | **Description** | **Type** | **Default** | **Options** | **Required** |
|---------------|-----------------|----------|:-----------:|:-----------:|:------------:|
| pve_vm__api_host | api host name  | string | localhost |  | yes |
| pve_vm__api_user | api user name  | string | root@pam |  | yes |
| pve_vm__api_password | api user password  | string |  |  | yes |
| pve_vm__validate_certs | validate certificates  | boolean | true | true/false | no |
| pve_vm__node | node name  | string | pve |  | yes |
| pve_vm__vm_os_type | os type  | string | linux | linux/windows | yes |
| pve_vm__vm_os_name | os name  | string | ubuntu | ubuntu/debian/centos/fedora/archlinux/opensuse/alpine/flatcar | yes |
| pve_vm__provision_type | vm provision type  | string | tpl | tpl/iso | yes |
| pve_vm__vm_settings | vm settings  | dictionary |  |  | yes |
| pve_vm__vm_force_delete | force delete vm  | boolean | false | true/false | no |
| pve_vm__vm_force_stop | force stop vm  | boolean | true | true/false | no |
| pve_vm__vm_template | vm template settings | dictionary |  |  | yes |
| pve_vm__user_data | user data settings | dictionary(list) |  |  | no |
| pve_vm__network_config | network config settings | dictionary |  |  | no |

## *Dependencies*

There are no dependencies.

## *Example Playbook*

This is an example of use role with optionals and required parameters:

*play-pve-vm.yml*
```yaml
    - name: "Mange VMs on Proxmox VE"
      hosts: localhost
      gather_facts: true

      roles:
        - role: ansible-role-pve-vm
          vars:
            pve_vm__api_host: "pve.example.local"
            pve_vm__api_user: "root@pam"
            pve_vm__api_password: "ChangeMe"
            pve_vm__node: "pve"
            pve_vm__vm_os_type: "linux"
            pve_vm__vm_os_name: "ubuntu"
            pve_vm__provision_type: "iso"
            pve_vm__vm_settings:
              name: "test-vm"
              domain: "example.local"
              cores: 1
              sockets: 1
              machine: "q35"
              memory: 512
              net: "vmbr0"
              iso_storage: "local"
              iso: "image.iso"
              storage: "local-lvm"
              disk_size: 10
```

These are some examples of the use of this playbook with the different tags that can be used in this role:

Download template vm with cloud-init config
```bash
  ansible-playbook -i inventory play-pve-vm.yml -t download
```

Create vm with cloud-init config
```bash
  ansible-playbook -i inventory play-pve-vm.yml -t provision
```

Delete vm
```bash
  ansible-playbook -i inventory play-pve-vm.yml -t erase
```

Unprotect vm to erase
```bash
  ansible-playbook -i inventory play-pve-vm.yml -t unprotect
```

## *License*

MIT

## *Author Information*

This role was created in 2024 by:

- Fernando Hernández San Felipe (ironwolphern@outlook.com)
