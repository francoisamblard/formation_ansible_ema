# Exercice

## Playbook `chrony.yml`

```
---  # Playbook pour installer et configurer Chrony sur les cibles
- name: Install and configure Chrony
  hosts: all
  become: true
  tasks:
    - name: Install chrony package
      package:
        name: "{{ chrony_package }}"
        state: present

    - name: Copy chrony configuration file
      template:
        src: chrony.conf.j2
        dest: "{{ chrony_confdir }}"
        mode: '0644'
      notify: restart chrony

    - name: Ensure chrony service is started and enabled
      service:
        name: chronyd
        state: started
        enabled: true

  vars:
    chrony_package: "{{ 'chrony' if ansible_facts['os_family'] in ['Debian', 'Ubuntu'] else 'chrony' }}"
    chrony_confdir: |
      {% if ansible_facts['os_family'] == 'Debian' %}
        /etc/chrony/chrony.conf
      {% elif ansible_facts['os_family'] == 'RedHat' %}
        /etc/chrony.conf
      {% elif ansible_facts['os_family'] == 'Suse' %}
        /etc/chrony.conf
      {% endif %}

  handlers:
    - name: restart chrony
      service:
        name: chronyd
        state: restarted
...
```

## Fichier de template `chrony.conf.j2`

```
# {{ ansible_managed }}

server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```