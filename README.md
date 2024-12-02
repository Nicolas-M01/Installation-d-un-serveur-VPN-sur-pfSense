# Installation-d-un-serveur-VPN-sur-pfSense  
---

Récupérer une VM sur laquelle PfSense est installée.
Dans mon cas elle contient 2 cartes réseaux :
* 1 carte en pont en 192.168.1.45/24 en IP statique
* 1 carte en réseau interne 192.168.2.1/24 en IP statique.


## La gestion des certificats  
### Créer authorité de certification  
Aller dans `System`>`Cert. Manager`>`CAs`>`Add`

`Descriptive name` est visible pour PfSense.
`Method` : `Create an internal ertificate Authority`
`Common Name` : Ce sera le nom affiché dans les certificats.
Sauvegarder.

L'authorité de certification apparait.

### Créer le certificat Server  

Aller dans `System`>`Cert. Manager`>`Certificates`
Dans `Certificate type`, il faut bien indiquer `Server type`.
Le "Common Name" correspond au nom intégré dans le certificat.
Sauvegarder.  

## Créer les utilisateurs locaux  

