---
title: "Migrer depuis Erb vers React"
date: 2019-01-24T14:51:13+09:00
type: post
categories:
  - front
  - code
---

Le but de cet article est de donner une marche a suivre pour migrer à sa propre vitesse d'une front rails classique (ici à base de Erb) vers une solution basée sur un front en JS (ici en React) géré par Webpacker.

{{< youtube IhEM7-FxmF0 >}}

## Présentation de l'app

On va ici prendre une application assez basique comme exemple mais c'est repliable juste plus long sur des applications de taille normale. 

C'est une seule page pour visualiser des posts (ou route) et 3 actions, Create/Update/Delete.
La page est protégée par une authentification via devise et affiche au besoin des erreurs de créations/update.
De plus l'application vérifie que vous ayez bien les droits d'effectuer une action avant de la faire via la gem pundit.

Vous pourrez trouver le code [ici](https://github.com/denispasin/rails_to_webpack/tree/start_rails).

Ceci devrait ressembler peu ou prou à la stack d'une application standard de Rails.

## Présentation de Webpacker

Webpacker est un moyen d'utiliser facilement Webpack en conjonction avec Rails.

Qu'est ce que Webpack ? C'est un outil pour bundle et compiler des assets ensemble à l'image de l'assets pipeline de Rails.
Il est très utilisé dans l'écosystème JavaScript.

(Si vous vous demandez pourquoi facilement, je vous renvoi vers mon article sur le [sujet](../2018-03-30-webpack-basics).)

## Pourquoi feriez vous ça ?

Plusieurs raisons peuvent vous pousser a amener Webpacker dans votre application.

* Vous voulez utiliser facilement les _nombreux_ packages disponibles de l'écosystème JavaScript sans le brancher dans l'assets pipeline.
* Vous voulez amener React/Vue/Else dans votre application et ne voulez pas recréer une application front ex-nilo mais déplacer petit à petit des bouts de votre app vers une de ces applications sans recoder toutes les vues de Devise par exemple.

Le présent article s'adresse plus au point 2 qu'au point 1 même si vous pourriez utiliser pas mal de points de cet article pour vous aider dans le premier cas.

## Initialisation de Webpacker
### Installation de Webpacker
### Installation de React
### Plug de votre application dans votre/vos views

## Votre première vue

### Méthodologie
### Récupérer des données
### Envoyer des données et actions

## Et après ?

### Le Routing
### Le CSS
