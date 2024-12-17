# HMP - Formation DevSecOps

## Pré-requis :
- Avoir Git
- Avoir un PC sous Windows [W7 / W10 / W11]
- Python [Python 3.9.7]
- Avoir un compte github
- De préférence, avoir un éditeur de code (ex: VS Code)

## TP2 - DevSecOps

### I - Préparation de l'environnement
1) Pour ce TP on prendra comme base ce qui a été fait durant le TP1.

2) Vérifier que vous avez bien accès à l'onglet "files" sur le site. Différents fichiers devraient s'afficher.

3) Activer les alertes dependabot dans settings => Code security and analysis => Dependabot alerts.
4) Activer CodeQL. Sur Github, aller dans Actions => New Workflow => chercher "CodeQL" => Cliquer sur "Configure" => "Commit changes ...".

### II - Découverte et test de l'application

5) Ouvrez le fichier requirements.txt et vérifier que la version de PyYAML est bien la 5.3 "PyYAML==5.3". (Si besoin taper: pip install pyyaml==5.3)

6) Avec python, exécutez :
```
pip install -r requirements.txt
python application.py
```

7) Allez sur le navigateur et dans la barre de navigation tapez "127.0.0.1:5000". Vous devriez tomber sur un site web d'analyse de fichier de configuration 

> :mag: Inspectez le fichier payload.yml. Quelle ligne est intéressante dans ce fichier ? Que fait cette ligne ?

8) Dans l'onglet "Upload", sélectionner et upload le fichier "payload.yaml". 

> :mag: Regardez alors votre terminal. Que constatez-vous ?

Avec CTRL+C il est possible d'arréter l'exécution de l'application dans votre terminal. 

### III - Investigation SCA

> :mag: Quel pourrait être le niveau de criticité de cette vulnérabilité  ? (LOW/MEDIUM/HIGH/CRITICAL)

> :mag: Quel est le composant vulnérable ?

> :mag: Quel est le nom de la vulnérabilité ? (CVE-XXXX-XXXX)

> :mag: Quel est le niveau réel de criticité de cette vulnérabilité ? D'après vous, pourquoi a-t-elle ce niveau ?

> :mag: Imaginez une attaque exploitant cette vulnérabilité.

> :mag: A partir de quelle version cette vulnérabilité est-elle corrigée ?

> :mag: Qu'est-ce qu'une CWE ?

> :mag: Quelle est la CWE liée à cette vulnérabilité ?

> :mag: Quelle est la différence entre CWE et CVE ?


### IV - Correction de la vulnérabilité SCA

9) Créer une nouvelle branch dev avec votre pseudo github et aller dessus
```
git branch dev-[pseudo git]
git checkout dev-[pseudo git]
```

10) La vulnérabilité que l'on vient d'exploiter a été corrigée dans la version que vous avez troué précédemment.
Tapez les commandes suivantes pour mettre à jour la dépendance et mettre à jour le fichier requirements.txt. 

```
pip install pyyaml==[version (ex: 1.2.3)]
python update_requirements.py
```

11) Vérifier que le fichier "requirements.txt" s'est bien mis à jour et que la version de PyYAML est bien celle installée. On va maintenant vérifier que la vulnérabilité est bien corrigée. Relancer l'application.
```
python application.py
```

12) Aller dans votre navigateur et dans la barre de navigation taper "127.0.0.1:5000". Essayer à nouveau d'envoyer le fichier YAML et regarder le terminal. Une erreur devrait apparaître. Il n'est donc plus possible d'exploiter la vulnérabilité.

### V - Investigation SAST

> :mag: Comment exploiter cette faiblesse ?

> :mag: En quoi cette faiblesse est une vulnérabilité ?

### VI - Correction de la vulnérabilité SAST

Afin de corriger la vulnérabilité, il suffit d'ajouter les lignes suivantes.
```
#Vérification de sécurité pour empêcher la sortie du répertoire
if not requested_path.startswith(base_path) or not os.path.isfile(requested_path):
   abort(403)
```
> :mag: Expliquer les lignes ci-dessus.


13) Faire un git add / git commit / git push de l'application avec la nouvelle version de PyYAML vers la branch dev.

14) Faire un merge de la branch dev vers la branch main
```
git merge [branch]
```

____________________________________________________________________________________________________________
   ![HMP](https://github.com/user-attachments/assets/e7576c9a-c7bd-4150-aba2-9adee745a976)
