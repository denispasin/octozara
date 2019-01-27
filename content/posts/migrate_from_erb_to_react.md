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

Si vous voulez suivre la vidéo en même temps: [Youtube](https://youtu.be/IhEM7-FxmF0?t=384) (jusqu'à 26:52)

On va commencer par faire le bout de plomberie qui va nous permettre de travailler ensuite. C'est à dire amener une app React minimaliste à l'intérieur d'une de nos vues.

Ces étapes là vont aller assez vite.

### Installation de Webpacker

On va directement suivre la [doc de webpacker](https://github.com/rails/webpacker) pour ça (et on va prendre soin d'utiliser la version @next de webpacker qui apporte un bon lot de fonctionnalités). 

On ajoute a notre `Gemfile`:

```ruby
gem 'webpacker', '>= 4.0.x'
```

Puis on installe les différents packages

```bash
bundle install
yarn add @rails/webpacker@next
```

Enfin on initialise Webpacker:

```bash
bundle exec rails webpacker:install
```

Ici vous avez déjà webpacker presque opérationnel. On va effectuer quelques petites modification dans notre config/webpacker.yml:

* Remplacer tous les `localhost` par `0.0.0.0`. Ça fait que vous pouvez accéder a votre site sur n'importe quelle URL et vous aurez accès a Webpack.
* Changer `extract_css` à `true` ça fera que tous vos fichiers CSS importés seront automatiquement exportés.
* En development, changez `hmr` à `true`. Vous ne déclencherez plus de full reload a chaque changement de component dans React.

### Installation de React

On laisse webpacker ajouter les fichiers minimaux pour notre app React:

```
bundle exec rails webpacker:install:react
```

On va ensuite renommer ça et déplacer des fichiers. C'est l'occasion de parler un peu du dossier `app/javascript`. Vous aurez dedans un dossier spécial nommé `packs`.
Dans ce dossier ne devra aller que les endpoints que vous voulez exposer dans votre application (ici on en veut qu'un seul: `application.js`).

* Supprimez le fichier `app/javascript/packs/application.js`.
* Renommez le fichier `app/javascript/packs/hello-react.jsx` en `app/javascript/packs/application.jsx`.
* Créez un dossier `app/javascript/src`. C'est ici que nous mettrons tous les autres fichiers de l'application.
* Créez un fichier `app/javascript/src/App.jsx`. Dans ce fichier créez un component React minimaliste.
* Éditez votre fichier `app/javascript/packs/application.jsx` pour importer le component `App` et le rendre et supprimez le component par défaut contenu dans le fichier.

Normalement si vous avez bien tout fait,vos fichiers devraient ressembler à:

{{< filename "app/javascript/packs/application.jsx" >}}
```js application.jsx
// Run this example by adding <%= javascript_pack_tag 'hello_react' %> to the head of your layout file,
// like app/views/layouts/application.html.erb. All it does is render <div>Hello React</div>div> at the bottom
// of the page.

import React from 'react';
import ReactDOM from 'react-dom';
import App from '../src/App';

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(<App />, document.getElementById('app-container'));
});
```

{{< filename "app/javascript/src/App.jsx" >}}
```js App.jsx
import React, { Component } from 'react';

export default class App extends Component {
  render() {
    return <div>Hello from React :)</div>;
  }
}
```

Votre "App" est prête a être plug dans vos views classiques.

### Plug de votre application dans votre/vos views

Dans votre view (ici `/app/view/posts/index.js`), on va ajouter en fin de fichier la ligne:

```html
<%= javascript_pack_tag 'application' %>
```

De plus on va ajouter la div qui recevra notre app a terme:

```html
<div id="app-container"></div>
```

Il reste plus qu'à tester maintenant.

### Lancez votre app webpack-dev-server

Maintenant il faut lancer les deux serveurs dans 2 terminaux différents:

```bash
# Votre serveur Rails classique
rails s

# Le serveur de dev webpack qui va se charger de recompiler vos JS a chaque changement.
bin/webpack-dev-server
```

## Votre première vue

### Méthodologie
### Récupérer des données

Si vous voulez suivre la vidéo en même temps: [Youtube](https://youtu.be/IhEM7-FxmF0?t=1598) (jusqu'à 53:13)

### Envoyer des données et actions

Si vous voulez suivre la vidéo en même temps: [Youtube](https://youtu.be/IhEM7-FxmF0?t=3192) (jusqu'à 1:12:56)

## Et après ?

### Le Routing
### Le CSS
### Le déploiement
