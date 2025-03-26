# Exercice

## Playbook `kernel.yml`

```
---  # kernel.yml

- hosts: all
  gather_facts: false

  tasks:
    - name: Get kernel information
      command: uname -a
      changed_when: false
      register: kernel_info

    - name: Display kernel information (using msg)
      debug:
        msg: "{{ kernel_info.stdout }}"

    - name: Display kernel information (using var)
      debug:
        var: kernel_info.stdout
...
```

## Playbook `packages.yml`

```
---  # packages.yml

- hosts: rocky,suse
  gather_facts: false

  tasks:
    - name: Count installed RPM packages
      command: rpm -qa | wc -l
      changed_when: false
      register: rpm_count

    - name: Display number of installed RPM packages
      debug:
        msg: "Total installed RPM packages: {{ rpm_count.stdout }}"
...
```