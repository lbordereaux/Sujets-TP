# HMP - Formation DevSecOps

## Pré-requis :
- Avoir Git 
- Avoir un PC sous Windows [W7 / W10 / W11]
- Python [Python 3.9.7]
- Avoir un compte github
- De préférence, avoir un éditeur de code (ex: VS Code)

>:pencil2: Quelle est la différence entre Git et Gitlab ou Github ?

## TP1 - DevOps

### I - Git clone et exécution

1) Faire un fork de ce [projet](https://github.com/HMP-DSO/Formation-DSO) vers votre répository Head Mind.

> :pencil2: Qu'est ce qu'un fork ? A quoi servent-ils ?

2) Va dans ton github et génère un [Personnal Acces Token](https://github.com/settings/tokens). Coche toute les cases pour donner l'ensemble des droits à ton token (en réalité, c'est fortement déconseillé de faire ça). Conserve bien ton token, il te servieras par la suite.

3) Cliquer sur code (bouton vert), dans HTTPS copier l'URL (.git) et aller dans votre invite de commande et taper la commande :
```
git clone [URL .git]
```

5) Ouvrez le fichier requirements.txt et vérifier que la version de PyYAML est bien la 5.3 "PyYAML==5.3". (Si besoin taper: pip install pyyaml==5.3)

6) Avec python, exécutez :
```
pip install -r requirements.txt
python application.py
```

> :pencil2: A quoi le fichier `requirements.txt` sert-il ? D'où proviennent les dépendances ?

### II - Modification du code

7) Créer une nouvelle branch 'dev' et aller dessus
```
git branch dev
git checkout dev
```


8) Tapez les commandes suivantes pour mettre à jour la dépendance et mettre à jour le fichier requirements.txt. 
```
pip install pyyaml==5.3.1
python update_requirements.py
```
> :pencil2: Pourquoi avoir créé une nouvelle branche pour faire cette modification ?


9) Envoyer le code vers la branch dev que vous venez de créer. Attention, il sera demandé d'ajouter votre Personnal Access Token.
```
git add .
git commit -m "first commit dev"
git push --set-upstream origin dev
```
> :pencil2: Que fait la commande `git add .` ? Que pourrait-il mal se passer ?

10) Aller dans github et vérifiez que la version de PyYAML dans la branch dev est bien en 5.3.1 alors que dans la branch main elle n'est encore que en 5.3.

11) Faire un merge de la branch dev vers la branch main. 
Aller dans git => pull request => créer une pull request => merge pull request => confirm merge

> :pencil2:    Quelle est l'utilité de faire une pull request ?

12) Vérifier alors que les deux branch ont bien la même valeur pour PyYAML.

> :pencil2:    Avec ce que l'on a vu, quel pourrait être les différents vecteur d'attaque que pourrait exploiter un un attaquant ? (3 réponses)

# Cheatsheet :
Se connecter à son repo:
```
git remote set-url origin https://[USERNAME]:[TOKEN_GITHUB]@github.com/[USERNAME]/HMP-DSO/Formation-DSO.git"
```
Supprimer tt les containers:
```
docker ps -aq | ForEach-Object { docker rm -f $_ }
```
Lister branch:
```
git branch
``` 
Changer de branch:
```
git checkout [branch]
```
   
____________________________________________________________________________________________________________
   ![HMP](https://github.com/user-attachments/assets/e7576c9a-c7bd-4150-aba2-9adee745a976)


