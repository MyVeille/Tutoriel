# Qu'est-ce que MongoDB ?
###  Introduction 

Les bases de données (BD) sont omniprésentes dans la société actuelle. 
Pour les entreprises, elles permettent de stocker et d'organiser des données volumineuses de manière structurée, offrant ainsi une source unique et fiable d'informations importantes dans la prise de décisions.
Elles présentent de nombreux avantages, notamment en renforçant la sécurité avec des mécanismes de cryptage, tout en facilitant la collaboration entre les membres d'une entreprise grâce au partage sécurisée des informations. 
En outre, les bases de données sont essentielles pour les analyses avancées, ~~telles que l'apprentissage automatique et l'intelligence artificielle,~~ permettant ainsi aux entreprises de rester compétitives dans un environnement commercial en constante évolution.

Une base de données se montre donc très utile puisqu'elle regroupe toutes les informations au même endroit, facilitant ainsi le partage d'informations entre les différents départements. 

### Relation entre les documents
Il existe plusieurs types de base de donnée (BD) : 

Le type le plus courant est la base de données relationnelle. Dans ce type de BD, les données sont stocké sous forme de table (rangées et colonnes), et les informations contenues dans une table peuvent être liées aux informations d'une autre table. 

Voici un exemple de données liées que l'on peut retrouver dans une entreprise de ventes. 

**Table des clients**

| ID Client | Nom       | Adresse  |  
|:----------|:----------|:---------|
| 21        | Jean Kant | 332 Galt |
| 22        | Mary Lin  | 456 King | 
 
**Table des factures**

| # Facture | ID Client | Date       | Paiement | 
|:----------|:----------|:-----------|:---------|
| 18        | 22        | 20/11/2015 |          |

**Table des achats**

| ID Achat | # Facture | ID Produit | Quantité |
|:---------|:----------|:-----------|:---------|
| 0067     | 18        | 03         | 2        |
| 0068     | 18        | 11         | 5        |

Les produits seraient ensuite liée à une autre table contenant la description complète des produits.
 
Un autre type de BD est la base de données __structurée sous forme de documents__. Dans ce type de BD, les données sont enregistrées sous une forme similaire à un document JSON. 
En d'autres termes, les informations sont regroupées pour former une structure semblable à celle d'un arbre.

Le même exemple de ventes est utilisé avec une structure à base de documents :  

```python
# Voici les données des clients et leurs factures

{
	id: 22, 
	nom: "Mary Lin", 
	adresse: "456 King", 
	Facture:[
		{id:18, achats:[
				{Produit_Id: 03 , quantite: 2}, 
				{Produit_Id: 11, quantite: 5}
			]
		}
	]
},
{
	id: 21, 
	nom: "Jean Kant", 
	adresse: "332 Galt", 
	Facture:[]
}
	
```
Dans ce tutoriel, vous vous familiariserez à [MongoDB](https://www.mongodb.com/fr-fr), un système qui gère des bases de données construites sous forme de documents.

### Pourquoi utiliser mongoDB 

Mongo DB possède certains avantages et désavantages qui sont soit liée à la structure particulière des données ou simplement à Mongo lui-même. 

__Les points positifs :__

- Flexibilité : L'absence de schéma prédéfini permet de stocker n'importe quel type de données sans être restreint.
- Fragmentation : MongoDB permet de diviser et distribuer les données sur plusieurs serveurs, répondant ainsi à des problèmes de performance liées à de grand ensemble de données. 
- Support technique : MongoDB offre du support professionnel aux usagers.    

- Langage de programmation : MongoDB est un système de gestion de base de données avec lequel l'utilisateur peut communiquer en utilisant différent langage dont le langage python.
Cette caractéristique facilite l'utilisation de la BD par les développeurs.

__Pour les points négatifs :__  

- Jointure difficile : Contrairement à une base de données relationnelle, pour mongoDB, il est plus difficile d'associer des données qui ne sont pas dans la même table ou branche entre elles.
Il est possible de le faire manuellement à l'aide de code, mais cela prend plus de temps et peut affecter les performances.
- Redondance des données et utilisation de la mémoire : Puisque MongoDB conserve les données par paire (clés : valeurs), une certaine redondance des données est géré en raison des limitations des jointures.
Cette limitation entraine donc une utilisation plus élever de la mémoire. 
- Limite de taille : MongoDB possède une limite de 16MB de mémoire pour la taille de ses documents.    

Il existe bien évidement d'autres [avantages et désavantages](https://www.geeksforgeeks.org/mongodb-advantages-disadvantages/) qui peuvent être trouvé sur le web. 
 
## Se connecter à mongoDB

### 1. Produits

La plateforme MongoDB offrent plusieurs produits selon les besoins des usagers. 
Les liens de chaque produit vous guiderons vers les tutoriels d'installation officiels de MongoDB.

- [MongoDB Atlas](https://www.mongodb.com/docs/atlas?tck=docs_server) représente la version entièrement gérée et déployée sur le Cloud de MongoDB.
- [MongoDB Entreprise](https://www.mongodb.com/docs/manual/administration/install-enterprise/#std-label-install-mdb-enterprise) est une version proposée sous abonnement et gérée par l'utilisateur.
- [MongoDB Community](https://www.mongodb.com/docs/manual/administration/install-community/#std-label-install-mdb-community-edition) constitue une version libre d'accès, gratuite et également gérée par l'utilisateur. 

Selon le système d'exploitation utilisé, la méthode d'installation peut varier.  
Nous continuerons avec la version `Mongo Community` dans ce tutoriel.  

### 2. Driver 

Peu importe le produit choisi, pour communiquer avec MongoDB via le langage de programmation Python, un `Driver` est nécessaire pour établir le lien entre les requêtes effectuées sur l'interface locale (API : Application Programming Interface) et la base de données.
Ce `Driver` agit comme un pont entre ces deux entités.


Pour cela, nous pouvons utiliser [pymongo](https://www.mongodb.com/docs/drivers/pymongo/), qui permet une utilisation standard (synchrone), ou [motor](https://www.mongodb.com/docs/drivers/motor/), qui est adapté pour des utilisations asynchrones.
Lors de l'installation d'un des produits, 

L'outil [MongoCompass](https://www.mongodb.com/docs/compass/current/), offert par MongoDB, facilite la visualisation de la base de données et peut donc être intéressant à utiliser. 

## Opérations 

Après avoir installé le nécessaire, il est possible d'accéder à l'interface de programmation locale pour effectuer des requêtes.


```python
import pymongo

# Se connecter à MongoDB 
client = pymongo.MongoClient("mongodb://localhost:27017/")

# Sélectionner une base de données
db = client["ma_base_de_donnees"]

# Sélectionner une collection
collection = db["ma_collection"]

# Insérer un document
nouveau_document = {"nom": "John", "age": 30}
collection.insert_one(nouveau_document)

# Trouver un document
document_trouve = collection.find_one({"nom": "John"})
print(document_trouve)

# Mettre à jour un document
collection.update_one({"nom": "John"}, {"$set": {"age": 35}})

# Supprimer un document
collection.delete_one({"nom": "John"})

# Déconnecter du serveur MongoDB
client.close()

```

## Conclusion
En résumé, MongoDB représente une solution moderne et flexible pour la gestion de bases de données, et appréciée pour sa capacité à stocker des données de manière efficace sous forme de documents JSON.
Des outils conviviaux tels que MongoDB Compass simplifient la gestion et l'analyse des données, facilitant ainsi le travail des développeurs et des administrateurs de bases de données. 
Dans l'ensemble, MongoDB reste un élément central de l'infrastructure technologique actuelle, répondant aux besoins croissants de stockage et de traitement de données à grande échelle.

### Référence
La majorité des références ont été liées dans le texte du tutoriel.


__Vidéo en anglais__

[MongoDB + Python par Tech with Tim ](https://www.youtube.com/watch?v=UpsZDGutpZc), produite en 2022 

[MongoDB with python crash course - Tutorial for beginner](https://www.youtube.com/watch?v=E-1xI85Zog8), produite en 2019

[MongoDB Crash course with python 2022](https://www.youtube.com/watch?v=qWYx5neOh2s)

__Page web__

[ThinkAutomation - Mongo pros and cons](https://www.thinkautomation.com/our-two-cents/understanding-the-key-mongodb-pros-and-cons)

[GeeksForGeeks - MongoDB Advantages & Disadvantages](https://www.geeksforgeeks.org/mongodb-advantages-disadvantages/)

[w3schools - Python MongoDB](https://www.w3schools.com/python/python_mongodb_getstarted.asp)
