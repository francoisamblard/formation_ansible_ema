# Exercice : Comprendre l'idempotence avec Ansible

* Installation des paquets `tree`, `git` et `nmap` sur toutes les cibles

```sh
$ ansible all -m package -a "name=tree state=present"
$ ansible all -m package -a "name=git state=present"
$ ansible all -m package -a "name=nmap state=present"
```

* Desinstallation des paquets :  

```sh
$ ansible all -m package -a "name=tree state=absent"
$ ansible all -m package -a "name=git state=absent"
$ ansible all -m package -a "name=nmap state=absent"
```
* Copier le fichier /etc/fstab du Control Host vers tous les Target Hosts sous forme d’un fichier /tmp/test3.txt.  

```sh
$ ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt mode=0644"
```  

* Supprimez le fichier /tmp/test3.txt sur les Target Hosts en utilisant le module file avec le paramètre state=absent.  

```sh
$ ansible all -m file -a "path=/tmp/test3.txt state=absent"
```  

* Enfin, affichez l’espace utilisé par la partition principale sur tous les Target Hosts. Que remarquez-vous ?  

```sh
$ ansible all -m command -a "df -h /"
```

## Observations :
* Une action est appliquée si nécessaire lors de la première exécution.

* Les exécutions suivantes ne provoquent aucun changement si l'état souhaité est déjà atteint.

* Le module command, quant à lui, ne respecte pas cette logique et s’exécute systématiquement.