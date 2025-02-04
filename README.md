# dhcp-linux

***https://www.it-connect.fr/configurer-adresse-ip-statique-netplan-ubuntu-ou-debian/***

- Configure la carte réseau de ta machine virtuelle en Réseau Interne
**Sur Ubuntu, la configuration réseau se fait principalement via Netplan pour les versions récentes. Nous allons modifier le fichier de configuration Netplan pour définir notre adresse IP statique.**

**Ouvrez un terminal et exécutez la commande suivante pour éditer le fichier de configuration (sous Debian, ce fichier doit être créé) :**

***sudo nano /etc/netplan/50-cloud-init.yaml***

![chemin adreese ip](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/chemin-add-ip.png)

![config adreese ip](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/addresseIp.png)


Puis, nous allons modifier les permissions sur ce fichier de configuration.
Voici la commande à exécuter :
sudo chmod 600 /etc/netplan/50-cloud-init.yaml

![adresse ip](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/droits.png)

***Appliquer la configuration Netplan***

Utilisez les commandes suivantes pour générer la configuration et l'appliquer auprès du gestionnaire de réseau du système :

***sudo netplan generate***
***sudo netplan apply***
![active @ ip](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/activer-add.png)

**vérifier la configuration**
Pour afficher et vérifier votre nouvelle configuration réseau, vous pouvez utiliser les options spécifiques de Netplan, à la place de la traditionnelle commande "ip a". Voici plusieurs commandes pour afficher la configuration complète ou celle d'une carte réseau spécifique.

- sudo netplan get
- sudo netplan status enp0s3
- sudo netplan status --all
- ip a show enp0s3

![vérifier adresse ip get](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/netplan-get.png)
![vérifier adresse ip ens0s3](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/netplan-enp0s3.png)
![vérifier adresse ip all](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/NETPLAN-ALL.png)
![vérifier adresse ip a ](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/ip-a.png)


- Configure l'interface réseau du serveur

**Exécutez la commande ci-dessous pour installer le package serveur DCHP**
![install dhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/install-dhcp.png)

**Une fois l'installation terminée, éditez le fichier /etc/default/isc-dhcp-server pour définir les interfaces que DHCPD doit utiliser pour traiter les requêtes DHCP, avec l'option INTERFACES.**
![config fichier dhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/config-fich-dhcp.png)

- Configure le service DHCP et met en place une adresse statique par adresse MAC pour un client particulier

**Le fichier de configuration DHCP principal est /etc/dhcp/dhcpd.conf, vous devez ajouter toutes les informations de votre réseau à envoyer aux clients ici**

***sudo vi /etc/dhcp/dhcpd.conf***

![config bail dhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/configbail.png)

![la plage dhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/palgedhcp.png)

**Redémarrer le service DHCP**
sudo systemctl restart isc-dhcp-server
Vérifier son état:
sudo systemctl status isc-dhcp-server

![restart dhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/restartDhcp.png)    


# Configuration et test du client windows 

![conf-ip4-clt](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/conf-ip4-clt.png)    
![adressecltdhcp](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/adressecltdhcp.png)   

# Configuration et test du client LUNIX 
    ip a show enp0s3
![adressecltdhcp lunix](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/clt-lunix.png)

- Mettre en place une attribution statique pour une machine cliente particulière dont l'adresse MAC permet d'obtenir l'adresse 172.20.0.10
    Éditer le fichier /etc/dhcp/dhcpd.conf :
![adresse statique](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/clt-statique.png)

    Redémarrer le service DHCP :
***sudo systemctl restart isc-dhcp-server***
***sudo systemctl enable isc-dhcp-server***
***sudo systemctl status isc-dhcp-server***
![activer adresse statique](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/activerAdresseStatique.png)

- Test la bon fonctionnement du serveur avec un client classique et le client devant avoir une adresse statique
    ip a show enp0s3
![test adresse statique](https://github.com/KAOUTARBAH/DHCP-LINUX/blob/main/images/adresse-statique.png)



