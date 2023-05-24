# Memo sur l'installation de Django et utilisation des commandes sur Gnu/Linux
## Créer un environnement virtuel de votre projet  
1. Créer le dossier du projet  
`mkdir nom_du_projet`  

2. Se rendre dans le dossier du projet  
`cd nom_du_projet`  

3. Créer un dossier d'environnement ".venv"  
`mkdir .venv`  

4. Installer l'environnement du projet ( Etre dans le dossier du projet )  
`pipenv install` 

5. Activer l'environnement du projet  
`pipenv shell`  
---

## Installation ( Python / Django )  

#### **- Installation de Python**  

- Python est installé par defaut sur Linux\Mac  
- Windows suivre le lien suivant : https://www.python.org/downloads/  

#### **- Installation de Django**  

- Créer un environnement virtuel de votre projet ( Voir plus haut )  

- Puis installer Django avec la commande suivante  
`pipenv install django`  

- Verifier qu'il soit bien installé  
`django-admin --version`  
---

## Tutoriel : 1ère partie - requêtes et réponses  

#### **- Création d'un projet Django**  
- Commande de création du projet  
`django-admin startproject nom_du_projet`  

- Cela va créer un répertoire 'nom_du_projet' dans le répertoire courant  
    > nom_du_projet/  
    >>  manage.py  
    >> nom_du_projet/  
    >>>> _ _init_ _.py  
    >>>> settings.py  
    >>>> urls.py  
    >>>> asgi.py  
    >>>> wsgi.py  

#### **- Ces fichiers sont :**  
- Le premier répertoire racine mysite/  
    > Est un contenant pour votre projet. Son nom n’a pas d’importance pour Django ; vous pouvez le renommer comme vous voulez  

- manage.py  
    > Un utilitaire en ligne de commande qui vous permet d’interagir avec ce projet Django de différentes façons. Vous trouverez toutes les informations nécessaires sur manage.py dans django-admin et manage.py  

- Le sous-répertoire mysite/  
    > Correspond au paquet Python effectif de votre projet. C’est le nom du paquet Python que vous devrez utiliser pour importer ce qu’il contient (par ex. mysite.urls)  

- mysite/_ _init_ _.py  
    > Un fichier vide qui indique à Python que ce répertoire doit être considéré comme un paquet. Si vous êtes débutant en Python, lisez les informations sur les paquets (en) dans la documentation officielle de Python  

- mysite/settings.py  
    > Réglages et configuration de ce projet Django. Les réglages de Django vous apprendra tout sur le fonctionnement des réglages  
- mysite/urls.py  

    > Les déclarations des URL de ce projet Django, une sorte de « table des matières » de votre site Django. Vous pouvez en lire plus sur les URL dans Distribution des URL  

- mysite/asgi.py  
    > Un point d’entrée pour les serveurs Web compatibles aSGI pour déployer votre projet. Voir Comment déployer avec ASGI pour plus de détails  

- mysite/wsgi.py  
    > Un point d’entrée pour les serveurs Web compatibles WSGI pour déployer votre projet. Voir Comment déployer avec WSGI pour plus de détails  

#### **- Le serveur de développement**  
- Lancement du serveur ( port par defaut : 8000 )  
`python manage.py runserver`  

- Modification du port si besoin  
`python manage.py runserver 8080`  

- Modification de l'IP si besoin  
`python manage.py runserver 0.0.0.0:8000`  

#### **- Les applications**
- Quelle est la différence entre un projet et une application ?  
    - Une application est une application Web qui fait quelque chose – par exemple un système de blog, une base de données publique ou une petite application de sondage  

    - Un projet est un ensemble de réglages et d’applications pour un site Web particulier. Un projet peut contenir plusieurs applications. Une application peut apparaître dans plusieurs projets  
- Création de l'application  
`django-admin startapp nom_de_application`  

- Cela va créer un répertoire 'nom_de_application' dans le répertoire courant  
    > nom_de_application/  
    >> _ _init_ _.py  
    >> admin.py  
    >> apps.py  
    >> models.py  
    >> tests.py  
    >> views.py  
    >> migrations/  
    >>> _ _init_ _.py  

#### **- Les vues**  
- Les vues se trouve dans le fichier du répertoire de l'application ( views.py )  
    - Ex de fichier qui affiche 'Hello World !'
        ```
        from django.http import HttpResponse  

        def index(request):  
            return HttpResponse("Hello, world !")  
        ```

- Pour créer un URLconf dans le répertoire polls, créez un fichier nommé urls.py. Votre répertoire d’application devrait maintenant ressembler à ceci :  
`mkdir urls.py`  

- Vous devriez avoir ceci  
    > nom_de_application/  
    >> _ _init_ _.py  
    >> admin.py  
    >> apps.py  
    >> models.py  
    >> tests.py  
    >> urls.py
    >> views.py  
    >> migrations/  
    >>> _ _init_ _.py  

- Dans le fichier nom_de_application/urls.py, insérez le code suivant  
    ```
    from django.urls import path

    from . import views

    urlpatterns = [
        path('', views.index, name='index'),
    ]
    ```

- L’étape suivante est de faire pointer la configuration d””URL racine vers le module nom_de_application.urls. Dans nom_du_projet/urls.py, ajoutez une importation django.urls.include et insérez un appel include() dans la liste urlpatterns, ce qui donnera  
    ```
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path('nom_de_application/', include('nom_de_application.urls')),
        path('admin/', admin.site.urls),
    ]
    ```  

- La fonction include() permet de référencer d’autres configurations d’URL. Quand Django rencontre un include(), il tronque le bout d’URL qui correspondait jusque là et passe la chaîne de caractères restante à la configuration d’URL incluse pour continuer le traitement.  

- La fonction path() reçoit quatre paramètres, dont deux sont obligatoires : route et view, et deux facultatifs : kwargs et name. À ce stade, il est intéressant d’examiner le rôle de chacun de ces paramètres.

- Paramètre de path() : route  
    - route est une chaîne contenant un motif d’URL. Lorsqu’il traite une requête, Django commence par le premier motif dans urlpatterns puis continue de parcourir la liste en comparant l’URL reçue avec chaque motif jusqu’à ce qu’il en trouve un qui correspond. Les motifs ne cherchent pas dans les paramètres GET et POST, ni dans le nom de domaine. Par exemple, dans une requête vers https://www.example.com/myapp/, l’URLconf va chercher myapp/. Dans une requête vers https://www.example.com/myapp/?page=3, l’URLconf va aussi chercher myapp/.  

- Paramètre de path() : view  
    - Lorsque Django trouve un motif correspondant, il appelle la fonction de vue spécifiée, avec un objet HttpRequest comme premier paramètre et toutes les valeurs « capturées » par la route sous forme de paramètres nommés. Nous montrerons cela par un exemple un peu plus loin.

- Paramètre de path() : kwargs  
    - Des paramètres nommés arbitraires peuvent être transmis dans un dictionnaire vers la vue cible. Nous n’allons pas exploiter cette fonctionnalité dans ce tutoriel.

- Paramètre de path() : name  
    - Le nommage des URL permet de les référencer de manière non ambiguë depuis d’autres portions de code Django, en particulier depuis les gabarits. Cette fonctionnalité puissante permet d’effectuer des changements globaux dans les modèles d’URL de votre projet en ne modifiant qu’un seul fichier.

## Tutoriels non traités  
- 2ème partie : les modèles et le site d’administration  
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
