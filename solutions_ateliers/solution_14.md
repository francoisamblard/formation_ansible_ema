# Exercice

## Playbook `pkg-info.yml`

```
---
- name: Display package manager information
  hosts: all
  gather_facts: yes
  tasks:
    - name: Show package manager
      debug:
        msg: "Package manager is {{ ansible_pkg_mgr }}"
...
```

## Playbook `python-info.yml`

```
---
- name: Display Python version
  hosts: all
  gather_facts: yes
  tasks:
    - name: Show Python version
      debug:
        msg: "Python version is {{ ansible_python.version.full }}"
...
```

## Playbook `dns-info.yml`

```
---
- name: Display DNS servers
  hosts: all
  gather_facts: yes
  tasks:
    - name: Show DNS servers
      debug:
        msg: "DNS servers are {{ ansible_dns.nameservers }}"
...
```