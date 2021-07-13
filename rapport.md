# Automatisation dans un S.I
Marc Cenon

Stage du 12/04/2021 au 31/12/2021

web: https://marc-cenon.github.io/my_resume/

mail: marc.cenon33@gmail.com

## Table des matieres

- [Remerciements](#remerciements)
- [Introduction](#introduction)
- [Partie 1:](#partie-1)
  * [L'entreprise CGI](#l'entreprise-cgi)
  * [La Bussiness Unit](#la-bussiness-unit)
  * [L'équipe Infra](#l-équipe-infra)
  * [Mes missions](#mes-missions)
- [Partie 2:](#partie-2)
  * [Ansible: la magie de l'automatisation](#ansible--la-magie-de-l-automatisation)
    + [Pourquoi le besoin d'automatisation ?](#pourquoi-le-besoin-d-automatisation--)
    + [Présentation d'Ansible](#pr-sentation-d-ansible)
  * [La solution de monitoring](#la-solution-de-monitoring)
    + [Présentations des différentes applications qui constitue la stack de monitoring](#pr-sentations-des-diff-rentes-applications-qui-constitue-la-stack-de-monitoring)
      - [Télégraf](#t-l-graf)
      - [Influxdb](#influxdb)
      - [Loki](#loki)
      - [Promtail](#promtail)
      - [Grafana](#grafana)
    + [Mise en place des différents éléments.](#mise-en-place-des-diff-rents--l-ments)
      - [composition de l'infrastructure d'implentation de la stack TIG](#composition-de-l-infrastructure-d-implentation-de-la-stack-tig)
  * [Installation d'Ansible](#installation-d-ansible)
    + [Notions de base](#notions-de-base)
  * [Le playbook pour le deploiment de la stack](#le-playbook-pour-le-deploiment-de-la-stack)
    + [Organisation](#organisation)
    + [Role Grafana](#role-grafana)
    + [Role Influxdb](#role-influxdb)
    + [Telegraf](#telegraf)
    + [Promtail](#promtail-1)
    + [Loki](#loki-1)
    + [le playbook](#le-playbook)
    + [le fichier host.yml](#le-fichier-hostyml)
    + [InflxQL : syntaxe SQL propre à Influxd](#inflxql---syntaxe-sql-propre---influxd)
    + [Exemple de configuration de l'agent Promtail pour récuperer des logs.](#exemple-de-configuration-de-l-agent-promtail-pour-r-cuperer-des-logs)
    + [Ajout des datastore dans Grafana](#ajout-des-datastore-dans-grafana)
    + [Importation du dashboard](#importation-du-dashboard)
    + [Interpréter le monitoring](#interpr-ter-le-monitoring)
    + [conclusion](#conclusion)
  * [Déploiment d'une stack complexe multi-services, multi-plateformes, multi-fournisseurs](#d-ploiment-d-une-stack-complexe-multi-services--multi-plateformes--multi-fournisseurs)
- [Conclusion](#conclusion)
- [Annexes:](#annexes-)


## Remerciements

Je tiens à remercier en premier lieu toute l'équipe Infra de CGI pour son accueil chaleureux, tout particulièrement **Mr. Thomas Colenos**, **Mr. Arthur Bertinetti** et **Mr Laurent Poutou*** pour leur patience et leur grande pédagogie. J'ai pu ainsi benéficier de leur grande expérience, ce qui m'a permis d'avoir une bonne monté en compétence.

En effet, chacune des personnes de l'équipe a su me consacre du temps et partager avec moi leur expertise, méthodes et connaissances tout au long de ce stage. Il m'ont permis de rendre cette experience de 6 mois enrichissante et pleine d'interet

J'ai énormément appris. Ils m'ont fait confiance pour travailler avec eux sur divers projets et avec une grande autonomie.

Je les remercie egalmeent pour la bonne humeur et les temps de rigolade qu'ils ont su me communiquer et l'envie qu'ils m'ont donné de travailler au sein de leur équipe.

Je tiens à remercier également le corps enseignant de l'université, notament **Mr Samuel Thibault** et **Mr Olivier Delmas** pour leurs soutient et leurs enseignemants.


## Introduction

Dans le cadre de la Licence professionnelle ADSILLH, j'ai éffectué un stage de 6 mois au sein de l'équipe infrastructure chez CGI. Dans ce rapport, je vais vous presenter l'entreprise qui m'a accuelli et plus précisement l'équipe où j'ai réalisé mon stage. Vous trouvez dans les annexes un tableau qui récapitules les taches sur lequelles j'ai travaillé, semaine par semaine.

Etant donné la diversité des taches réalisées, j'ai choisi comme thème de rapport de stage l'automatisation dans un S.I avec un focus sur le déploiment d'une stack de monitoring ainsi que le déploiement d'un service complexe (centre de formation) que je presenterai rapidement. et le deploiement du service jupyter dans un cluster kubernetes OVH

Aucunes données confidentielles ne sera présenté dans ce rapport.

Le but de ce stage etait d'intégrer l'équipe Infrastructure afin de participer au developement du numérique à l'école ainsi que sur le gestion de cette infrastructure. Ce stage m'a permis d'apprendre et de manipuler des technologies comme Ansible, Kubernetes, Python, mariadb,...

Au delà du gain en compétences techniques, l'immersion au sein d'un processus de gestion de projet m'a appris à reconnaitre et interagir avec chacune des phases du projet sur le terrain. Cette immersion au sein d'un environnement complexe m'a également appris à etre plus efficace, que ce soit par le biais d'une meilleure gestion de mon temps ou encore une meilleure communication sur l'avancement des mes taches auprès de l'équipe que j'ai intégré.


## Partie 1
### L'entreprise CGI

Fondé en juin 1976 par Serge Godin à Québec, Canada, CGI est un groupe canadien actif dans le domaine des technologies de l’information et en gestion des processus d’affaires. Au cours des dix premières années d’existence, CGI a développé une stratégie, un modèle et un ensemble de principes de gestion qui se sont traduits par une croissance considérable. Devant les demandes des clients d’externaliser leurs systèmes informatiques, CGI s'est adapter et a élaborer une nouvelle stratégie pour se positionner sur le marché émergent de l’externalisation.

Durant la fin des années 80 et début 90, CGI commença à acquérir des sociétés proposant des services d’externalisation. Dès lors, CGI est en mesure d’offrir à ses clients des services informatiques complets tels que des services en TI (Technologies de l’Information) et en gestion, des services d’intégrations de système et d’externalisation.

Dans les 20 dernières années, CGI chercha à atteindre une taille critique sur les marchés géographiques de ses clients, d’acquérir une croissance approfondie de leurs secteurs d’activités ainsi que de développer des pratiques spécialisées et des solutions novatrices.
En 2010, CGI fait l’acquisition de Stanley Inc. et de ses filiales Oberon et Techrizon dans le but de doubler la taille de ses activités aux États-Unis. Deux années plus tard, CGI réalisa sa plus grosse acquisition en fusionnant avec l’entreprise Logica faisant passer son nombre de collaborateurs de 31 000 à 68000.

Au cours de son histoire, CGI a réussi une expansion exponentielle et continue pendant 35 ans grace à une stratégie de rachat et de conquete des différents marchés comme en témoigne le tableau ci-joint en Annexe  relatant sa forte croissance en chiffre d’affaires, en nombre de bureaux et en nombre d’employés.

CGI est l'un des leader mondial du conseil et des services numériques. Avec plus de 40 ans d'expertise et de savoir faire et présent dans plus de 40 pays, le groupe CGI est implenté dans 21 villes en france avec environs 11 000 salariés.



L’entreprise est actuellement dirigée par trois personnes :
- Serge Godin : Fondateur et président exécutif du conseil,
- André Imbeau : Fondateur et membre du conseil d’administration,
- George D. Schindler : Président et chef de la direction.

Avec une présence dans 40 pays, une solide expertise dans tous ses marchés cibles et un éventail complet de service en IT, la priorité de CGI reste de satisfaire ses clients. Grace à une approche cohérente, disciplinée et responsable en matière de prestation de services, CGI affiche un bilan inégalé de 95% de projets réalisés dans le respect des échéances prévues et affiche un indice de satisfaction des clients qui est constamment supérieur à 9 sur 10. Ce score de satisfaction couplé à la croissance continue de CGI témoigne de la confiance que ses clients accordent à CGI et du dévouement de ses collaborateurs. 

Ceci dans le but de devenir un fournisseur de services complets, d’atteindre des résultats grace à des ressources mondiales, à une connaissance approfondie de l’industrie, à une stabilité et des professionnels motivés. CGI possède maintenant 6 domaines d’expertises métiers qui sont le Business Consulting, l’intégration de systèmes, l’Outsourcing IT, les Services d’infrastructures, l’Application management et les Business procès services. Ces 6 domaines d’expertises sont répartis dans pas moins de 9 secteurs d’activités.


CGI est la cinquième plus importante entreprise indépendante en services IT et en gestion des processus d'affaires au monde au service avec plus de 10 000 clients dans le monde dont 500 en France.

Le groupe est composé de 70 000 membres répartis sur 400 bureaux répartis dans 40 pays dont 22 en France et réalise 7,6 milliards € de revenus mondiaux dont 1 milliard en France, au travers de projets intégration de système, d'outsourcing IT et également plus de 100 solutions exclusives soutenant les activités critiques de nos clients.

L’implantation de CGI en France résulte de la fusion de CGI avec Logica en 2012. Au niveau national, la filiale française de CGI est dirigée par Jean-Michel Baticle, entré dans le groupe en 1969. Son implantation dans la plupart des grandes villes françaises lui procure une implantation homogène pour couvrir l’ensemble du territoire métropolitain.

La structure de direction de CGI France est centrée autour des clients et chacune de ses activités sont regroupées au sein de Business Units qui sont au coeur même du modèle de CGI



### La Bussiness Unit 

En France, CGI est organisé en B.U : businness unit. J'ai réaliser mon stage dans la BU TPSHR (transport, secteur public, ressources humaine), plus précisement dans le group Local Gov, au service des collectivités locales. Local Gov à pour but de proposer aux collectivités territoriales des solutions de services visant à faciliter le quotidien du citoyen, rendre les acces plus directs aux services et permettre un plus grand benefice de la dematerialisation.

Vous trouverez en annexe un organigrame de la BU.

### L'équipe Infra

Mon maitre de stage **Mr Thomas Coleno** ainsi que *Mr Laurent Poutou** et *Mr Arthur Bertinetti** m'ont accueilli dans leur équipe. Le contexte sanitaire a fait que 99% de mon temps de travail été à distance. A partir du mois de Juillet, nous avons pu nous reunir une fois par semaine dans les locaux de CGI au Haillan.

CGI m''a également fourni un ordinateur portable afin de pouvoir télétravailler dans de bonne condition.


### Mes missions

J'ai été recruté pour rejoindre l'équipe qui travaille dans le secteur de l'éducation nationale et particulièrement sur l'ENT: Espace Numérique de Travail, qui est utilisé par plusieurs regions de France. Cet ENT, très complet fournis des solutions clées en mains au collégiens et lycéens mais également aux professeurs et parents d'élève. Dans le contexte sanitaire actuel, l'équipe à du s'adapter très rapidement pour fournir une solution performante et robuste afin de pouvoir supporter le fort développent du télé-enseignement. En annexe, vous trouverez un tableau qui reprend les principaux outils que l'ENT propose.

Je suis donc arrivé en Avril 2021 afin de pouvoir accompagner l'équipe en place dans leur travail au quotidient. Pour la liste des taches que j'ai effectués semaine par semaine, veuillez vous reporter au tableau en annexe. Je vais vous présenter les principales missions ci dessous:

- 1. Rapport d'alarmes quotidien
... Tout les jours je redigé un rappport sur les alertes de la veille. Ce rapport utilise la solution de monitoring CENTREON, avec des sondes et des parametres specifiques a la surveillance de l'infrastructure.

- 2. Création de scripts d'automatisation avec Ansible.
... La majeure partie de mon travaile consisté à automatisé des taches qui aurait été très chronophage. Mon tuteur **Mr Thomas Colenos** à une excellente maitrise de cet outils et il m'a permis d'apprendre en réalisant plusieurs script Ansible, particulièrement le déploiment d'une stack de monitoring que je présenterai dans la partie 2 de ce rapport.

- 3. support de l'équipe sur diverses taches.
... J'ai eu la chance d'avoir un stage avec des missions très variés. Ce qui a été très formatteur.

## Partie 2:

### Ansible: la magie de l'automatisation

#### Pourquoi le besoin d'automatisation ?

L'automatisation consiste à utiliser des logiciels pour créer des instructions reproductibles dans le but de remplacer ou de réduire l'intervenion humaine. C'est un gain de temps et surtout cela permet de garantir le même résulat pour une opération réalisé n fois avec les mêmes paramêtres: c'est le principe d'idempotence

On passe du temps à écrire des règles d'automatisation mais une fois ces dernières testées et approuvées, on peut s'assurer du résultat et enlever les erreurs humaines (ex; faute de frappe,...)

L'automatisation est un élément clé de l'optimisation de l'environnement informatique dans un monde qui evolue rapidement, l'automatisation joue un rôle essentiel.

#### Présentation d'Ansible

Ansible est un outil libre qui sert à automatiser la gestion de la configuration, du déploiement et de l’orchestration. Ses points forts:
- pas d'agents à déployer sur les machines
- permet de déployer des configurations normalisées: la même configuration sur un grand nombre de machine
- permet de déployer des configurations plus spécifiques: on peut cibler une machine ou un groupe de machines
- utilisation de SSH pour communiquer les taches d'executions sur les machines cibles (pas besoins d'ouvrir de ports spécifiques)
- utilisation de YAML comme language
– Grande communautée. 

   Lancé en 2013 et acquis par Red Hat en 2015. Avec plus d’un quart de millions de téléchargements, il est actuellement l’outil d’automatisation de logiciel libre le plus populaire sur GitHub. 

- ansible galaxy: collection de playbook pour un grand nombre de taches. Plus besoin de faire de script bash.

.. Pour des taches comme installer un serveur APACHE, des roles sont disponibles où seul un parametrage des variables du playbook permet d'obtenir un résultat reproductible, previsible et fiable.


Ansible permet d'automatiser la configuration à plusieurs différents niveaux (systèmes d’exploitation, composantes d’application), et peut être appliqué à différents équipements (serveur, stockage, réseau) ou infrastructures (Bare-metal, VM , cloud). 

Ansible s'inscrit dans la mouvence IaC: Infrastructure as Code, c'est à dire gérer la configuration d'une infrastrucre à l'aide de fichiers de configuration stockable, versionable dans un flow CI/CD

Avec le développement des infrastrucre Cloud, Ansible, couplé à des outils comme Terraform et Packer, permet de gérer un infrastructure Cloud en mode IaC.

Personnelement, je ne vois que des avantages dans ce mode de gestion IaC. C'est ce que j'utilise pour gérer mon homelab (voir annexe)
Le fait de pouvoir redeployer son infrastructure et sa configuration grace des des fichiers de configuration rend est un atout majeur en cas de problème technique. Une reinstallation d'un service peut etre realisé rapidement.


### La solution de monitoring

Une de mes missions à été de mettre en place une solution de monitoring déploiable par Ansible pour pouvoir surveiller l'infrastrucre d'un client. La solution de monitoring retenue à été la suivante:

- Grafana: pour la centralisation des graphiques
- Influxdb comme base de données pour les différents metriques.
- Telegraf pour la collectes des metriques
- Loki pour le gestion des logs
- Promtail pour la recupération des logs


#### Présentations des différentes applications qui constitue la stack de monitoring

Cette solution, plus connus sous le nom de TIG (Telegraf - Influxdb -  Grafana) et de PLG (Promtail - Loki - Grafana) pour les logs, est une solution efficace, robuste, scalable facilement et extrèmement customisable.
Nous somme sur une architecture logicielle sur 3 niveaux:

- la collectes des metriques et des logs
- le stockage des metriques dans la bdd Influxdb
- l'affichage des graphique dans grafana


##### Télégraf

Telegraf est un agent de récupération de métriques, 1 seul agent est nécessaire par machine. Cet agent sait récupérer des métriques exposées et propose 2 modes de récupération des métriques, via :

- push : la métrique est poussée dans Telegraf par le composant qui l’expose
- pull : Telegraf récupère la métrique en interrogeant le composant qui l’expose (le mode le plus utilisé)

Les metriques sont par la suite insérées dans la bdd Influxdb


##### Influxdb

InfluxDB est une Time Series Database (TSDB) écrite en Go. Ces principaux avantages sont les performances, la durée de rétention importante et la scalabilité


##### Loki

Loki est un aggrégateur de logs, facilement scalable et inspiré de Prometheus, un autre outils de monitoring qui peut remplacer Influxdb  dans la stack Il utilise un mécanisme de découverte de service et ajoute des labels aux logs au lieu de de les indexer, ce qui rend facile leur manipulatiopn et ordonne leur stockage.
les journaux reçus de Promtail se composent du même ensemble de labels que celui des métriques d'application. Ce qui permet une meilleur intégration des logs et des metriques

De plus, Loki à besoin de peu de ressources pour fonctionner


##### Promtail

Promtail est un agent qui expédie les logs vers une instance Loki. Il est déployé sur chaque machine sur laquelle des applications doivent être surveillées.Il fonctionne en 3 temps:

- Découvre des cibles
- Attache des tags aux logs
- pousse les logs vers Loki.

Promtail est très customisable. Nous verons plus loin un exemple de configuration

##### Grafana

Grafana est un outil supervision moderne. Il permet d'exposer sous formes de dashboards les métriques brutes ou agrégées provenant d’InfluxDB. L'une de ses grande force est qu'il permet de créer très facilement des seuils d’alertes et les actions associées. comme l'envoie de mail pour alerter l'administrateur du S.I
On accède à Grafana depuis un navigateur Internet, Ce qui est très utile quand on veut monitorer une infrastructure à distance. Plus besoin d'installer de logiciels complet....

#### Mise en place des différents éléments.

Point Important: cette stack peut être très facilement être installé grace à Docker. Personnelement, j'utilise cette solution sous docker, le tout orchestré avec k8S pour monitorer mon homelab. (voir annexe pour plus d'info)
Le choix fait par CGI et d'éviter la conteneurisation pour les environnement de production. Nous sommes donc parti sur une installation en dur des différentes briques de cette stack, le tout déployé par ansible.

Etant donnée la nature sensisble des informations, j'illustrerai par des graphiques de mon homelab et présenterez dans ce rapport seulement quelques morceaux que je juge important pour la compréhension

Vous trouverez en annexes le playbook dans son intégralité.

##### composition de l'infrastructure d'implentation de la stack TIG

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


### Installation d'Ansible

Ansible est disponible pour un grand nombre de Distribution Linux. Il peut être installé par un gestionnaire de packet ou par PIP car Ansible s'appuis majoritairement sur le language Python.

Pour l'installer sur CentOs:


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



#### Notions de base

Avant de présenter les playbook que j'ai réalisé, il est important de comprendre quelques elements d'Ansible.
On définie des rôles, qui contiennent des taches à executer à l'aide de différents modules, le tout regroupé dans un playbook, qui va réunir les différents roles. Comme précisé plus haut, tout est ecrit en YAML.
Il existe de nombreux modules qui permettent de réaliser toutes les actions immaginables.

Ansible utilise egalement des template, au format jinja2 afin de facilité la creation de fichiers de configurations et la gestion des variables.

Il est de bonne pratique de créé un dossier par projet. Ce dossier va contenir plusieurs elements. voici un example simple d'arborescence d'un projet, que j'ai adapté depuis la documentation officielle d'ansible:

```yaml
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


### Le playbook pour le deploiment de la stack
#### Organisation

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

il est egalement poissible de redeployer seuelemnt un role en precisant le tag du role dans la commande ci dessus. Ce qui donne par exemple:

```shell
ansible-playbook playbook.yml -i inventory/host.yaml --tags="NOM_DU_ROLE"
```


#### Role Grafana

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
    state: "t s’adapter et d{{ item.state  }}"  <- on definie une variable pour l'état du port
    port: "{{ item.port  }}"    <- on definie une variable pour le numéro de port
    zone:
    immediate:
    permanent: yes
  with_items:                   <- cette option permet d'itérer les item defini plus haut
    - { state: 'enabled', port:'3000/tcp'  } <- on affecte des valeurs au variables item
    - { state: 'enabled', port:'25/tcp'  }
```

Avec ces quelques lignes, on ouvres les ports, dans la zone par defaut (car nous n'avons pas renseigné de zone specifique dans zone), de manière permanente et immédiate.


#### Role Influxdb

Les étapes pour l'installation d'influxdb sont sensiblement identique à celle de grafana:

- creation du groupe et du compte influxdb:monitoring
- creations des dossiers necessaires
- telechargement du programme et extraction dans le bon dossier
- creation d'un fichier de configuration et du service à partir d'un template
- ouverture des ports dans le firewall
- activation du service
- pause de quelques seconde
- configuration d'influxdb en passant une command shell avec les paramêtre definie dans le fichier de variable
- activatio du service


la difficulté ici et la dernieres etape pour automatiser la configuration d'influxdb, on passe une commande shell, avec des arguments issus de variables defini dans group_vars/all.yml,  pour la creation des elements necessaires à influxdb. 


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


#### Telegraf

pour compléter notre stack TIG, il nous reste à deployer le role pour Telegraf. Il sera installer sur toutes les machines à monitorer. Les étapes du role sont les suivantes:

- creation du groupe de du compte telegraf:monitoring
- creation des dossiers necessaires
- telechargement et extration dans le bon dossier
- creation d'un fichier de configuration et dun service avec un template
- activation du service

Les êtapes sont sensiblement les mêmes que pour grafana et influxdb. Le point important ici est le fichier de configuration. Une partie de la configuration sera la même pour toutes les machines (%CPU, %MEM, uptime, %sdd,...).En fonction des specificité des machines, la configuration sera à affiné pour recupérer des metriques specifiques commes des metriques sur nginx, apache, mariadb,...

Pour cela  2 stratégie sont possible. 

- deployer la meme configration sur toute les machines et ajouter la configuration specifique manuellement ... ce qui ne parait pas logique quand on veut automatiser.

- creer des sous dossiers dans group_vars ou host_vars (si déploiment d'une config specifique à une machine) avec dedans un fichier avec les variables necessaires à la configuration specifique des machines.
 C'est le deuxieme choix qui semble le plus aventageux.

Quand il y a de la configuration specifique à un groupe de machine, il suffit de definir ces variable adéquates dans un fichier dans un dossier qui porte le nom du groupe de machine dans le dosser group_vars.

C'est également le choix qui sera retenue pour le deploiementy de la configuration de promtail.


#### Promtail

L'installation de Promtail suit le même schema que télégraf. Comme cet agent sera deployer sur toute les machines, il y aura un bout de configuration commune et un autre spécifique à un group de machine.


#### Loki

L'installation de Loki est identique à celle de Grafana et de Promtail


#### le playbook

Le playbook var regrouper les differents roles afin de les executer à la suite. Voici comment le role grafana est appelé dans le playbook


```yaml
- name: install grafana
  remote_user: "{{ user  }}"  <- variable qui sert à definir le compte utilisé pour executer le role
  become: true                <-  permet de passer root. Cela est necessaire pour pour copier le service dans le bon repertoire et pour l'activer
  hosts: monit                <- ici on défini la cible ou le groupe de machine sur laquelle le role sera executé. Grafana est seulement déployer sur le controleur
  tags: [grafana]             <- definir un tag nous permet si on le veux de choisir les roles a executer en utilisant les tags dans la commande d'ansible
  roles:
    - role: install_grafana   <- le nom du dossier qui contient le role grafana.
```

On repète le meme schema pour les autres roles.


#### le fichier host.yml

C'est l'un des fichier les plus important. C'est dans ce dernier que l'on va definir la la liste des machines que nous voulons intégrer à ce playbook. Il peut être au format **.ini** mais il peut être egalement ecrit au format **.yml** 

Voici un exemple de fichier hosts:


```yaml
all:
  children:
    monit:                              <- le groupe monitoring,
      hosts:
        monitoring-vm1:                  <- le nom de la machine
          ansible_host: 192.168.0.1     <- l'IP de la machine
    clients:                            <- le group client qui contient des sous groupes
      children:
        bdd:                            <- sous groupe bdd
          hosts:
            moodle-bdd-vm1:
              ansible_host: 192.168.0.2
            springboard-bdd-vm2:
              ansible_host: 192.168.0.3
        nginx:                          <- sous groupe nginx
          hosts:
            springboard-nginx01:
              ansible_host: 192.168.0.4
            springboard-nginx02:
              ansible_host: 192.168.0.5
        apache:                         <- sous groupe apache
          hosts:
            moodle-apache01:
              ansible_host: 192.168.0.6
```

On a beaucoup de flexibilité et de modularité dans le fichier host pour creer des groupes et des sous groupes. Cela nous permets de pouvoir deploiyer de la configration avec une très grande précision et de cibler une ou un groupe de machines.


#### InflxQL : syntaxe SQL propre à Influxd

Influxdb est une base de donnée temporelle, à la différence des bases de données relationnelles comme MySql ou Mariadb. Ce type de base de donnée idéal quand on doit manipuler des données temporelles comme la mesure de la température du CPU toute les 10 secondes. Du fait que ce type de bdd traite une très grande quantité d'informations, et dans un temps très courts, la gestion des données est differentes à celle d'un base de donnée relationnelle. 

Les bases de données temporelles dispose de reglès de retentions que l'administrateur decide afin de choisir la quantité d'information à stocker/recicler.

Depuis la version 2.0 D'influxdb, le language de requete InfluxQL à été remplacé par le language FLUX, qui est plus performant et customizable.

Flux est une alternative à InfluxQL et à d'autres langages de requete de type SQL pour interroger et analyser des données. Il utilise des modèles de langage fonctionnels, ce qui le rend capable de surmonter bon nombre des limitations d'InfluxQL. Sa syntaxe est en partie inspéré de Javascript.

Quelques notion importante pour pouvoir ecrire des requestes avec Flux:

- utilisation de "pipe forward" |> pour enchainer des actions
- toutes les données sont structuré sous forme de tableau.
- Un regroupement de tableaux avec une politique de rétention est un Bucket. 

Voici quelques exemples de requêtes en language FLUX:

Nombre de processess par machine:
```SQL
from(bucket: "bucket-vm")
  |> range(start: 2021-07-05T02:28:35Z, stop: 2021-07-05T08:28:35Z)
  |> filter(fn: (r) => r["_measurement"] == "processes")
  |> filter(fn: (r) => r["_field"] == "total")
  |> group(columns: ["host"])
  |> aggregateWindow(every: 20s, fn: mean, createEmpty: false)
  |> yield(name: "mean")
```


Utilsation du cpu par machine:
```SQL
from(bucket: "bucket-vm")
  |> range(start: 2021-07-05T02:29:36Z, stop: 2021-07-05T08:29:36Z)
  |> filter(fn: (r) => r["_measurement"] == "cpu")
  |> filter(fn: (r) => r["_field"] == "usage_system")
  |> filter(fn: (r) => r["cpu"] == "cpu-total")
  |> group(columns: ["host"])
  |> aggregateWindow(every: 20s, fn: mean, createEmpty: false)
  |> yield(name: "mean")
```

Influxdb dispose également d'une WEBUI qui permette de facilité grandement la creation de requete complique. Il suffit de choisir les critères dans le menu et d'importer la requete dans grafana, qui nous permettra de visualiser le resultat avec un graphique très customisable

L'ensemble des requêtes du playbook est également disponible de le fichier dashboard.json.
Flux est un language très puissant mais le WEBUI d'Influxdb permet d'arriver au même resultat rapidement et de gerer les buckets, et politiques de retention des données très facilement.


#### Exemple de configuration de l'agent Promtail pour récuperer des logs.

Afin de compléter notre stack de monitoring pour les logs, il faut configurer promtail pour lui dire quels logs recupérer. C'est ce que l'on appelle "Scrape Job"

Voici un exemple de configuration de promtail pour récupérer les logs nginx


```yaml
#scrape job for cron log
- job_name: cron
  static_configs:
  - targets:
      - localhost
    labels:
      job: cron
      __path__: /var/log/cron
```

Une template est utilisé pour configurer les scrape jobs en fonction des différents groupe de machine. La template est dans le dossier template du role promtail et les variables sont définié dans les sous- dossiers qui portent le nom de chaque groupe, dans le dossier group_vars.

#### Ajout des datastore dans Grafana

Une fois les agents Promtail et Télégraf configurer pour envoyer les données à influxdb et loki, il faut par la suite ajouter dans grafana les data sources, c'est à dire Influxdb et Loki
Cette action est realisé dans les options de grafana en lui indiquant le chemin d'acces pour Influxdb et loki. (voir images en annexe)


#### Importation du dashboard

Le playbook contient également un Dashboard que j'ai créé précédement et qui peut être réutilisé pour chaque nouveau déploiment. Il suffit de le charger dans le menu a gauche et nous avons les graphiques correspondant à chaque requetes d'influxdb

Pour les logs, pour le moment il n'y a pas de dashboard de crée. Il suiffit d'aller dans explorer puis de selectionner loki comme data source et nous trouver les logs que promtail à recuperer.

#### Interpréter le monitoring

Grafana permet de créer des alertes en fonction de critères choisis par l'administrateur. On peut par exemple definir l'envoi d'un mail lorsque un seuil est franchi.

C'est très utile pour surveiller l'espace disque. L'administrateur va definir un seuil d'alerte (ex: 80% Plein) et quand il est atteind, un mail est envoyé.

Plutôt qu'un mail, il est possible de creer des alertes dans Teams, ou Slack en configurant des webhooks.


#### conclusion

Nous avons ici un system de monitoring complet (metriques + logs système et applicatifs) avec des graphique facilement compréhensible et avec un système d'alerte en place. Ce qui est rassurant pour l'administrateur qui a definit ses seuils d'alertes afin de se laisser une marge de temps pour agir en conséquences.

Cela à été pour moi un projet très enrichissant car j'ai pu construire sur les bases que j'avais en Ansible pour arriver à produire un script fonctionnel avec plusieurs briques logiciels. J'ai rencontré certaine difficultés dans la compréhension du fonctionnement de certain module d'Ansible mais en perseverant et avec l'aide de **Mr Thomas Colenos** et **Mr Arthur Bertinetti** j'ai pu reusssir mes taches.

Ansible est une technologie qui m'interesse beaucoup et je suis très content d'avoir pu travailler dessus durant mon stage. J'ai par la suite créé d'autres script Ansible du type:

- installation / Configuration d'un serveur Apache
- Configuration d'un pool de machine Big Blue Button
- Deploiement d'une infrastructure complexe ( nginx, apache, drupal, mariabd, moodle, python )

Sur cette derniere j'ai rencontré des difficultés sur certain points. Mon responsable a pu utiliser une partie du travail que j'ai fait pour arriver à un script qui fonctionne. Grace a lui, j'ai appris de mes erreurs et pu grandement et efficacement ameliorer mes compétences en Ansible.


### Déploiment d'une stack complexe multi-services, multi-plateformes, multi-fournisseurs

## Conclusion

Ce stage correspondait parfaitement à ce que je recherché. Il m'a permit de d'apprendre et de perfectionner certaines de mes connaissances, notament tout ce qui touche à l'automatisation, au scripting, et à la gestion de plusieurs vm.


J'ai eu la chance au cours de ce stage de perfectionner sur des technologies comme Ansible ou Kubernetes qui me passionne.

Ce stage au sein d'une grande entreprise de service numérique de renommé mondial fut une expérience très enrichissante tant sur le plan personnel que professionnel. Cela m'a permis de conformter mon envie de travailler dans le secteur informatique en tant que DevOps. A 33 ans, en reconversion professionnelle, il faut etre concient de ses forces et faiblesse et je pense que j'ai fait le bon choix d'écouter ma passion pour en faire mon métier.

Le contexte actuel sanitaire a fait que j'étais en télétravail 99% du temps, ce qui ne rends pas forcement les choses faciles pour encadrer un stagiaire. **Mr Thomas Colenos** à parfaitement su me supperviser et m'apporter l'aide necessaire quand j'en avais besion. Il m'a laisser une grande autonomie et m'a permis de progresser enormement.

En parrallèle de ce stage, j'ai choisi de passer des certifications afin de valider mes compétences. J'ai obtenu une certification en cybersécurité (comptia security +), une certification sur Kubernetes (CKA: certified kubernetes administrator)  Je passe fin Septembre la certification RHCE (Red Hat Certified Engeneer), ce qui me permettra d'avoir un profil solide et d'exellente base pour exercer le metier de DevOps.

Pour conclure, j'ai eu une proposition d'embauche en CDI après à la suite de ce stage et j'ai accepté. Je vais pouvoir évoluer au sein d'une équipe dynamique, sur des prjets et des technologies intéressantes.


## Annexes:

Le playbook complet du deploiement est disponible sur mon git a l'addresse suivante: https://github.com/marc-cenon/rapport_de_stage.
Je vous invite à lire le readme.md pour comprendre l'arboresence du dossier.

Bucket dans influxdb:

![bucket influxdb](images/bucket.png "bucket Influxdb")


Dashboard Grafana

![grafana dashboard](images/grafana-dash.png "Grafana Dashboard")


Datasources dans Grafana

![grafana datasources](images/grafana-datasources.png "Grafana Datasources")


Example de construction de query dans Influxdb

![influxdb query](images/influxdb-query.png "Influxdb Query")


Diagram Influxdb et connecteurs

![influxdb diagram](images/influxdb_diagram.png "Influxdb Diagram")


Exemple de recuperation de log cron avec loki dans Grafana

![loki cron](images/loki-cron.png "Loki Cron")


Explication Influxdb

![influxdb](images/difference-relational-time-series.png "Influxdb TBS")

- tableau du travail semaine par semaine

