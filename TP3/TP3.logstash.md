# TP3 Utilisation de Logstash et graphiques Kibana

Le but de ce TP est d'apprendre à utiliser Logstash pour peupler un index ElasticSearch, d'executer des requêtes avancées via la console DevTools et créer des visualisations avancées dans Kibana

Fichiers utilisés (fournis) :

* Données `sncf-non-conformites.csv`
* Mapping de l'index : `sncf-mappings.txt`
* Configuration Logstash : `sncf.conf`

## Création des mappings

Analyser le fichier `sncf-mappings.txt` et le jouer dans la console DevTools.

Apporter des modifications si besoin (type de champs, etc...)

## Insertion de données

Récupérer le fichier de configuration Logstash. Ouvrir et éditer le fichier si besoin (chemin absolu du fichier de données, nom de l'index...)

Lancer l'import des données 

`bin/logstash -f /chemin/complet/vers/sncf.conf`

Vérifier que la base de données est bien remplie en explorant les données avec Kibana ou en utilisant la commande :

```
GET sncf/_count
```

## Graphiques Kibana

Dans Kibana :

Afficher l'évolution du nombre de non conformités, mois par mois, toutes gares comprises

Sur le même graphique :
Afficher l'évolution du nombre d'observations

* Imaginer des vues ayant de la pertinence en fonction des données
* Créer un graphique de chaque type ayant du sens
* Modifier les options des graphiques pour influer sur les échelles et couleurs des graphiques

## Ajout de champs virtuels

Ajouter ces champs au mapping: 

```

PUT sncf/_mapping
{
  "runtime": {
      "taux_v2": {
        "type": "double",
        "script": {
          "source": """
          if( doc['Nombre_observations'].size()!=0 && doc['Nombre_Non_Conformites'].size() != 0 && doc['Nombre_observations'].value > 0 && doc['Nombre_Non_Conformites'].value > 0)
            emit(1.0* doc['Nombre_Non_Conformites'].value / doc['Nombre_observations'].value);
          else
            emit(0);
          """
        }
      },
      "hour_of_day": {
        "type": "double",
        "script": {
          "source": """
          if(doc['Fin_controle'].size()!=0 )
            emit(doc['Fin_controle'].value.getHour());
          else
            emit(0);
          """
        }
      },
      "day_of_week": {
        "type": "keyword",
        "script": {
          "source": """
          if(doc['Fin_controle'].size() == 0)
            emit('NA');
          else
          emit(doc['Fin_controle'].value.dayOfWeekEnum.getDisplayName(TextStyle.FULL, Locale.ROOT));
          """
        }
      }
    }
}
```

Réaliser les graphiques suivants :

- Affichage de l'évolution du taux de conformité
- Affichage des problèmes par jour de la semaine
- Affichage des problèmes par heure du jour



