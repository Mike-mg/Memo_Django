# Memo Django -installation / utilisation ( Gnu/Linux )

## Création d'un environnement virtuel dans le projet  
- Créer le dossier du projet  
`mkdir nom_du_projet`  

- Se rendre dans le dossier du projet  
`cd nom_du_projet`  

- Créer un dossier d'environnement ".venv"  
`mkdir .venv`  

- Installer l'environnement du projet ( Etre dans le dossier du projet )  
`pipenv install` 

- Activer l'environnement du projet  
`pipenv shell`  
---

## Installation ( Python / Django )  
**Installation de Python**  
- Python est installé par defaut sur Linux\Mac  

- Windows suivre le lien suivant : https://www.python.org/downloads/  

**Installation de Django**  
- Documentation officielle 4.1 : https://docs.djangoproject.com/fr/4.1/  

- Créer un environnement virtuel de votre projet ( Voir plus haut )  

- Installation de Django ( Etre dans le dossier du projet et activer l'environnement virtuel )  
`pipenv install django`  

- Verifier qu'il soit bien installé  
`django-admin --version`  
---

## Tutoriel : 1ère partie - requêtes et réponses  
**Création d'un projet Django**  
- Création du projet ( l'environnement viruel doit etre activer )  
`django-admin startproject nom_du_projet .`  
    - Attention au "." a la fin de la commande qui est important. Cela va créer un dossier 'nom_du_projet' dans le dossier courant  

**Le serveur de développement**    
- Lancement du serveur ( port par defaut : 8000 )  
`python manage.py runserver`  

- Modification du port si besoin  
`python manage.py runserver 8080`  

- Modification de l'IP si besoin  
`python manage.py runserver 0.0.0.0:8000`  

**Les applications**
- Quelle est la différence entre un projet et une application ?  
    - Une application est une application Web qui fait quelque chose – par exemple un système de blog, une base de données publique ou une petite application  

    - Un projet est un ensemble de réglages et d’applications pour un site Web particulier
    
    - Un projet peut contenir plusieurs applications. Une application peut apparaître dans plusieurs projets  

- Création de l'application dans le dossier parent  
`django-admin startapp nom_de_application`  
    - Cela va créer un dossier 'nom_de_application' dans le dossier courant  

- Création de l'application dans un sous-dossier 'apps' du projet  
    - Si l'on souhaite installer les applications dans un sous-dossier, il faut le créer avant  
        `mkdir -p monsite/apps/nom_de_application`  
    - Ensuite créer l'application  
        `django-admin startapp nom_de_application apps/nom_de_application`  

**Les vues**  
- Les vues se trouve dans le fichier 'views.py' du dossier de l'application  
    - Ex de fichier qui affiche 'Hello World !'
        ```
        from django.http import HttpResponse  

        def index(request):  
            return HttpResponse("Hello, world !")  
        ```

- Pour créer un URLconf dans le dossier de l'application, créer un fichier nommé 'urls.py'  
`mkdir urls.py`  

- Dans ce fichier 'urls.py'  
    - Importer 'path' de 'django.urls' et le module des vues
    - Créer une variable 'urlpatterns' de type liste et y inserer le routage de/des vue(s) comme indiquer ci-dessous grace a 'path'

        ```
        from django.urls import path
        from . import views

        urlpatterns = [
            path('', views.index, name='index'),
        ]
        ```

- L’étape suivante est de faire pointer le module 'urls.py' du projet vers le module 'urls.py' de l'application  

- Dans nom_du_projet/urls.py, ajoutez une importation django.urls.include et insérez un appel include() dans la liste urlpatterns, ce qui donnera  
```  
from django.contrib import admin  
from django.urls import include, path  

urlpatterns = [  
    path('nom_de_application/', include('nom_de_application.urls')),  
    path('admin/', admin.site.urls),  
]  
```  
---
## Tutoriel : 2ème partie : les modèles et le site d’administration  

#### **- Configuration de la base de données**  
- Parametrer la base donnée avec le fichier monsite/settings.py avec la variable DATABASES qui est un dictionnaire  

    ```
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.sqlite3",
            "NAME": BASE_DIR / "db.sqlite3",
        }
    }
    ```  
    - ENGINE : Leconnecteur de la base de donnée  
    
    - NAME :  Le nom de votre base de données. Si vous utilisez SQLite, la base de données sera un fichier sur votre ordinateur. Dans ce cas, NAME doit être le chemin absolu complet de celui-ci, y compris le nom de fichier. La valeur par défaut, BASE_DIR / 'db.sqlite3', stocke ce fichier dans le dossier de votre projet  

    - Si vous utilisez une autre base de données que SQLite, des réglages supplémentaires doivent être indiqués, comme USER, PASSWORD ou HOST. Pour plus de détails, consultez la documentation de référence de DATABASES ci-dessous  
        - https://docs.djangoproject.com/fr/4.1/ref/settings/#std-setting-DATABASES  

- Définir le bon TIME_ZONE  
    - France : CET  

- Reglage de INSTALLED_APPS  
    - C'est ici qu'il faudra ajouter les nouvelles applications créées  

- Les migrations  
`python manage.py migrate`  
    - La commande migrate examine le réglage INSTALLED_APPS et crée les tables de base de données nécessaires en fonction des réglages de base de données dans votre fichier mysite/settings.py et des migrations de base de données contenues dans l’application. Vous verrez apparaître un message pour chaque migration appliquée  

#### **- Création des modèles**  
- Un modèle est la source d’information unique et définitive pour vos données. 

- Il contient les champs essentiels et le comportement attendu des données que vous stockerez.

- Le but est de définir le modèle des données à un seul endroit, et ensuite de dériver automatiquement ce qui est nécessaire à partir de celui-ci. Ceci inclut les migrations. les migrations sont entièrement dérivées du fichier des modèles et ne sont au fond qu’un historique que Django peut parcourir pour mettre à jour le schéma de la base de données pour qu’il corresponde aux modèles actuels  

- Ces concepts sont représentés par des classes Python. Éditez le fichier polls/models.py de façon à ce qu’il ressemble à ceci  
    - Eemple avec des 2 modeles 'Question' et 'Choice'  

        ```
        from django.db import models


        class Question(models.Model):
            question_text = models.CharField(max_length=200)
            pub_date = models.DateTimeField('date published')


        class Choice(models.Model):
            question = models.ForeignKey(Question, on_delete=models.CASCADE)
            choice_text = models.CharField(max_length=200)
            votes = models.IntegerField(default=0)
        ```
- Ici, chaque modèle est représenté par une classe qui hérite de django.db.models.Model. Chaque modèle possède des variables de classe, chacune d’entre elles représentant un champ de la base de données pour ce modèle.

- Chaque champ est représenté par une instance d’une classe Field – par exemple, CharField pour les champs de type caractère, et DateTimeField pour les champs date et heure. Cela indique à Django le type de données que contient chaque champ.  

- Le nom de chaque instance de Field (par exemple, question_text ou pub_date) est le nom du champ en interne. Vous l’utiliserez dans votre code Python et votre base de données l’utilisera comme nom de colonne.  

- Vous pouvez utiliser le premier paramètre de position (facultatif) d’un Field pour donner un nom plus lisible au champ. C’est utilisé par le système d’introspection de Django, et aussi pour la documentation. Si ce paramètre est absent, Django utilisera le nom du champ interne. Dans l’exemple, nous n’avons défini qu’un seul nom plus lisible, pour Question.pub_date. Pour tous les autres champs, nous avons considéré que le nom interne était suffisamment lisible.

- Certaines classes Field possèdent des paramètres obligatoires. La classe CharField, par exemple, a besoin d’un attribut max_length. Ce n’est pas seulement utilisé dans le schéma de base de la base de données, mais également pour valider les champs, comme nous allons voir prochainement.

- Un champ Field peut aussi autoriser des paramètres facultatifs ; dans notre cas, nous avons défini à 0 la valeur default de votes.

- Finalement, notez que nous définissons une relation, en utilisant ForeignKey. Cela indique à Django que chaque vote (Choice) n’est relié qu’à une seule Question. Django propose tous les modèles classiques de relations : plusieurs-à-un, plusieurs-à-plusieurs, un-à-un.

#### **- Activation des modèles**  
#### **- Jouer avec l’interface de programmation (API)**  
#### **- Introduction au site d’administration de Django**  
- Création d’un utilisateur administrateur  
- Démarrage du serveur de développement
- Entrée dans le site d’administration
- Rendre l’application de sondage modifiable via l’interface d’admin
- Exploration des fonctionnalités de l’interface d’administration


## Tutoriels non traités  
- 3ème partie : vues et gabarits  
- 4ème partie : formulaires et vues génériques  
- 5ème partie : tests  
- 6ème partie : fichiers statiques  
- 7ème partie : personnalisation du site d’administration  
---
##  Tutoriel avancés non traités  
- Comment écrire des applications réutilisables
- Écriture de votre premier correctif pour Django  

<!-- ## Configuration et utilisation de Django ( Modèle MVT )   
1. Configuration du fichier 'settings.py' du projet ( Dans le sous-dossier du dossier principal )  
    - Structure du fichier settings.py  
        > BASE_DIR : Chemin parent du projet  
        
        > DEBUG = True : Affiche les erreurs du programme ( Ne pas laisser 'True' en production )  
        
        > DATABASES : Gestion de la base de donnée  
        
        > LANGUAGE_CODE : Langage du code (fr-FR pour mettre en francais )  
        
        > INSTALLED_APPS : Applications par defaut et celles créées pour le projet  

2. Les applications
    - ... -->
