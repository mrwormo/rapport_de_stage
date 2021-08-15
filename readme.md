# Rapport de stage

Bienvenue dans le dépot GitHub de mon rapport de stage et de la présentation pour la soutenance pour la licence LPRO ADSILLH


Le rapport de stage a été rédigé en Markdown, avec quelques éléments de Latex pour la mise en page.
Pandoc est utilisé pour générer le **PDF**

La soutenance est au format **odp** et **pdf**

# Theme du rapport

J'ai choisi le theme de **l'automatisation**, plus présicément l'utilisation d'**Ansible** et le déploiement d'une **solution de monitoring** pour surveiller une infrastructure.


# Lecture du rapport

Le rapport est disponible en **mardown** et en **pdf**


## Format Markdown

Le rapport est également consultable au format **Markdown** à la racine du dossier. 

Vous pouvez consulter le document rapport.md [ici](rapport.md)


## Format Pdf

vous pouvez consulter et télécharger le rapport au format **pdf** [ici](files/rapport.pdf "rapport")

Pour générer un pdf à partir du rapport en markdown, il est possible d'utiliser pandoc (installation [ici](http://pandoc.org/installing.html))

pour générer un pdf, on peut utiliser par exemple la commande suivante: 

```shell
pandoc --listings -H text.tex rapport.md -o files/rapport.pdf --pdf-engine=xelatex
```
 ou

```shell
cat pdf_built.txt | sh
```

## Présentation pour la soutenance
La présentation au format **.odp** et **.pdf** est disponible dans le dossier **files**
Vous pouvez accéder au **PDF** directement [ici](files/prez.pdf "présentation")

## Playbook

Le playbook utilisé dans ce rapport de stage est disponible dans le dossier **files**
