#### Ansinelli Yohann, Ralite Justin, Mathéo Balazuc

<br />

# <center>SAE5.CYBERCLOUD</center>

<br />

## <b><u>I/ Présentation SAE</u></b>

Cette SAE a pour objectif de créer un environnement d'entraînement pour les équipes "blue team et red team" de notre entreprise. Nous devions mettre en oeuvre un environnement technique permettant de simuler un environnement de production Windows à la fois sur VirtualBox et sur Proxmox. Nous sommes un groupe composé de trois membres, qui avait chacun différentes tâches à remplir. On va notamment pouvoir mettre en place des SIEM, ils permettent de réaliser de la collecte de données, faire un stockage sécurisé de ces dernières, ils vont être très important dans leurs rôles d'analyste et sur les rapports détaillés et vont permettre la détection des incidents de sécurité.

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

    <img src="Photo_SAECLOUDCYBER\VirtualBox.png">

* ### <u><b> Proxmox </b></u>
  L'installation du projet GOAD sur un serveur Proxmox nécessite plusieurs configurations et installations.
   
   **_(L'installation est longue, donc voici un rapide résumé des étapes à suivre. Pour obtenir plus de détails, je vous invite à consulter mon compte rendu [(ici)](https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD/blob/889820262ddf1d18cd4bd4a73ebc8380600d8cf4/(Proxmox)Installation_GOAD_Pfsense_Packer_Terraform_Ansible/Installation-GOAD(Proxmox)-Pfsense-Packer-Terraform-Ansible-SAE-Balazuc-Matheo.pdf).)_**

    Pour commencer, il faut configurer les réseaux du serveur en lui ajoutant des réseaux LAN, WAN et des VLAN.

    ![Alt text](Photo_SAECLOUDCYBER/Network(Proxmox).png)

    Ensuite, on installe la VM **Pfsense** qui nous servira de pare-feu (Firewall) et de serveur DHCP pour les agents GOAD.

    ![Alt text](Photo_SAECLOUDCYBER/Pfsense(Proxmox).png)

    On configure le pare-feu pour les réseaux LAN, WAN et le VLAN10 sur Pfsense.

    Ensuite nous allons créer un template Provisioning
    ![Alt text](Photo_SAECLOUDCYBER/provisioning(proxmox).png)

    **Packer** sera néccesaire pour l'installation de serveur Windows :
    ![Alt text](Photo_SAECLOUDCYBER/packer(proxmox).png)

    **Terraform** nous permettra l'installation des VM de GOAD : 
     ![Alt text](Photo_SAECLOUDCYBER/goad(proxmox).png)

     Et enfin **Ansible** : 

     ![Alt text](Photo_SAECLOUDCYBER/ansible(proxmox).png)




## <b><u>III/ Répartitions des tâches</u></b>

Le support utilisé pour la répartition des tâches a été trello, il permet d'organiser et de gérer visuellement et facilement le projet. Voici une photo exemple de notre projet : 

<img src="Photo_SAECLOUDCYBER\Trello.png">

Vous pouvez retrouver le Trello de notre groupe en cliquant sur ce lien ce mot **[Yojuma](https://trello.com/invite/b/7TZ5XEJ0/ATTI6f183bf588a9821f81585e399a62ff8dED5283D7/yojuma34500)**

Pour ce qui est du récapitulatif des heures passées sur chaque tâche, vous pouvez retrouver le tableau ci-dessous :

|       |   <center>Membre</center>    |   <center>Nombre(s) d'Heure(s)</center>    |   <center>Commentaire(s)</center>    |
|---    |:-:    |:-:    |:-:    |
|<center>GOAD VirtualBox</center>       |   Justin    |    48h   |   Installation sur 3 machines IUT différentes plus debug pour au final finir sur PC Perso   |
|<center>GOAD Proxmox</center>       |   Mathéo    |    72h   |   Déploiement à la main de GOAD, résolution d'erreurs au fur et à mesure.    |
|<center>Ansible Proxmox</center>       |   Mathéo    |    48h   |   Installation d'Ansible sur GOAD avec les scripts, avec la résolution de plusieurs erreurs.    |
|<center>Packer Proxmox</center>       |   Mathéo    |    2h   |   Installation de Packer.    |
|<center>Terraform Proxmox</center>       |   Mathéo    |    2h   |   Installation de Terraform.    |
|<center>iDRAC</center>       |   Mathéo    |    1h   |   Configuration réseau et administration du serveur Dell.    |
|<center>Ansible - Automatisation</center>       |    Justin   |   16h    |    Compréhension de l'utilisation d'API Elastic + mise en corrélation script bash + ansible   |
|<center>Scripts - Automatisation</center>       |    Justin   |   24h    |    Script qui utilisent l'API elastic   |
|<center>Elastic VirtualBox</center>       |    Yohann - Justin   |   Justin(24h)   |    Justin (Script Bash + Ansible)   |
|<center>Elastic Proxmox</center>       |   Mathéo    |   3h    |   Installation du serveur Elastic, des intégrations et déploiement sur les Agents.    |
|<center>Wazuh</center>       |   Yohann - Justin    |    8h   |   Installation plus attaques   |
|<center>Wazuh Proxmox</center>       |   Mathéo    |    1h30   |    Installation du Serveur Wazuh et déploiement sur les Agents GOAD.   |
|<center>OpenWEC</center>       |   Yohann    |   24h    |   Déploiement du serveur + client OpenWEC pour le DC03 / Beaucoup de problèmes avant de parvenir à réussir.    |
|<center>BloodHound</center>       |   Justin    |   0.5h    |       |
|<center>Splunk</center>       |   Yohann    |   8h    |       |
|<center>Chainsaw - Hayabusa</center>       |   Yohann    |    4h   |   Installation + Chasse globale + Répartion des différents events par ID / Pas de difficultés particulières de rencontrées, mais un manque de temps pour pouvoir mieux manipuler les outils.    |
|<center>Autid</center>       |   Yohann    |   5h    | • <u>Côté linux </u> :<br/> Installation du paquet auditd avec la mise en place d'une surveillance sur le fichier "/etc/passwd". <br/><br/>• <u>Côté Windows </u> :<br/> Activation de audit pour réaliser la surveillance du fichier log de Splunk.  |
|<center>pfSense</center>       |    Mathéo   |   6h    |   Ajout de Firewall, WAN, LAN, VLAN's, DHCP Serveur    |
|<center>Schéma Réseau VirtualBox</center>       |    Yohann   |   2h    |   Création rapide du schéma de notre réseau sur VirtualBox avec l'ajout de petit logo pour faciliter la compréhension de chaque machine sur le schéma.    |
|<center>Schéma Réseau Proxmox</center>       |    Mathéo   |   1h    |       |
|<center>Attaques</center>       |     Justin   |      |        |
|<center>Alertes</center>       |    Mathéo  |    3h(Mathéo)   |    (Mathéo: Ajouts de Sigma's, utilisation de APT Simulator)   |

**⚠️ ATTENTION ⚠️** Les heures passées sur chaque tâches ne prennent pas en compte la rédaction des différents comptes rendus. 
    
## <b><u>IV/ Elasitc</u></b>
    
    Elastic est une entreprise proposant Elastic Stack, une plateforme logicielle open source pour la recherche, l'analyse et la visualisation de données. Composée d'Elasticsearch, Logstash, Kibana, et Beats, cette suite d'outils est utilisée pour la gestion des logs, la surveillance, l'analyse de données, et d'autres applications liées à la recherche et à l'analyse de données. Elastic Stack est flexible et scalable, adapté à diverses situations, de la gestion des logs à l'analytique des données, et est largement utilisé dans des contextes allant des petites entreprises aux grandes organisations.</br>
    
* ### <u><b> VirtualBox </b></u>

  Nous avons opté pour une installation d'Elastic automatisé ainsi que de la mise en place des policies et des intégrations automatiquement. Lorsque Elastic plante, celui-ci plante ses agents ainsi que ses policies et ses intégrations, l'ajout 'intégration, des agents ainsi que des policies est donc automatisé pour simplifier la remise en route du Serveur entièrement.
      
  ⭐ Vous pouvez retrouver notre compte rendu sur l'automatisation au chemin suivant :

  🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

    * **Compte Rendu** : SAE5.CYBERCLOUD ➔ Automatisation ➔ ansible.pdf
    
    * **Scripts Installation Elastic** : SAE5.CYBERCLOUD ➔ Automatisation ➔ Installation_elastic
      
    * **Scripts Déploiement Agent** : SAE5.CYBERCLOUD ➔ Automatisation ➔ Déploiement_agents
      
    * **Scripts bash utilisés par Ansible** :  https://github.com/RaliteJ/repo_script_elastic
      
* ### <u><b> Proxmox </b></u>

_**Installation complète détaillée sur mon compte rendu [(ici)](https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD/blob/50967c96a3e9a301410d69b4aa8e74497344c21e/(Proxmox)Installation_Elastic_&_Wazuh/Installation-Elastic(Proxmox)-SAE-Balazuc-Math%C3%A9o.pdf).**_

Installation d'Elastic, Fleet, Kibana, Filebeat :
![Alt text](Photo_SAECLOUDCYBER/docker-elastic(proxmox).png)

Configuration de la policy Elastic pour les agents GOAD, avec leurs intégrations :
 ![Alt text](Photo_SAECLOUDCYBER/policy-elastic(proxmox).png)

Ajout de tout les agents GOAD : 
![Alt text](Photo_SAECLOUDCYBER/agent-elastic(proxmox).png)

Visualisation d'alertes des agents GOAD avec les règles Sigma et le logiciel APT Simulator : 
![Alt text](Photo_SAECLOUDCYBER/alertes-elastic(proxmox).png)
![Alt text](Photo_SAECLOUDCYBER/regles-elastic(proxmox).png)


## <b><u>V/ Wazuh</u></b>
* ### <u><b> VirtualBox </b></u>
Wazuh est une plateforme de gestion de la sécurité open-source qui offre une approche complète pour renforcer la sécurité des entreprises. Grâce à ses fonctionnalités de détection des menaces, de surveillance des journaux et de gestion des incidents, Wazuh permet de détecter rapidement les activités malveillantes, de suivre les incidents de sécurité et de prendre des mesures correctives. La plateforme propose une interface web facile à prendre en main pour visualiser les alertes, les rapports et les tableaux de bord, facilitant ainsi la compréhension de l'état de la sécurité. Wazuh dispose d'un côté serveur et un côté agent à déployer sur l'ensemble des machines du GOAD, une fois déployer on peut observer en temps réel l'activité de l'ensemble de nos machines :

<img src="Photo_SAECLOUDCYBER\wazuh.png">

Le déploiement de notre serveur et de nos agents pourra se faire de manière automatique grâce à un script ansible qu'on a fait :

<img src="Photo_SAECLOUDCYBER\wazuh_ansible.png">

On a ensuite configurer notre serveur et nos agents pour remonter les logs de sécurité sysmon :

<img src="Photo_SAECLOUDCYBER\sysmon.png">

Et enfin Wazuh nous permettra de voir comment réagisse nos machines en cas d'attaques et de visualiser un rapport rapide des attaques faites :

<img src="Photo_SAECLOUDCYBER\attaques.png">

⭐ Vous pouvez retrouver notre compte rendu sur WAZUH ainsi qu'un fichier de log sur notre github au chemin suivant :

🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ WAZUH ➔ WAZUH-INSTALL.pdf

* **Fichier d'alerts log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ WAZUH ➔ alerts.log

* ### <u><b> Proxmox </b></u>
  _**Installation complète détaillée sur mon compte rendu [(ici)](https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD/blob/d8ef1963aacfec93a09d3f9db8e79549b897fa62/(Proxmox)Installation_Elastic_&_Wazuh/Installation-Wazuh(Proxmox)-SAE-Balazuc-Matheo.pdf).**_

  Installation de Wazuh et interface Web : 
![Alt text](Photo_SAECLOUDCYBER/wazuh(proxmox).png)
![Alt text](Photo_SAECLOUDCYBER/wazuh-web(proxmox).png)

    Ajout des agents GOAD : 
    ![Alt text](Photo_SAECLOUDCYBER/agents-wazuh(proxmox).png)


## <b><u>VI/ BloodHound</u></b>

BloodHound est principalement utilisé pour l'analyse de la sécurité des infrastructures AD dans les environnements Windows. Il permet de visualiser et d'analyser les relations entre les différents objets AD, comme les utilisateurs, les ordinateurs ... Il va collecter des donnéesà partir de l'AD pour construire des graphiques représentant les relations entre les objets AD. On peut par conséquent visualiser rapidement les vulnérabilités potentielles. Notre groupe a notamment pu visualiser le graphique de tout les domaines admins :

<img src="Photo_SAECLOUDCYBER\Bloodhound.png">

⭐ Vous pouvez retrouver notre compte rendu sur BloodHound sur notre github au chemin suivant :

🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

**ATTENTION** le compte rendu a été effectué sur un ordinateur portable de l'entreprise d'un membre du groupe par conséquent le nom en bas du fichier pdf et obsolète !

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ BloodHound.pdf 

## <b><u>VII/ OpenWEC</u></b>

OpenWEC est une implémentation gratuite et opensource d'un serveur Windows Event Collector. Il va permettre de collecter les journaux d'événements Windows à partir d'une machine Linux sans avoir besoin d'un agent local tiers exécuté sur les machines Windows. Il va être composé de choses :

* **openwecd** qui est le serveur OpenWEC.
* **openwec** qui est utilisé pour gérer le serveur OpenWEC.

Dans notre groupe c'est Yohann qui s'est chargé de la configuration de OpenWEC. Il y a passé environ 24 heures suite à de nombreux problèmes rencontrés. Le principal problème de OpenWEC et son fonctionnement avec Kerberos qui peut parfois nous causer quelques soucis, de plus le DC01 à eu un problème de "computers" qui ne remontait pas, par conséquent il a fallu faire la configuration premièrement sur le DC03. Au bilan OpenWEC a réussit à fonctionner et nous a permis de détecter les machines du DC03 et de pouvoir accèder à leurs logs :

<img src="Photo_SAECLOUDCYBER\OpenWECstats.png">

Exemple de fichier de log de la machine "MEEREN" :

<img src="Photo_SAECLOUDCYBER\LogMEEREN.png">

⭐ Vous pouvez retrouver notre compte rendu sur OpenWEC ainsi qu'un fichier de log sur notre github au chemin suivant :

🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ OPENWEC ➔ SAE_OPENWEC_INSTALL.pdf

* **Fichier de log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ OPENWEC ➔ openwec_logs

## <b><u>VIII/ Splunk</u></b>

Au cours de cette SAE on a pu découvrir Splunk, c'est une plateforme qui va permettre d'obtenir des informations sur d'innnombrables sources de données. Dans notre cas Splunk va avoir un peu la même utilisateur que Wazuh et Elastic. <br/>
Une des particularité de Splunk c'est qu'il faut obligatoirement créer un compte pour pouvoir télécharger le paquet pour installer le serveur et le forwarder côté client. L'installation reste simple, elle est accompagnée d'un assistant pour facilité cette dernière. Une fois installer, le serveur est accessible sur le port 8000. Sur le serveur pour pouvoir écouter les clients, on va venir créer un receveur sur le port 9997 (par défaut). Maintenant côté client il s'agit d'un exécutable où il va falloir préciser l'adresse IP du serveur ainsi que les ports par défaut notamment celui du receveur 9997. Le forwarder ne suffit pas à recevoir les informations des clients windows, il faut également télécharger un addon et modifier un fichier de configuration pour enfin pour observer l'apparition de nos agents. En voici un exemple :

<img src="Photo_SAECLOUDCYBER\clientsplunk.png"> 

L'installation et la configuration est très simple mais demande un petit temps si elle n'est pas faite de manière automatique à l'aide de ansible. Dans notre cas on a également rajouter la réception des logs sysmon sur le serveur avec l'ajout du port UDP 514 en écoute et côté client l'ajout de deux petits paramètres sur un fichier de conf. On observera bien par la suite la remontée de log sysmon :

<img src="Photo_SAECLOUDCYBER\sysmon_log.png"> 

⭐ Vous pouvez retrouver notre compte rendu sur Splunk ainsi qu'un fichier de log et un fichier .evtx sur notre github au chemin suivant :

🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ SAE_INSTALL_SPLUNK.pdf

* **Fichier de log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ SPLUNK_LOGS-2023-12-07.pdf

* **Fichier evtx** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ SPLUNK ➔ sysmon_log.evtx

## <b><u>IX/ Auditd - Chainsaw - Hayabusa</u></b>

1. <u><b>Auditd</u></b>

Auditd permet d'implémenter ce qu'on appel des "hooks" ou appels système, qui permettent de surveiller les processus en mode utilisateur et générer des événements d'audit dans le cas où ils correspondent à la politique de sécurité définie sur le système. Dans notre cas pour la SAE l'utilisation de auditd sur Linux à seulement permis de faire la surveillance du fichier "/etc/passwd" en ajoutant une régle dans le fichier de rules de auditd, vous pouvez retrouver une image de la configuration du fichier ci-dessous :

<img src="Photo_SAECLOUDCYBER\auditd.png"> 

Pour les logs, auditd remonte les logs dans le fichier "/var/log/audit/audit.log".

2. <u><b>Audit - Windows</u></b>

Pour ce qui est de la partie audit sur les clients Windows, on a pu réaliser la configuration de ce dernier pour qu'il surveille les fichiers de log de notre choix notamment le fichier de log de Splunk. L'application **Event Viewer** nous permettra de voir la remonter des logs :

<img src="Photo_SAECLOUDCYBER\log_audit.png"> 

3. <u><b>Chainsaw</u></b>

Chainsaw permet de visualiser et analyser les fichiers de logs qu'on génére au format .evtx, il va examnier et comprendre les journaux qu'on a généré. Dans notre cas on va réutiliser le fichier .evtx téléchargé après l'installation de Splunk pour réaliser simplement une chasse global et avoir une analyse global de nos logs :

<img src="Photo_SAECLOUDCYBER\chainsaw.png"> 

Voici l'exemple d'une détection réalisé par chainsaw où il nous remonte qu'il a détecté un changement de configuration de sysmon. On a pas pu l'utiliser plus en profondeur par manque de temps mais également par manque de fichier exploitable au format .evtx.

4. <u><b>Hayabusa</u></b>

Hayabusa a un peu la même utilité que Chainsaw il va nous permettre de faire de l'analyse de log, toujours au même format mais sous d'autres manières. Il va permettre notamment de faire l'analyse des événements par leurs ID :

<img src="Photo_SAECLOUDCYBER\hayabusa.png"> 

Comme pour chainsaw on a pas pu l'utiliser plus en profondeur par manque de temps et par manque de fichier.

⭐ Vous pouvez retrouver notre compte rendu sur l'ensemble d'audit et de chainsaw/hayabusa ainsi qu'un fichier de log et les fichiers de chasse sur notre github au chemin suivant :

🐱 Lien vers notre github : https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD

* **Compte Rendu** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ AUDITD-CHAINSAW ➔ AUDITD-CHAINSAW.pdf

* **Fichier de log** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ AUDITD-CHAINSAW ➔ audit.log

* **Fichier Chasse Chainsaw** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ AUDITD-CHAINSAW ➔ chainsaw_hunt_global.out

* **Fichier Hayabusa** : SAE5.CYBERCLOUD ➔ Installation_SIEM ➔ AUDITD-CHAINSAW ➔ hayabusa_sysmon.out

## <b><u>X/ Attaques</u></b>

* ### <u><b> Proxmox Alertes </b></u>
  
Pour le déclenchement d'alertes, nous avons utilisé APT Simulator à partir des agents GOAD.

![Alt text](Photo_SAECLOUDCYBER/apt-simulator(proxmox).png)

Suite à ce déclenchement nous pouvons étudier les alertes depuis Elastic et Wazuh.

_**Pour en savoir plus sur la procédure de déclanchement d'alertes ou de résolution d'erreurs, voir le compte rendu Elastic [(ici)](https://github.com/YohannAnsinelli/SAE5.CYBERCLOUD/blob/50967c96a3e9a301410d69b4aa8e74497344c21e/(Proxmox)Installation_Elastic_&_Wazuh/Installation-Elastic(Proxmox)-SAE-Balazuc-Math%C3%A9o.pdf).**_

## <b><u>XI/ Schéma Réseau</u></b>

Pour cette SAE, il fallait réaliser la conception d'un schéma réseau, hors au vu de l'organisation de la SAE qui met en parallèle VirtualBox et Proxmox, il paraissait compliqué de réunir tout sur un même schéma, notre groupe a par conséquent de réaliser un schéma pour VirtualBox et un schéma pour Proxmox :

* ### <u><b> VirtualBox </b></u>

<img src="Schéma Réseau.png"> 

* ### <u><b> Proxmox </b></u>

## <b><u>XIII/ Conclusion</u></b>

En Conclusion, notre équipe aura pu mettre en place de nombreux outils permettant la surveillance et la défense de notre réseau, tous avec des avantages, ce qui nous a permis de les comparer et de voir lesquelles répondent le mieux à notre besoin, mais également cela nous a permis d'en découvrir de nouveau et par conséquent agrandir notre bibliothèque personnelle. Pour plus de diversité, le déploiement a été effectué sur plusieurs logiciels/plateformes de virtualisation permettant d'étendre nos capacités d'adaptations en entreprise. Le principe de "blue team" et "red team" a permis notamment de voir comment nos machines réagissent fasse à une menace et quelles sont les mesures à prendre pour protéger le réseau de cette dernière. Cette SAE aura aussi permis de renforcer le travail d'équipe, dans notre cas cela regrouper deux personnes d'une même filière avec une personne d'une filière différente permettant d'apporter un point de vue différent. Dans notre groupe, on a d'abord regardé là où chacun était le plus à l'aise pour ensuite se repartir les tâches et faire des petites réunion sur l'avancement du projet tous les jours, mais également l'utilisation en parallèle une application de gestion de tâche pour permettre de noter l'avancement des tâches de chacun. Comparais au dernier passage de Mr Pouchoulon pour vérifier ce qui a été fait au sein du groupe, il y a eu l'ajout de tous les agents sur Splunk, mais également le déploiement d'audit sur les postes Windows. 





