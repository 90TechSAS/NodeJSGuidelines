#NodeJS Guidelines - ZenLabs

##Sommaire
1. [Objectifs du guide](#objectifs-du-guide)
2. [Fonctionnement de nos APIs](#fonctionnement-de-nos-lapis)
3. [Dépendances développeurs tiers](#dependances-developpeurs-tiers)
4. [Structure des dossiers dans un module NPM](#structure-des-dossiers-dans-un-module-npm)
5. [Structure des dossiers dans une API](#structure-des-dossiers-dans-une-api)


##Objectifs du guide
En mettant en place ce guide, l'objectif global est de permettre une certaine unité dans le développement de notre API. Pour cela, certains sous-objectifs sont à considérer :

1. Augmenter la lisibilité de votre code.
2. Définir explicitement fonctions, classes et variables.
3. Penser davantage modularité et développement orienté composant.
4. Documenter proprement pour qu'une seule lecture suffise.
5. Informer les autres développeurs de vos changements.
6. Penser à s'informer sur l'existence de composants déjà présents et reconnus.
7. Savoir se remettre en question vis-à-vis de ce guide.


##Fonctionnement de nos l'APIs
Nos APIs implémentent plusieurs modules NPM privés qui représentes leurs différentes ressources. Cette façon de procéder nous permet d'une part d'avoir une meilleure clarté au niveau organisation code, et d'autre part d'avoir une séparation des fonctionnalités.

Ces modules privés sont hébergés sur un registe NPM interne. Chaque modules regroupent les ressources créer dans une application bien précise (mais elles peuvent être utilisé par d'autres applications).

##Dépendances développeurs tiers

- [Baucis](https://github.com/wprl/baucis) - Baucis est un outil permettant de construire rapidement et proprement des API REST évolutive avec Mongoose, Express. Il génère une API classique à partir de modèle Mongoose.


##Structure des dossiers dans un module NPM

```
.
│   
└───lib
    └───controllers
        |   controller1.js
        |   controller2.j
        |   ...
    └───middlewares
        |   middleware1.js
        |   middleware2.j
        |   ...
    └───models
        |   model1.js
        |   model2.js
        |   ...
    └───test
    |   index.js
```

###Le dossier Controllers
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

###Le dossier Middlewares
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

###Le dossier Models
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

###Le dossier Test
Il permet de tester le module sans avoir besoin de publier ce dernier sur le registre NPM privé.


##Structure des dossiers dans une API
