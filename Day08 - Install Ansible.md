# Day 8: Install Ansible

## Question

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with `Ansible` for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use `jump host` as an Ansible controller to test different kind of tasks on rest of the servers.

**Task:**  

Install `ansible` version `4.7.0` on `Jump host` using `pip3` only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

---

## Solution

Install Ansible version 4.7.0 globally using pip3
We use the `--prefix=/usr/local` to make it available for all users.

```bash
sudo pip3 install ansible==4.7.0
```

---

## Verification

Check version:

```bash
ansible --version
pip show ansible
```

**Example Output:**

```bash
thor@jumphost ~$ sudo pip3 install ansible==4.7.0
Successfully installed MarkupSafe-3.0.2 ansible-4.7.0 ansible-core-2.11.12 jinja2-3.1.6 packaging-25.0 resolvelib-0.5.4
thor@jumphost ~$ ansible --version
ansible [core 2.11.12] 
  python version = 3.9.18 (main, Jan 24 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)]
  jinja version = 3.1.6
  libyaml = True
thor@jumphost ~$ pip show ansible
Name: ansible
Version: 4.7.0
Summary: Radically simple IT automation
Home-page: https://ansible.com/
Author: Ansible, Inc.
Author-email: info@ansible.com
License: GPLv3+
Location: /usr/local/lib/python3.9/site-packages
Requires: ansible-core
Required-by:
```

