# TP1 : Installation type utilisant un serveur Elasticsearch pour de l'indexation de données.

Le but de ce TP est d'apprendre à 

* Installer elasticsearch
* Découvrir les commandes de base de la console
* Injecter un fichier de données
* Installer Kibana
* Découvrir les fonctions de base de Kibana

---

Prérequis de l'environnement :

* curl (inclus dans git bash, ou à télécharger/dézipper)
* console (git bash, cmder ou autre)
* Google Chrome

---

## 1ere partie

### Installation de Elasticsearch et Kibana

#### Installation d'ElasticSearch

* Récupérer l'archive sur la page https://www.elastic.co/fr/downloads/elasticsearch
* Décompresser l'archive
* Ouvrir un terminal dans le dossier décompressé, puis lancer la commande suivante

    bin/elasticsearch

Vérifier que le service est bien lancé en accédant à http://localhost:9200 depuis votre navigateur.

#### Installation de Kibana

* Récupérer l'archive sur la page https://www.elastic.co/fr/downloads/kibana
* Décompresser l'archive
* Ouvrir un terminal dans le dossier décompressé, puis lancer la commande suivante

    bin/kibana

Vérifier que le service est bien lancé en accédant à http://localhost:5601 depuis votre navigateur.

### Utilisation de la console "dev tools" dans Kibana

Accéder à Kibana, et naviguer dans le menu "Dev tools" pour accéder à la console.

Découvrir l'auto-complétion, les options et outils de la console, et exécuter les requêtes suivantes : 

    GET /
    GET /_cluster/health
    GET /_nodes
    GET /_stats
    GET /_cat/indices?v


Insérer un peu de données, exemple :

    POST /users/_doc
    {
        "name":"Joe",
        "age": 32
    }

Demander à nouveau les informations du cluster et des indices

    GET /_cat/indices?v

Pourquoi est-il en statut yellow ?

### 2ème partie 

### Injection de donnés

Insertion de 1000 enregistrements avec curl et la bulk API

* Ouvrir un terminal
* Lancer la commande suivante dans le dossier qui contient `accounts.bulk` 

`curl -XPOST localhost:9200/bank/_bulk -H"Content-Type: application/json" --data-binary @accounts.bulk`

Le fichier accounts.bulk contient une liste comptes bancaires, dont voici un élément :

```
{
	"account_number": 13,
	"balance": 32838,
	"firstname": "Nanette",
	"lastname": "Bates",
	"age": 28,
	"gender": "F",
	"address": "789 Madison Street",
	"employer": "Quility",
	"email": "nanettebates@quility.com",
	"city": "Nogal",
	"state": "VA"
}
```


### Navigation et découverte des données avec Kibana

Premiers pas avec Kibana

* Accèder à Kibana http://localhost:5601
* Ajouter le nom de l'index créé (bank) dans un nouvel "index pattern"
* Utiliser ensuite l'onglet discover pour visualiser les données
* Faire des recherches simples dans le champ principal (exemple : name:Joe,age:28)

### 3ème partie

### Sauvegarde de vues dans kibana

Création d'un dashboard de données

* Sauvegarder des vues, triées selon l'age, le compte en banque, etc...
* Bien nommer ces vues
* Présenter ces vues dans l'onglet dashboard

### Création de graphiques

Dans l'onglet "Visualize" créer des "Metric"

* Moyenne d'age
* Nombre de comptes
* Max, min et moyenne du compte en banque

Les ajouter au dashboard précédemment créé.

### Bonus

Découvrir les "agrégations" avec les autres types de graphiques (area, pie chart, line chart, ...)

* Créer et sauvegarder des graphiques (exemple : compte en banque moyen par tranches d'age et par genre)
* A vous d'imaginer d'autres vues selon leur pertinence
* Ajouter les graphiques au dashboard



