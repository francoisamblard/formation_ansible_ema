# Exercice

## Playbook `apache-debian.yml`

```
---  # apache-01.yml

- hosts: debian

  tasks:

    - name: Update package information
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      apt:
        name: apache2

    - name: Start & enable Apache
      service:
        name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>My first Ansible-managed website</h1>
            </body>
          </html>
...
```

## Playbook `apache-rocky.yml`

```
---
- name: Install and configure Apache on Rocky Linux
  hosts: rocky
  become: yes
  tasks:
    - name: Install Apache
      dnf:
        name: httpd
        state: present

    - name: Start and enable Apache service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Deploy custom index.html
      copy:
        content: "<h1>Welcome to Apache on Rocky Linux</h1>"
        dest: /var/www/html/index.html
...
```

## Playbook `apache-suse.yml`

```
---
- name: Install and configure Apache on SUSE
  hosts: suse
  become: yes
  tasks:
    - name: Install Apache
      zypper:
        name: apache2
        state: present

    - name: Start and enable Apache service
      service:
        name: apache2
        state: started
        enabled: yes

    - name: Deploy custom index.html
      copy:
        content: "<h1>Welcome to Apache on SUSE</h1>"
        dest: /srv/www/htdocs/index.html
...
```