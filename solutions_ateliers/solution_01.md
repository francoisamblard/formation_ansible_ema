# Exercice 1
Démarrez la VM ubuntu depuis le répertoire atelier-01.  
`vagrant up ubuntu`

Connectez-vous à cette VM.  
`vagrant ssh ubuntu`

Rafraîchissez les informations sur les paquets.  
`sudo apt update`

Recherchez le paquet ansible avec les options qui vont bien.  
`apt-cache search --names-only ansible`

Installez le paquet officiel fourni par la distribution.  
`sudo apt install -y ansible`

Vérifiez si l’installation s’est bien déroulée.  
`ansible --version`

Notez la version d’Ansible.  
`ansible [core 2.17.9]`

Déconnectez-vous et supprimez la VM.  
`exit`
`vagrant destroy -f ubuntu`

# Exercice 2
Répétez l’exercice précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :  
`sudo apt-add-repository ppa:ansible/ansible`

Notez la version fournie par ce dépôt tiers et comparez avec la version officielle de l’exercice précédent.  
`ansible [core 2.17.9]`

# Exercice 3
`vagrant up rocky`  
`vagrant ssh rocky`  
`python3 -m venv ~/.venv/ansible`  
`source ~/.venv/ansible/bin/activate`  
`pip install --upgrade pip`  
`pip install ansible`  
`ansible --version`  
`deactivate`  