% Automatisation dans un S.I
% Marc Cenon
% Stage du 12/04/2021 au 31/12/2021

# Remerciements
Je tiens à remercier en premier lieu toute l'équipe Infra de CGI pour son accueil chaleureux, tout particulièrement **Mr. Thomas Colenos** et **Mr. Arthur Bertinetti** pour leur patience et leur grande pédagogie. 
J'ai énormément appris. Ils m'ont fait confiance pour travaillers avec eux sur divers projets et avec une grande autonomie.

Je remercie également le corps enseignents de l'université de Bordeaux pour leurs cours de qualités. 

# Introduction

Dans le cadre de la Licence professionnelle ADSILLH, j'ai éffectué un stage de 6 mois au sein de l'équipe infrastructure chez CGI. Dans ce rapport, je vais vous presenter l'entreprise qui m'a accuelli et plus précisement l'équipe où j'ai réalisé mon stage. Vous trouvez dans les annexes un tableau qui récapitules les taches sur lequelles j'ai travaillé, semaine par semaine.

Etant donné la diversité des taches réalisées, j'ai choisi comme thème de rapport de stage l'automatisation dans un S.I avec un focus sur le déploiment d'une stack de monitoring ainsi que le déploiement d'un service complexe (centre de formation) que je presenterai rapidement.

Ayant signé un accord de non divulgation, aucunes données confidentielles ne seront présentées dans ce rapport.

\pagebreak

# Partie 1:
## L'entreprise CGI
CGI est le leader mondial du conseil et des services numériques. Avec plus de 40 ans d'expertise et de savoir faire et présent dans plus de 40 pays, le groupe CGI est implenté dans 21 villes en france avec environs 11 000 salariés. Quelques points clés:
+ fondée en 1976 par Serge Godin dans la ville de Québec au Canada
+ Le sigle **« CGI »** signifie **« Conseillers en gestion et informatique »**. En anglais, l’appellation reconnue est **« Consultants to Government and Industry »**.
+ de nombreux secteurs d'activités (Assurance, Banque, Luxe, Enerrgie, Industrie, Santé, Secteur Public, Logistique, Transport, Télécommunication, ...)
+ plus de 7,5 MD€ en 2020
- quelques chiffres sur CGI
- organigrame france
- presentation de la BU

## La BU
- l'organigrame
- la repartition des taches
  - l'organisation du travail
  - types de clients
    - présentation du secteur education nationale

## L'équipe Infra
- a qui je rend des comptes et qui me suit au jours le jours
- environnement technique de travail

## Mes missions
- j'ai été recruté pour rejoindre l'équipe qui travaille dans le secteur de l'éducation nationale et particulièrement sur l'ENT: Espace Numérique de Travail, qui est utilisé par plusieurs regions de France. Cet ENT, très complet fournis des solutions clées en mains au collégiens et lycéens mais également aux professeurs et parents d'élève. Dans le contexte sanitaire actuel, l'équipe à du s'adapter très rapidement pour fournir une solution performante et robuste afin de pouvoir supporter le fort développent du télé-enseignement. En annexe, vous trouverez un tableau qui reprend les principaux outils que l'ENT propose.

Je suis arrivé en Avril 2021 afin de pouvoir accompagner l'équipe en place dans leur travail au quotidient. Pour la liste des taches que j'ai effectués semaine par semaine, veuillez vous reporter au tableau en annexe. Je vais vous présenter les principales missions ci dessous:
1. Rapport d'alarmes quotidien
... Tout les jours je redigé un rappport sur les alertes de la veille. Ce rapport utilise la solution de monitoring CENTREON, avec des sondes et des parametres specifiques a la surveillance de l'infrastructure.
2. Création de scripts d'automatisation avec Ansible.
... La majeure partie de mon travaile consisté à automatisé des taches qui aurait été très chronophage. Mon tuteur **Mr Thomas Colenos** à une excellente maitrise de cet outils et il m'a permis d'apprendre en réalisant plusieurs script Ansible, particulièrement le déploiment d'une stack de monitoring que je présenterai dans la partie 2 de ce rapport.
3. support de l'équipe sur diverses taches.
... J'ai eu la chance d'avoir un stage avec des missions très variés. Ce qui a été très formatteur.


# Partie 2:

## Ansible: la magie de l'automatisation

### Pourquoi le besoin d'automatisation ?
L'automatisation consiste à utiliser des logiciels pour créer des instructions reproductibles dans le but de remplacer ou de réduire l'intervenion humaine. C'est un gain de temps et surtout cela permet de garantir le même résulat pour une opération réalisé n fois avec les mêmes paramêtres: c'est le principe d'idempotence
On passe plus de temps à écrire des règles d'automatisation mais une fois ces dernières testées et approuvées, on peut s'assurer du résultat et enlever les erreurs humaines (ex; faute de frappe,...)

L'automatisation est un élément clé de l'optimisation de l'environnement informatique dans un monde qui evolue rapidement, l'automatisation joue un rôle essentiel.

### Présentation d'Ansible
Ansible est un outil libre qui sert à automatiser la gestion de la configuration, du déploiement et de l’orchestration. Ses points forts:
- pas d'agents à déployer sur les machines
- permet de déployer des configurations normalisées: la même configuration sur un grand nombre de machine
- permet de déployer des configurations plus spécifiques: on peut cibler une machine ou un groupe de machines
- utilisation de SSH pour communiquer les taches d'executions sur les machines cibles (pas besoins d'ouvrir de ports spécifiques)
- utilisation de YAML comme language
– Grande communautée. 
...Lancé en 2013 et acquis par Red Hat en 2015. Avec plus d’un quart de millions de téléchargements, il est actuellement l’outil d’automatisation de logiciel libre le plus populaire sur GitHub. 
- ansible galaxy: collection de playbook pour un grand nombre de taches. Plus besoin de faire de script bash... Pour des taches comme installer un serveur APACHE, des roles sont disponibles où seul un parametrage des variables du playbook permet d'obtenir un résultat reproductible, previsible et fiable.

Ansible permet d'automatiser la configuration à plusieurs différents niveaux (systèmes d’exploitation, composantes d’application), et peut être appliqué à différents équipements (serveur, stockage, réseau) ou infrastructures (Bare-metal, VM , cloud). 

Ansible s'inscrit dans la mouvence IaC: Infrastructure as Code, c'est à dire gérer la configuration d'une infrastrucre à l'aide de fichiers de configuration stockable, versionable dans un flow CI/CD

Avec le développement des infrastrucre Cloud, Ansible, couplé à des outils comme Terraform et Packer, permet de gérer un infrastructure Cloud en mode IaC.

Personnelement, je ne vois que des avantages dans ce mode de gestion IaC. C'est ce que j'utilise pour gérer mon homelab (voir annexe)

## La solution de monitoring
Une de mes missions à été de mettre en place une solution de monitoring déploiable par Ansible pour pouvoir surveiller l'infrastrucre d'un client. La solution de monitoring retenue à été la suivante:
- Grafana: pour la centralisation des graphiques
- Influxdb comme base de données pour les différents metriques.
- Telegraf pour la collectes des metriques
- Loki pour le gestion des logs
- Promtail pour la recupération des logs

### Présentations des différentes applications qui constitue la stack de monitoring
Cette solution, plus connus sous le nom de TIG (Telegraf - Influxdb -  Grafana) et de PLG (Promtail - Loki - Grafana) pour les logs, est une solution efficace, robuste, scalable facilement et extrèmement customisable.
Nous somme sur une architecture logiciel sur 3 niveaux
- la collectes des metriques et des logs
- le stockage des metriques dans la bdd Influxdb
- l'affichage des graphique dans grafana

#### Télégraf
Telegraf est un agent de récupération de métriques, 1 seul agent est nécessaire par machine. Cet agent sait récupérer des métriques exposées et propose 2 modes de récupération des métriques, via :

- push : la métrique est poussée dans Telegraf par le composant qui l’expose
- pull : Telegraf récupère la métrique en interrogeant le composant qui l’expose (le mode le plus utilisé)

Les metriques sont par la suite insérées dans la bdd Influxdb

#### Influxdb
InfluxDB est une Time Series Database (TSDB) écrite en Go. Ces principaux avantages sont les performances, la durée de rétention importante et la scalabilité

#### Loki
Loki est un aggrégateur de logs, facilement scalable et inspiré de Prometheus, un autre outils de monitoring qui peut remplacer Influxdb  dans la stack Il utilise un mécanisme de découverte de service et ajoute des labels aux logs au lieu de de les indexer, ce qui rend facile leur manipulatiopn et ordonne leur stockage.
les journaux reçus de Promtail se composent du même ensemble de labels que celui des métriques d'application. Ce qui permet une meilleur intégration des logs et des metriques
De plus, Loki à besoin de peu de ressources pour fonctionner

#### Promtail
Promtail est un agent qui expédie les logs vers une instance Loki. Il est déployé sur chaque machine sur laquelle des applications doivent être surveillées.Il fonctionne en 3 temps:

- Découvre des cibles
- Attache des tags aux logs
- pousse les logs vers Loki.

Promtail est très customisable. Nous verons plus loin un exemple de configuration

#### Grafana
Grafana est un outil supervision moderne. Il permet d'exposer sous formes de dashboards les métriques brutes ou agrégées provenant d’InfluxDB. L'une de ses grande force est qu'il permet de créer très facilement des seuils d’alertes et les actions associées. comme l'envoie de mail pour alerter l'administrateur du S.I
On accède à Grafana depuis un navigateur Internet, Ce qui est très utile quand on veut monitorer une infrastructure à distance. Plus besoin d'installer de logiciels complet....

### Mise en place des différents éléments.
Point Important: cette stack peut être très facilement être installé grace à Docker. Personnelement, j'utilise cette solution sous docker, le tout orchestré avec k8S pour monitorer mon homelab. (voir annexe pour plus d'info)
Le choix fait par CGI et d'éviter la conteneurisation pour les environnement de production. Nous sommes donc parti sur une installation en dur des différentes briques de cette stack, le tout déployé par ansible.
Etant donnée la nature sensisble des informations, j'illustrerai par des graphiques de mon homelab et présenterez dans ce rapport seulement quelques morceaux que je juge important pour la compréhension

Vous trouverez en annexes le playbook dans son intégralité.

#### composition de l'infrastructure d'implentation de la stack TIG
cette solution de monitoring va surveiller plusieurs éléments d'une infrastructure d'une vingtaine de machines qui comprend:
- serveurs d'applications
- serveurs web nginx
- plusieurs bdd (MariaDB, MongoDb)

Etant donnée la composition de l'infrastruture, Telegraf qui sera deployé sur chaque machine va pouvoir récupérer une grande variété de metriques tels que:
- statistique machines: Memoire, CPU, Uptime, Stockage, Disk I/O
- nginx: load, network I/O, traffic, differentes requetes, nombres de connexions,...
- dans un autres temps les bdd: erreurs, SQL commands/sec, Heatmap (queries/sec) cache,...

Et Promtail sera en charge de recuperer les logs suivants:
- logs système
- logs applicatifs (nginx principalement)

## Installation d'Ansible
Ansible est disponible pour un grand nombre de Distribution Linux. Il peut être installé par un gestionnaire de packet ou par PIP car Ansible s'appuis majoritairement sur le language Python.
Pour l'installer sur CentOs

```shell
sudo yum install epel-release   <- ajout du repo
sudo yum install ansible   <- installation du packet
```

On verifie la bonne installation d'Ansible et des dépendences:

```shell
marc@pi-master [06:42:03] [/]
-> % ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/marc/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Jun  2 2021, 10:49:15) [GCC 9.4.0]
```
Ansible a besoin que le port SSH soit ouvert. Verifions que c'est bien le cas et egalement pour facilité et sécuriser la communication SSH, il est recommandé d'activé l'authentification par clé plutôt que par mot de passe

```shell
sudo firewall-cmd --list-services

dhcpv6-client mdns samba-client ssh
```
ssh fait bien partie des services actif dans le firewall

```shell
ssh-keygen
ssh-copy-id 'machine_cliente'
```
L'authentification par clé est mise en place  l'environnement de base est configurer.


### Notions de base
Avant de présenter les playbook que j'ai réalisé, il est important de comprendre quelques elements d'Ansible.
On définie des rôles, qui contiennent des taches à executer à l'aide de différents modules, le tout regroupé dans un playbook, qui va réunir les différents roles. Comme précisé plus haut, tout est ecrit en YAML.
Il existe de nombreux modules qui permettent de réaliser toutes les actions immaginables.
Ansible utilise egalement des template, au format jinja2 afin de facilité la creation de fichiers de configurations et la gestion des variables.

Il est de bonne pratique de créé un dossier par projet. Ce dossier va contenir plusieurs elements. voici un example simple d'arborescence d'un projet, que j'ai adapté depuis la documentation officielle d'ansible:

```shell
production.yml                # fichier inventaire pour la production

group_vars/
   group1.yml             # variables assigné à un groupe définie dans l'inventaire. Ici le groupe1
   group2.yml
host_vars/
   hostname1.yml          # variables assigné à une machine specifiquement
   hostname2.yml

library/                  # dossier pour des modules developé specifiquement

site.yml                  # Playbook nommé site.yml
webservers.yml            # playbook  nommé webservers.yml

roles/
    common/               # cette structure de fichier represente un role
        tasks/            #
            main.yml      #  <--fichier qui va contenir toutes les taches à effectuer
        handlers/         #
            main.yml      #  <-- fichier qui va contenir des taches inactives qui seront appliqué si elles sont appelé dans le fichier main.yml avec
        templates/        # 
            ntp.conf.j2   #  <------- c'est ici que les templates utilisé par le role sont stockées
        files/            #
            bar.txt       #  <-- On stocke les autres fichiers dans le dossier files
            foo.sh        #  <-- on va par exemple stocker des scripts ou des fichiers txt
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- Ce fichier indique les dépendances necessaires pour ce role
    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # "" un autre dossier qui contient le role monitoring
```

Il est important de respecter une structure et de s'y tenir car un projet peut contenir rapidement beaucoup de fichiers

## Le playbook pour le deploiment de la stack
### Organisation
Le playbook est organisé de la facon suivante:
- playbook.yml : nom du playbook qui contient tous les roles
- /roles: va contenir touts nos roles, templates, et handlers
- /inventory/hosts.yml: inventaire des machines 
- /inventory/group_vars/all.yml : variable globales
... le dossier group_vars contient egalement les dossiers avec les configuration spécifique de promtail pour chaque groupes de machines

la commande suivante permettra de deployer notre stack
```shell
ansible-playbook playbook.yml -i inventory/host.yaml
```

### Role Grafana

Les étapes du role d'installation de grafana sont simple. Avec l'aide des modules adéquats d'ansible, les étapes pour l'installation et la configuration de grafana sont les suivantes:

- creation du groupe et du commpte grafana:monitoring
- creation des dossier necessaires
- telechargement du programme et extration dans le dossier d'installation définie precedement
- creation d'un fichier de configuration grace à un template
- import d'un dashboard déjà créer précedament
- ouverture des ports dans le firewall
- creation du fichier .service al'aide d'un template
- activation du service et redemarrage

Pour ce rôle, l'utisation de template pour générer le fichier de configuration de grafana et le service associé permettent de simplifier le processus d'installation. Cela permet également de pouvoir modifier rapidement et facilement le rôle en ajustant les variable adéquate dans le fichier /inventory/group_vars/all.yml  Voici la tasks du role grafana qui utilise le template crée pour generer le fichier service:

```yaml
- name: "copy grafana systemd service from template"
  template:                                     <- le nom du module pour utiliser un template
    src: grafana.service.j2                     <- on selectionne le template en source
    dest: /etc/systemd/system/grafana.service   <- creation du service avec le template et les variable dans group_vars/all.yml
```

Un autre aventage d'ansible est l'utilisation de loop 'boucle' pour répéter une même action dans une tache avec des variables différentes. Voici un exemple pour l'ouverture des ports:
```yaml
- name: "open firewall port 3000 on the machine and port 25 for SMTP email"
  firewalld:                    <- le module pour intéragir sur le firewall
    state: "{{ item.state  }}"  <- on definie une variable pour l'état du port
    port: "{{ item.port  }}"    <- on definie une variable pour le numéro de port
    zone:
    immediate:
    permanent: yes
  with_items:                   <- cette option permet d'itérer les item defini plus haut
    - { state: 'enabled', port:'3000/tcp'  } <- on affecte des valeurs au variables item
    - { state: 'enabled', port:'25/tcp'  }
```

Avec ces quelques lignes, on ouvres les ports, dans la zone par defaut (car nous n'avons pas renseigné de zone specifique dans zone), de manière permanente et immédiate.

### Role Influxdb

Les étapes pour l'installation d'influxdb sont sensiblement identique à celle de grafana:

- creation du groupe et du compte influxdb:monitoring
- creations des dossiers necessaires
- telechargement du programme et extraction dans le bon dossier
- creation d'un fichier de configuration et du service à partir d'un template
- ouverture des ports dans le firewall
- activation du service
- pause de quelques seconde
- configuration d'influxdb en passant une command shell avec les paramêtre definie dans le fichier de variable

la difficulté ici et la dernieres etape. pour automatiser la configuration d'influxdb, on passe une commande shell avec les arguments necessaires pour la creation des elements necessaires à influxdb. 

```yaml
- name: 'check if folder exist'
  stat:
    path: "{{ influxdb_main_folder  }}/.influxdbv2"
  register: folder_exist                                <- on verifie que le dossier de configuration existe déjà et on enregistre le resultat
                                                           a l'aide d'un register
- name: 'configure influxdb as influxdb user and not root'
  become_user: "{{influxdb_account_name}}"
  shell: >
    {{ influxdb_main_folder  }}/influxdb/influx setup --org {{ influxdb_organization  }} --bucket {{ influxdb_bucket  }} --username {{ influxdb_username  }} --password {{ influxdb_password  }} --token {{ influxdb_token  }} --force
  when: not folder_exist.stat.exists                    <- cet condition permet de lancer la configuration d'infludb seulement si le dossier de configuration n'existe pas.
```

cette condition permet de s'assurer que le rôle se déroule bien car si on essaie de configurer la base de donnée alors que le dossier de configuration est déja présent, la task va échoué et le playbook ne sera pas déroulé dans son intégralité

Le point que je souhaitais mettre en avant ici est la facilité avec laquelle on peut definir des condition pour lancer, ou non des rôles.












## Déploiment d'une stack complexe multi-services, multi-plateformes, multi-fournisseurs


# Conclusion
- les points que j'ai réussi, les points que je n'ai pas reussi.
- parler de l'autonomie en temps de COVID et du télétravail
- parler du CDI ?? fingers crossed
- parler des certifs que j'ai passé cet été et de ce qu'elles peuvent apporter ...

# Annexes
- les tableaux
- grafs
- visuels des applis
- tableau du travail semaine par semaine

