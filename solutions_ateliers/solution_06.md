# Exercice

* Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.  
```
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  target01.sandbox.lan    target01
192.168.56.20  target02.sandbox.lan      target02
192.168.56.30  target03.sandbox.lan     target03
```  

* Configurez l’authentification par clé SSH avec les trois Target Hosts.  
`ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts`  
`ssh-keygen`  
```
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```  


* Installez Ansible.  
`sudo apt update`  
`sudo apt install -y ansible`  

* Envoyez un premier ping Ansible sans configuration.  
`ansible all -m ping`  
On a un message d'avertissement car on a pas encore d'inventaire de configuré.  

* Créez un répertoire de projet ~/monprojet.  
`mkdir monprojet`  

* Créez un fichier vide ansible.cfg dans ce répertoire.  
`touch ansible.cfg`  

* Vérifiez si ce fichier est bien pris en compte par Ansible.  
`ansible --version | head -n 2`  

* Spécifiez un inventaire nommé hosts.  
`vim ansible.cfg`  
On ajoute les lignes suivantes dans le fichier :
```
[defaults]
inventory = ./hosts
```  

* Activez la journalisation dans ~/journal/ansible.log.  
Dans le même fichier, on ajoute :  
`log_path = ~/journal/ansible.log`  

* Testez la journalisation.  
```
ansible all -i target01,target02,target03 -m ping  
cat ~/journal/ansible.log
```  

* Créez un groupe [testlab] avec vos trois Target Hosts.  
On ajoute dans le fichier `~/monprojet/hosts` :  
```
[testlab]
target01
target02
target03
```  

* Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.  
On ajoute dans le fichier `~/monprojet/hosts` :  
```
[testlab:vars]
ansible_user=vagrant
```  

* Envoyez un ping Ansible vers le groupe de machines [all].  
`ansible all -m ping`

* Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.  
On ajoute dans le fichier `~/monprojet/hosts` :  
```
...
ansible_become=yes
```  

* Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.  
`ansible all -a "head -n 3 /etc/shadow"`

* Quittez le Control Host et supprimez toutes les VM de l’atelier.  
`exit`  
`vagrant destroy -f`  
