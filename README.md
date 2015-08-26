# NodeJS Guidelines - 90Tech

## Sommaire
1. [Objectifs du guide](#objectifs-du-guide)
2. [Fonctionnement de nos APIs](#fonctionnement-de-nos-lapis)
3. [Dépendances développeurs tiers](#dependances-developpeurs-tiers)
4. [Structure des dossiers dans un module NPM](#structure-des-dossiers-dans-un-module-npm)
5. [Structure des dossiers dans une API](#structure-des-dossiers-dans-une-api)


## Objectifs du guide
En mettant en place ce guide, l'objectif global est de permettre une certaine unité dans le développement de nos APIs. Pour cela, certains sous-objectifs sont à considérer :

1. Augmenter la lisibilité de votre code.
2. Définir explicitement fonctions, classes et variables.
3. Penser davantage modularité et développement orienté composant.
4. Documenter proprement pour qu'une seule lecture suffise.
5. Informer les autres développeurs de vos changements.
6. Penser à s'informer sur l'existence de composants déjà présents et reconnus.
7. Savoir se remettre en question vis-à-vis de ce guide.


## Fonctionnement de nos APIs
Nos APIs implémentent plusieurs modules NPM privés qui représentent leurs différentes ressources. Cette façon de procéder nous permet d'une part d'avoir une meilleure clarté au niveau organisation du code, et d'autre part d'avoir une séparation des fonctionnalités.

Ces modules privés sont hébergés sur un registe NPM interne. Chaque modules regroupent les ressources créées dans une application bien précise (mais elles peuvent être utilisées par d'autres applications).

## Dépendances développeurs tiers

- [Baucis](https://github.com/wprl/baucis) - Baucis est un outil permettant de construire rapidement et proprement des API REST évolutive avec [Mongoose](http://mongoosejs.com/) et [Express](http://expressjs.com/). Il génère une API classique à partir de modèles Mongoose.


## Structure des dossiers dans un module NPM

```
.
└───lib
    │   index.js
    │
    ├───controllers
    │       controller1.js
    │       controller2.j
    │       ...
    ├───middlewares
    │       middleware1.js
    │       middleware2.j
    │       ...
    ├───models
    │       model1.js
    │       model2.js
    │       ...
    └───tests
        
```

### Controllers
Les différents modèles Mongoose vont se trouver dans ce dossier.

Voilà à quoi ressemble un modèle :
```javascript
/* A appliquer ! */

var Ressource = require('../models/ressource');

// On charge les middlewares dont on a besoin
var middleware1 = require('../middlewares/middleware1'),
    middleware2 = require('../middlewares/middleware2');

module.exports = function(baucis) {

    var controller = baucis.rest(Ressource);

    controller.request('post', middleware1);

    controller.query('get', middleware2);

};
```

### Middlewares
Ce dossier contient des middlewares qui permettent de mieux "paramétrer" l'API. 

La structure est classique :
```javascript
/* A appliquer ! */

module.exports = function(req, res, next) {

    // On met la logique du middleware
    ...

    next();

};
```

### Models
Les différents modèles Mongoose vont se trouver dans ce dossier.

Voilà à quoi ressemble un modèle :
```javascript
/* A appliquer ! */

var mongoose = require('mongoose'),
    Schema = mongoose.Schema;

var ressourceSchema = new Schema({

    attribut1: {
        type: String,
        required: true
    },

    ...

});

module.exports = mongoose.model('Ressource', ressourceSchema, 'appName.Ressources');
```

### Tests
A écrire

## Structure des dossiers dans une API
```
.
│
│   app.js
│  
└───config
        core.js
        development.js
        production.js

```

### Config
Nous allons trouver 3 fichiers différents dans ce dossier.

La configuration générale se trouve dans **core.js**, tandis que les configurations spécifique dépendant de l'environnement de l'application sont dans **development.js** et **production.js**.

De cette façon, nous avons le fichier **core.js** qui charge un des deux autres fichiers en fonction de l'environnement.

Exemple de core.js :
```javascript
/* A appliquer ! */

var _ = require('underscore');

process.env.NODE_ENV = process.env.NODE_ENV || 'development';

var config = {
    db: 'myDatabaseAddresse',
    port: myDatabasePort,
    ...
};

module.exports = _.extend(
    config,
        require('./' + process.env.NODE_ENV) || {}
);

```

### app.js
C'est dans ce fichier que nous allons utiliser Baucis pour générer nos APIs.

```javascript
/* A appliquer ! */

var baucis = require('baucis');

var config = require('./config/core.js');

var app = express();

...

var apiApp1 = require('private-package-api-app1')(baucis),
    apiApp2 = require('private-package-api-app2')(baucis),
    ...

app.use('/app1', apiApp1);
app.use('/app2', apiApp2);
...



```
