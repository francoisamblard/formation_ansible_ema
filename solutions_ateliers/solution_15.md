# Exercice

## Playbook `chrony-01.yml`

```
---  # chrony-01.yml
- name: Install and configure Chrony
  hosts: all
  become: yes
  tasks:
    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_os_family == "RedHat"

    - name: Install Chrony on SUSE
      zypper:
        name: chrony
        state: present
      when: ansible_os_family == "Suse"

    - name: Ensure /etc/chrony directory exists
      file:
        name: /etc/chrony
        state: directory
      when: ansible_os_family in ["RedHat", "Suse"]

    - name: Deploy Chrony configuration
      copy:
        dest: /etc/chrony/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart chronyd
  handlers:
    - name: Restart chronyd
      service:
        name: "{{ 'chronyd' if ansible_os_family in ['RedHat', 'Suse'] else 'chrony' }}"
        state: restarted

    - name: Check Chrony status
      command: chronyc tracking
      register: chrony_status
      changed_when: false
      failed_when: chrony_status.rc != 0

    - name: Display Chrony status
      debug:
        var: chrony_status.stdout
...
```

## Playbook `chrony-02.yml`

```
---  # chrony-02.yml
- name: Install and configure Chrony
  hosts: all
  become: yes
  vars:
    chrony_package: "chrony"
    chrony_service: "{{ 'chronyd' if ansible_os_family in ['RedHat', 'Suse'] else 'chrony' }}"
    chrony_confdir: "/etc/chrony/chrony.conf"

  tasks:
    - name: Install Chrony
      package:
        name: "{{ chrony_package }}"
        state: present

    - name: Ensure /etc/chrony directory exists
      file:
        path: /etc/chrony
        state: directory
      when: ansible_os_family in ['RedHat', 'Suse']

    - name: Deploy Chrony configuration
      copy:
        dest: "{{ chrony_confdir }}"
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart chronyd

  handlers:
    - name: Restart chronyd
      service:
        name: "{{ chrony_service }}"
        state: restarted
...
```
