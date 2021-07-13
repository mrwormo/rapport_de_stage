# RAPPORT DE STAGE

Bienvenue dans le dépot GitHub de mon rapport de stage.

Celui-ci à été rédigé en Markdown et généré en pdf avec Pandoc

Vous trouverez ici toutes les sources utilisées.

# lecture du rapport
Le rapport est disponible en mardown et en pdf


## format markdown
Il s'agit du fichier rapport.md à la racine du dossier. Vous pouvez le consulter [ici](rapport.md)


## format pdf

vous pouvez consulter et télécharger le rapport au format pdf [ici](files/rapport.pdf "rapport")

Pour générer un pdf à partir du rapport en markdown, il est possible d'utiliser pandoc (installation [ici](http://pandoc.org/installing.html))

pour générer un pdf, on peut utiliser par exemple la commande suivante: 

```shell
pandoc rapport.md -o files/rapport.pdf -V fontsize=12pt -V linestretch=1 -V linkcolor=black --number-sections --table-of-contents -V documentclass=scrreprt -V lang=french 
```


## playbook

Le playbook utilisé dans ce rapport de stage est disponible dans le dossier files


