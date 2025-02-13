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
> listitems: !!str '!!python/object/apply:subprocess.Popen [["PowerShell", "-Command", "dir"]]' ; Cette ligne ouvre une nouvelle console de commande powershell et execute la commande "dir" pour affciher l'arborescence de fichier du projet.

8) Dans l'onglet "Upload", sélectionner et upload le fichier "payload.yaml". 

> :mag: Regardez alors votre terminal. Que constatez-vous ?
> On remarque bien que l'arborescence du fichier est afficher : la payload a bien fonctionné

Avec CTRL+C il est possible d'arréter l'exécution de l'application dans votre terminal. 

### III - Investigation SCA

> :mag: Quel pourrait être le niveau de criticité de cette vulnérabilité  ? (LOW/MEDIUM/HIGH/CRITICAL
CRITICAL

> :mag: Quel est le composant vulnérable ?
> PyAML

> :mag: Quel est le nom de la vulnérabilité ? (CVE-XXXX-XXXX)
> CVE-2020-1747

> :mag: Quel est le niveau réel de criticité de cette vulnérabilité ? D'après vous, pourquoi a-t-elle ce niveau ?
>  9.8 , elle permet l'execution de code arbitraire

> :mag: Imaginez une attaque exploitant cette vulnérabilité.
> Il peut par exemple récupérer les secrets contenus dans le fichier de configuration .env

> :mag: A partir de quelle version cette vulnérabilité est-elle corrigée ?
> 5.3.1

> :mag: Qu'est-ce qu'une CWE ?
>Common Weakness Enumeration : c'est une base de donnée qui repertorie des faiblesses déjà connues 

> :mag: Quelle est la CWE liée à cette vulnérabilité ?
> CWE-20

> :mag: Quelle est la différence entre CWE et CVE ?
> La CVE fait référence à un cas spécifique de vulnérabilité
> La CWE est une classification plus larges des faiblesses courantes


### IV - Correction de la vulnérabilité SCA

9) Créer une nouvelle branch dev avec votre pseudo github et aller dessus
```
git branch dev-[pseudo git]
git checkout dev-[pseudo git]
```

10) La vulnérabilité que l'on vient d'exploiter a été corrigée dans la version que vous avez trouvé précédemment.
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
13) Dans l'onglet "Security", regarder la partie "Code Scanning". Vous devriez avoir 5 notifications.

> :mag: Quelle notification concerne le code qui a été ajouté précédemment ?
> La 1ere : Uncontrolled data used in path expression, This path depends on a user-provided value.

> :mag: Quelle est la "Rule ID" de la notification ? Quelle vulnérabilité pourrait être possible au vu du nom de cette Rule ID ?
> py/path-injection : Accessing files using paths constructed from user-controlled data can allow an attacker to access unexpected resources. This can result in sensitive information being revealed or deleted, or an attacker being able to influence behavior by modifying unexpected files.

> :mag: Quelle sont les CWE associées ?
> 22, 23, 36, 73, 99

14) Essayez d'exploiter la vulnérabiltié.

> :mag: En quoi cette faiblesse est une vulnérabilité ?
> voir word avec joseph

### VI - Correction de la vulnérabilité SAST

15) CodeQL vous propose une correction, générer et ajouter cette correction dans le code source.
    
> :mag: Que font ces lignes de codes ?
> Elles s'assurent que l'on reste bien dans le dossier ou se trouvent le fichier, nous empêchant ainsi d'accéder aux autres fichiers. 

16) Faire un git add / git commit / git push de l'application avec la nouvelle version vers la branch dev.

17) Faire un merge de la branch dev vers la branch main.
```
git merge [branch]
```

18) Vérifier que la vulnérabilité n'est plus présente.


