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
`Method` : `Create an internal certificate Authority`
`Common Name` : Ce sera le nom affiché dans les certificats.
Sauvegarder.

L'authorité de certification apparait.

### Créer le certificat Server  

Aller dans `System`>`Cert. Manager`>`Certificates`
Dans `Certificate type`, il faut bien indiquer `Server type`.
Le "Common Name" correspond au nom intégré dans le certificat.
Sauvegarder.  

## Créer les utilisateurs locaux  

Dans `System`>`User Manager`>`Users`, cliquer sur "Add"
**Il faut indiquer notre certificat d'authorité**, cela va ajouter le formulaire de création du certificat juste en dessous. Pour créer le certificat, on se base sur notre autorité de certificationet créer un nom utilisateur et un mot de passe pour cet utilisateur

![Capture d'écran 2024-12-03 110742](https://github.com/user-attachments/assets/0e286ad6-9e11-4336-a5c1-5e8867cce25e)

Ensuite une fois enregistré, l'utilisateur apparait dans la liste des utilisateurs de la base locale.
![Capture d'écran 2024-12-03 111055](https://github.com/user-attachments/assets/504928b7-d460-45e0-9fef-20f675b48e8c)
