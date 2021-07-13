# rapport-stage
Bienvenue dans le dépot GitHub de mon rapport de stage.

Celui-ci à été rédigé avec le Markdown modifiée de Pandoc.
Vous trouverez ici toutes les sources utilisées.

# lecture du rapport
Le rapport est disponible en mardown et en pdf


## format markdown
Il s'agit du fichier rapport.md à la racine du dossier.

Vous pouvez le consulter [ici](rapport.md)


## format pdf

pré-requis :

* installer correctement pandoc [ici](http://pandoc.org/installing.html)

pour générer le pdf, on utilise la commande :

    pandoc rapport.md -o files/rapport.pdf -V fontsize=12pt -V linestretch=1 -V linkcolor=black --number-sections --table-of-contents -V documentclass=scrreprt -V lang=french 

vous pouvez le consulter [ici](files/rapport.pdf "rapport")

## playbook

Le playbook utilisé dans ce rapport de stage est disponible dans le dossier files


