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

## Configurer le serveur OpenVPN

Aller dans `VPN`>`OpenVPN`
Dans "Servers", cliquer en bas sur "Add".
La première chose à faire, c'est de choisir le "Server Mode" suivant : Remote Access (SSL/TLS + User Auth).
Pour le VPN, le protocole s'appuie sur de l'UDP, avec le port 1194 par défaut : je vous recommande d'utiliser un port différent. Pour l'interface, nous allons conserver "WAN" puisque c'est bien par cette interface que l'on va se connecter en accès distant.

Au niveau de la partie chiffrement, un peu plus bas dans la page, vous devez sélectionner votre autorité de certification au niveau du champ "Peer Certificate Authority". En complément, sélectionnez le certificat Server au niveau du champ "Server certificate".  
On peut également modifier l'algorythme de chiffrement pour améliorer la sécurité, mais on peut ressentir un lag avec une chiffrement plus complexe.  
![Capture d'écran 2024-12-03 111755](https://github.com/user-attachments/assets/db246a89-6afd-4b10-bcfa-0769077f14fd)  

Il faut paramétrer le réseau du tunnel VPN par exemple : 10.10.10.0/24 (IPv4 Tunnel Network)  
Dans `Redirect IPv4 Gateway`, si on coche `Force all client-generated IPv4 traffic through the tunnel`, tout le flux de l'IP cliente passera par le tunnel. Dans notre cas, ne pas cocher, puis dans `IPv4 Local network`, rentrer le réseau de l'entreprise que l'on veut accéder par VPN, ici : 192.168.1.0/24  
Dans `Dynamic IP`, si on coche, ça pemret aux clients en DHCP de conserver la connexion au cas où leur adresse change.  
Au niveau de la "Topology", remarque très importante à prendre en compte : pour des raisons de sécurité, il vaut mieux utiliser la topologie "net30 - isolated /30 network per client" pour que chaque client soit isolé dans un sous-réseau (de la plage réseau VPN) afin que les clients ne puissent pas communiquer entre eux !

## Exporter la configuration OpenVPN

Pour télécharger la configuration au format ".ovpn", il est nécessaire d'installer un paquet supplémentaire sur notre pare-feu. Rendez-vous dans le menu suivant : System > Package Manager > Available Packages.  
Recherchez "openvpn" et installez le paquet : openvpn-client-export.  
Lorsque c'est fait, retournez dans le menu "OpenVPN" puis dans l'onglet "Client Export".  

Si on souhaite à la fin de la page dans OpenVPN Clients, on peut télécharger la configuration. En sélectionnant `Bundled Configurations` et "Archive".  
![Capture d'écran 2024-12-03 190529](https://github.com/user-attachments/assets/579942af-91c9-4b4e-bda9-e4285f232983)


## Créer les règles de firewall pour OpenVPN  

`FireWall`>`Rules`>`Edit`  

![Capture d'écran 2024-12-03 191112](https://github.com/user-attachments/assets/d36bfbbf-74d3-410a-99ba-890896ea3e8d)
