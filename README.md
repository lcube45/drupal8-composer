# Drupal 8 et Composer
https://www.drupal.org/docs/develop/using-composer
https://getcomposer.org/

Composer est un gestionnaire de dépendances pour PHP.

## Références et sources
https://www.lullabot.com/articles/drupal-8-composer-best-practices

## Pourquoi utiliser Composer
Drupal 8 Core utilise Composer pour gérer ses dépendances (ex: Symfony, Guzzle, …)

Composer permet de :
- Identifier l’emplacement
- Télécharger
- Valider les dépendances
- Inclure les dépendances

## Initialiser un nouveau projet
https://github.com/drupal-composer/drupal-project

```
composer create-project drupal-composer/drupal-project:8.x-dev <project-path> --stability dev --no-interaction
```

**Arborescence projet obtenu** :

```
<project-path>
  /.git
  /drush
  /scripts
  /vendor
  /web
    .gitignore
    composer.json
    composer.lock
```

## Ajouter un module
Ajouter les sources du module dans votre code source.
```
composer require drupal/modulename:^1.0
```

Ajouter un module en ciblant un commit spécifique
```
composer require drupal/modulename:dev-[branch]#[commit-hash]
```

Comprendre les déclarations de versions pour Composer : https://getcomposer.org/doc/articles/versions.md

## Mettre à jour un module
Mettre à jour le code source du module en tenant compte des dépendances
```
composer update drupal/modulename --with-dependencies
```
Attention ceci ne met à jour que votre code source, il faut lancer l'update de Drupal
si besoin de mises à jour en base de données :

```
drush updatedb
```

Vérifier le statut des différents modules
```
composer outdated
```

## Supprimer un module
Attention au préalable de le désinstaller via l’UI ou via Drush ou via Drupal Console

```
composer remove drupal/modulename
```

## Régénérer le fichier composer.lock
```
composer update nothing
```

## Gérer les modules spécifiques au développement
```
composer require --dev drupal/modulename
```

Installer les dépendances en ignorant les modules de développement

```
composer install --no-dev
```

## Versionner le fichier composer.lock
https://getcomposer.org/doc/01-basic-usage.md#composer-lock-the-lock-file

## Patcher des modules
Un plugin de gestion de patches est disponible pour composer : https://github.com/cweagans/composer-patches

```
composer require cweagans/composer-patches:^1.0
```

Il faut ensuite ajouter une collection de patches à la section extra de notre fichier composer.json

```
 "extra": {
   "patches": {
     "drupal/core": {
        "2631468 - Menu subtrees in menu blocks show all subitems regardless of the active menu item": "https://www.drupal.org/files/issues/drupal-n2631468-95.patch"
      },
      "drupal/panelizer": {
        "2793841 - Properly integrate with Panels IPE": "https://www.drupal.org/files/issues/panelizer-panels-ipe-tempstore-id.patch",
      	"2664574 - Add support for Taxonomy Term entities": "https://www.drupal.org/files/issues/2664574-26.patch"
      },
      "drupal/panels": {
        "2793801 - Allow modules to influence the IPE tempstore ID": "https://www.drupal.org/files/issues/2793801-9.patch"
      }
    }
  }
```

## Supprimer le module à patcher, récupérer le module et appliquer le patch
```
composer update none
```

## Générer un artefact pour livraison en production (build)
```
composer install --no-dev --optimize-autoloader
```
