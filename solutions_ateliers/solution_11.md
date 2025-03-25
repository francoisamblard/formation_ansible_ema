# Exercice 

## Playbook `chrony.yml`

```
---  # chrony.yml

- hosts: redhat
  become: yes

  tasks:
    - name: Install Chrony
      dnf:
        name: chrony
        state: present

    - name: Configure Chrony
      copy:
        dest: /etc/chrony.conf
        content: |
          pool 0.pool.ntp.org iburst
          pool 1.pool.ntp.org iburst
          pool 2.pool.ntp.org iburst
          driftfile /var/lib/chrony/chrony.drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart Chrony

    - name: Enable and start Chrony
      service:
        name: chronyd
        state: started
        enabled: true

  handlers:
    - name: Restart Chrony
      service:
        name: chronyd
        state: restarted
...
```