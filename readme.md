# Rapport de stage


Bienvenue dans le dépot GitHub de mon rapport de stage.

Celui-ci à été rédigé en Markdown et généré en pdf avec Pandoc

Vous trouverez ici toutes les sources utilisées.


# Lecture du rapport

Le rapport est disponible en mardown et en pdf


## Format Markdown

Le rapport est également consultable au format Markdown à la racine du dossier. 

Vous pouvez consulter le document rapport.md [ici](rapport.md)


## Format Pdf

vous pouvez consulter et télécharger le rapport au format pdf [ici](files/rapport.pdf "rapport")

Pour générer un pdf à partir du rapport en markdown, il est possible d'utiliser pandoc (installation [ici](http://pandoc.org/installing.html))

pour générer un pdf, on peut utiliser par exemple la commande suivante: 

```shell
pandoc rapport.md -o files/rapport.pdf -V fontsize=12pt -V linestretch=1 -V linkcolor=black --number-sections --table-of-contents -V documentclass=scrreprt -V lang=french 
```


## Playbook

Le playbook utilisé dans ce rapport de stage est disponible dans le dossier files


