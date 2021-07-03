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

Ansible permet d'automatiser la configuration à plusieurs différents niveaux (systèmes d’exploitation, composantes d’application), et peut être appliqué à différents équipements (serveur, stockage, réseau) ou infrastructures (Bare metal, VM , cloud). 

Ansible s'inscrit dans la catégorie IaC: Infrastructure as Code, c'est à dire de gérer la configuration d'une infrastrucre à l'aide de fichier de configuration

Avec le développement des infrastrucre Cloud, Ansible, couplé à des outils comme Terraform et Packer permet de gérer un infrastructure Cloud en mode IaC.

Personnelement, je ne vois que des avantages dans ce mode de gestion IaC. C'est ce que j'utilise pour gérer mon homelab (voir annexe)
## Déploiment d'une stack de monitoring par Ansible

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

