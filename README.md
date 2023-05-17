## Memo sur l'installation de Django et utilisation des commandes sur Gnu/Linux
---
---
## Créer un environnement virtuel pour le projet  
1. Créer le dossier du projet  
`mkdir nom_du_projet`  

2. Se rendre dans le dossier du projet  
`cd nom_du_projet`  

3. Créer un dossier d'environnement ".venv"  
`mkdir .venv`  

4. Installer l'environnement pipenv  
`pipenv install` 

5. Activer l'environnement du projet  
`pipenv shell`  
---
---
## Utilisation de django
1. Installer django dans l'environnement virtuel  
`pipenv install django`

2. Créer un projet  
`django-admin startproject nom_du_projet` 

3. Démarrer le serveur  
`python manage.py runserver` 
---
---
## Configuration et utilisation de Django ( Modèle MVT )   
1. Configuration du fichier 'settings.py' du projet ( Dans le sous-dossier du dossier principal )  
    - Structure du fichier settings.py  
        > BASE_DIR : Chemin parent du projet  
        
        > DEBUG = True : Affiche les erreurs du programme ( Ne pas laisser 'True' en production )  
        
        > DATABASES : Gestion de la base de donnée  
        
        > LANGUAGE_CODE : Langage du code (fr-FR pour mettre en francais )  
        
        > INSTALLED_APPS : Applications par defaut et celles créées pour le projet  

2. Les applications
    - ...
