# TP4: Ingestion de logs avec filebeat

Le but de ce TP est de configurer filebeat pour ingérer des logs apache d'un serveur de production.

## Installation de filebeat

Récupérer le fichier .tar.gz fourni et le décompresser dans votre dossier utilisateur.

Editer le fichier filebeat.yml pour y ajouter votre configuration ElasticSearch et Kibana :

```yaml
setup.kibana:
  host: "localhost:5601"

output.elasticsearch:
  hosts: ["localhost:9200"]
  preset: balanced
  protocol: "https"
  username: "elastic"
  password: "VOTRE_MOT_DE_PASSE"
  ssl.verification_mode: "none"
```

Activer le module apache

```bash
./filebeat modules enable apache
```

Configurer le module apache 

```yaml
# fichier : modules.d/apache.yml
- module: apache
  access:
    enabled: true
    var.paths:
      - /partage/access.log

  # Error logs
  error:
    enabled: false
~              
```

Lancer la génération des dashboards et pipelines

```bash
./filebeat setup dashboards
```

## Ingestion des données et monitoring

Activer le monitoring dans Kibana -> stack monitoring. Repérer les différents écrans importants.

Lancer ensuite l'ingestion des données :

```bash
./filebeat
```
Monitorer l'ingestion (plusieurs millions de lignes).

Une fois l'ingestion terminée, repérer l'index qui contient les logs. Accéder au dashboard apache prédéfini et l'étudier.

Créer un dashboard personalisé qui affiche les requêtes 200 OK et 404 dans un graphique temporel.











