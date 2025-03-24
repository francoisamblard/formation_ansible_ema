# Exercice  
`sudo vim /etc/hosts`

Ajouter le texte suivant dans /etc/hosts :  
```
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  target01.sandbox.lan    target01
192.168.56.20  target02.sandbox.lan      target02
192.168.56.30  target03.sandbox.lan     target03
```  

`ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts`

Autoriser la connexion ssh par mot de passe sur les target.  

`ssh-keygen`  

Copier la clef ssh sur les cibles :
```
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```

Vérifier que les ping fonctionnent