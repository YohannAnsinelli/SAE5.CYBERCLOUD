#### Ansinelli Yohann, Ralite Justin, Mathéo Balazuc

<br />

# <center>SAE5.CYBERCLOUD</center>

<br />

## <b><u>I/ Présentation SAE</u></b>

Cette SAE a pour objectif de créer un environnement d'entraînement pour les équipes "blue team et red team" de notre entreprise. Nous devions mettre en oeuvre un environnement technique permettant de simuler un environnement de production Windows à la fois sur VirtualBox et sur Proxmox. Nous sommes un groupe composé de trois membres, qui avait chacun différentes tâches à remplir. 

## <b><u>II/ Déploiement Environnement</u></b>

La première tâche consistait à faire le déploiement de l'environnement qui est basé sur le projet GOAD d'Orange Cyberdéfense qui permet de déployer un Active Directory vulnérable :

* ### <u><b> VirtualBox </b></u>

    Pour réaliser le déploiement sur VirtualBox il est nécessaire d'avoir au préalable installer VirtualBox, Vagrant ainsi que Ansible :

        ###Installation VirtualBox###

        sudo apt install virtualbox

        ###Installation Vagrant###

        wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

        echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

        sudo apt update && sudo apt install vagrant 
        
        ###Installation Ansible + Git Clone###

        sudo apt install git
        git clone git@github.com:Orange-Cyberdefense/GOAD.git
        cd GOAD/ansible
        sudo apt install python3.8-venv
        python3.8 -m virtualenv .venv
        source .venv/bin/activate

        python3 -m pip install --upgrade pip
        python3 -m pip install ansible-core==2.12.6
        python3 -m pip install pywinrm

    Une fois les prérequis installés on peut lancer automatiquement l'installation : 

        ./goad.sh -t check -l GOAD -p virtualbox -m local

    Une fois l'installation terminée on observera sur virtualbox l'ensemble des DC et SRV appraître sous cette forme :

    ![Alt text](Photo_SAECLOUDCYBER\VirtualBox.png)

* ### <u><b> Proxmox </b></u>

## <b><u>III/ Répartitions des tâches</u></b>

Le support utilisé pour la répartition des tâches a été trello, il permet d'organiser et de gérer visuellement et facilement le projet. Voici une photo exemple de notre projet : 

![Alt text](Photo_SAECLOUDCYBER\Trello.png)

Vous pouvez retrouver le Trello de notre groupe en cliquant sur ce lien ce mot **[Yojuma](https://trello.com/invite/b/7TZ5XEJ0/ATTI6f183bf588a9821f81585e399a62ff8dED5283D7/yojuma34500)**

Pour ce qui est du récapitulatif des heures passées sur chaque tâche, vous pouvez retrouver le tableau ci-dessous :

|       |   <center>Membre</center>    |   <center>Nombre(s) d'Heure(s)</center>    |   <center>Commentaire(s)</center>    |
|---    |:-:    |:-:    |:-:    |
|<center>GOAD VirtualBox</center>       |   Justin    |       |       |
|<center>GOAD Proxmox</center>       |   Mathéo    |       |       |
|<center>iDRAC</center>       |   Mathéo    |       |       |
|<center>Ansible - Automatisation</center>       |    Justin   |       |       |
|<center>Elastic VirtualBox</center>       |    Yohann - Justin   |       |       |
|<center>Elastic Proxmox</center>       |   Mathéo    |       |       |
|<center>Wazuh</center>       |   Yohann - Justin    |    8h   |       |
|<center>OpenWEC</center>       |   Yohann    |   24h    |   Déploiement du serveur + client OpenWEC pour le DC03 / Beaucoup de problèmes avant de parvenir à réussir.    |
|<center>BloodHound</center>       |   Justin    |       |       |
|<center>Splunk</center>       |   Yohann    |   8h    |       |
|<center>Chainsaw - Hayabusa</center>       |   Yohann    |    4h   |   Installation + Chasse globale + Répartion des différents events par ID / Pas de difficultés particulières de rencontrées, mais un manque de temps pour pouvoir mieux manipuler les outils.    |
|<center>Autid</center>       |   Yohann    |   5h    | • <u>Côté linux </u> :<br/> Installation du paquet auditd avec la mise en place d'une surveillance sur le fichier "/etc/passwd". <br/><br/>• <u>Côté Windows </u> :<br/> Activation de audit pour réaliser la surveillance du fichier log de Splunk.  |
|<center>pfSense</center>       |    Mathéo   |       |       |
|<center>Schéma Réseau VirtualBox</center>       |    Yohann   |   2h    |   Création rapide du schéma de notre réseau sur VirtualBox avec l'ajout de petit logo pour faciliter la compréhension de chaque machine sur le schéma.    |
|<center>Schéma Réseau Proxmox</center>       |    Mathéo   |       |       |
|<center>Attaques</center>       |    Mathéo - Justin   |       |       |

**⚠️ ATTENTION ⚠️** Les heures passées sur chaque tâches ne prennent pas en compte la rédaction des différents comptes rendus. 
    
## <b><u>IV/ Elasitc</u></b>

* ### <u><b> VirtualBox </b></u>

* ### <u><b> Proxmox </b></u>

## <b><u>V/ Wazuh</u></b>

## <b><u>VI/ BloodHound</u></b>

## <b><u>VII/ OpenWEC</u></b>

OpenWEC est une implémentation gratuite et opensource d'un serveur Windows Event Collector. Il va permettre de collecter les journaux d'événements Windows à partir d'une machine Linux sans avoir besoin d'un agent local tiers exécuté sur les machines Windows. Il va être composé de choses :

* **openwecd** qui est le serveur OpenWEC.
* **openwec** qui est utilisé pour gérer le serveur OpenWEC.

Dans notre groupe c'est Yohann qui s'est chargé de la configuration de OpenWEC. Il y a passé environ 24 heures suite à de nombreux problèmes rencontrés. Le principal problème de OpenWEC et son fonctionnement avec Kerberos qui peut parfois nous causer quelques soucis, de plus le DC01 à eu un problème de "computers" qui ne remontait pas, par conséquent il a fallu faire la configuration premièrement sur le DC03. Au bilan OpenWEC a réussit à fonctionner et nous a permis de détecter les machines du DC03 et de pouvoir accèder à leurs logs :

<img src="Photo_SAECLOUDCYBER\OpenWECstats.png">

Exemple de fichier de log de la machine "MEEREN" :

<img src="Photo_SAECLOUDCYBER\LogMEEREN.png">

⭐ Vous pouvez retrouver notre compte rendu sur OpenWEC ainsi qu'un fichier de log sur notre github au chemin suivant :

:octocat: Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ OPENWEC ➔ SAE_OPENWEC_INSTALL.pdf

* **Fichier de log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ OPENWEC ➔ openwec_logs

## <b><u>VIII/ Splunk</u></b>

Au cours de cette SAE on a pu découvrir Splunk, c'est une plateforme qui va permettre d'obtenir des informations sur d'innnombrables sources de données. Dans notre cas Splunk va avoir un peu la même utilisateur que Wazuh et Elastic.
Une des particularité de Splunk c'est qu'il faut obligatoirement créer un compte pour pouvoir télécharger le paquet pour installer le serveur et le forwarder côté client. L'installation reste simple, elle est accompagnée d'un assistant pour facilité cette dernière. Une fois installer, le serveur est accessible sur le port 8000. Sur le serveur pour pouvoir écouter les clients, on va venir créer un receveur sur le port 9997 (par défaut). Maintenant côté client il s'agit d'un exécutable où il va falloir préciser l'adresse IP du serveur ainsi que les ports par défaut notamment celui du receveur 9997. Le forwarder ne suffit pas à recevoir les informations des clients windows, il faut également télécharger un addon et modifier un fichier de configuration pour enfin pour observer l'apparition de nos agents. En voici un exemple :

<img src="Photo_SAECLOUDCYBER\clientsplunk.png"> 

L'installation et la configuration est très simple mais demande un petit temps si elle n'est pas faite de manière automatique à l'aide de ansible. Dans notre cas on a également rajouter la réception des logs sysmon sur le serveur avec l'ajout du port UDP 514 en écoute et côté client l'ajout de deux petits paramètres sur un fichier de conf. On observera bien par la suite la remontée de log sysmon :

<img src="Photo_SAECLOUDCYBER\sysmon_log.png"> 

⭐ Vous pouvez retrouver notre compte rendu sur Splunk ainsi qu'un fichier de log et un fichier .evtx sur notre github au chemin suivant :

:octocat: Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ SAE_INSTALL_SPLUNK.pdf

* **Fichier de log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ SPLUNK_LOGS-2023-12-07.pdf

* **Fichier evtx** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ sysmon_log.evtx

## <b><u>IX/ Auditd - Chainsaw - Hayabusa</u></b>

## <b><u>X/ Attaques</u></b>

## <b><u>XI/ Schéma Réseau</u></b>

Pour cette SAE, il fallait réaliser la conception d'un schéma réseau, hors au vu de l'organisation de la SAE qui met en parallèle VirtualBox et Proxmox, il paraissait compliqué de réunir tout sur un même schéma, notre groupe a par conséquent de réaliser un schéma pour VirtualBox et un schéma pour Proxmox :

* ### <u><b> VirtualBox </b></u>

<img src="Schéma Réseau.png"> 

* ### <u><b> Proxmox </b></u>

## <b><u>XII/ Conclusion</u></b>

Ne pas hésiter à mettre ici ce qu'on a fait en plus entre lorsque le prof est passé et maintenant !






