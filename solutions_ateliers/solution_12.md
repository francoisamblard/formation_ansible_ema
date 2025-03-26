# Exercice

## Playbook `myvars1.yml`
```
---
# myvars1.yml
- name: Display favorite car and bike using play vars
  hosts: localhost
  vars:
    mycar: "Renault"
    mybike: "Yamaha"
  tasks:
    - name: Display mycar
      debug:
        msg: "My favorite car is {{ mycar }}"
    - name: Display mybike
      debug:
        msg: "My favorite bike is {{ mybike }}"
```

## Playbook `myvars2.yml`
```
---
# myvars2.yml
- name: Display favorite car and bike using set_fact
  hosts: localhost
  tasks:
    - name: Set variables
      set_fact:
        mycar: "Renault"
        mybike: "Yamaha"
    - name: Display mycar
      debug:
        msg: "My favorite car is {{ mycar }}"
    - name: Display mybike
      debug:
        msg: "My favorite bike is {{ mybike }}"
```

## Playbook `myvars3.yml`
```
---
# myvars3.yml
- name: Display car and bike from group_vars
  hosts: localhost
  tasks:
    - name: Display mycar
      debug:
        msg: "My favorite car is {{ mycar }}"
    - name: Display mybike
      debug:
        msg: "My favorite bike is {{ mybike }}"
```
Fichier group_vars/all.yml :
* mycar: "VW"
* mybike: "BMW"  

Fichier host_vars/target02.yml :
* mycar: "Mercedes"
* mybike: "Honda"  

## Playbook `display_user.yml`
```
---
# display_user.yml
- name: Prompt for user credentials
  hosts: localhost
  vars_prompt:
    - name: "user"
      prompt: "Enter username"
      private: no
      default: "microlinux"
    - name: "password"
      prompt: "Enter password"
      private: yes
      default: "yatahongaga"
  tasks:
    - name: Display user (without showing password)
      debug:
        msg: "Username is {{ user }}"
```