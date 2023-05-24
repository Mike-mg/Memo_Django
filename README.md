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
#### Installation de Python  
- Python est installé par defaut sur Linux\Mac  
- Windows suivre le lien suivant : https://www.python.org/downloads/  

#### Installation de Django  
- Créer un environnement virtuel de votre projet ( Voir plus haut )  
- Puis installer Django avec la commande suivante  
`pipenv install django`  
- Verifier qu'il soit bien installé  
`django-admin --version`  
---
## Tutoriel : 1ère partie - requêtes et réponses 
### Création d'un projet Django
- Commande de création du projet  
`django-admin startproject nom_du_projet`  

- Cela va créer un répertoire 'nom_du_projet' dans le répertoire courant  
    > nom_du_projet/  
    >>  manage.py  
    >> nom_du_projet/  
    >>>> __init__.py  
    >>>> settings.py  
    >>>> urls.py  
    >>>> asgi.py  
    >>>> wsgi.py  

#### Ces fichiers sont :  
- Le premier répertoire racine mysite/  
    > Est un contenant pour votre projet. Son nom n’a pas d’importance pour Django ; vous pouvez le renommer comme vous voulez  

- manage.py  
    > Un utilitaire en ligne de commande qui vous permet d’interagir avec ce projet Django de différentes façons. Vous trouverez toutes les informations nécessaires sur manage.py dans django-admin et manage.py  

- Le sous-répertoire mysite/  
    > Correspond au paquet Python effectif de votre projet. C’est le nom du paquet Python que vous devrez utiliser pour importer ce qu’il contient (par ex. mysite.urls).

- mysite/__init__.py  
    > Un fichier vide qui indique à Python que ce répertoire doit être considéré comme un paquet. Si vous êtes débutant en Python, lisez les informations sur les paquets (en) dans la documentation officielle de Python  

- mysite/settings.py  
    > Réglages et configuration de ce projet Django. Les réglages de Django vous apprendra tout sur le fonctionnement des réglages  

- mysite/urls.py  
    > Les déclarations des URL de ce projet Django, une sorte de « table des matières » de votre site Django. Vous pouvez en lire plus sur les URL dans Distribution des URL

- mysite/asgi.py  
    > Un point d’entrée pour les serveurs Web compatibles aSGI pour déployer votre projet. Voir Comment déployer avec ASGI pour plus de détails  

- mysite/wsgi.py  
    > Un point d’entrée pour les serveurs Web compatibles WSGI pour déployer votre projet. Voir Comment déployer avec WSGI pour plus de détails  

## Le serveur de développement
- Lancement du serveur ( port par defaut : 8000 )  
`python manage.py runserver`

- Modification du port si besoin  
`python manage.py runserver 8080`  

- Modification de l'IP si besoin  
`python manage.py runserver 0.0.0.0:8000`



 
---  
* 2ème partie : les modèles et le site d’administration  
---  
* 3ème partie : vues et gabarits  
---  
* 4ème partie : formulaires et vues génériques  
---  
* 5ème partie : tests  
---  
* 6ème partie : fichiers statiques  
---  
* 7ème partie : personnalisation du site d’administration  
---  
---  
##  Tutoriel avancés  
* Comment écrire des applications réutilisables
---
---
* Écriture de votre premier correctif pour Django  
---
---
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
