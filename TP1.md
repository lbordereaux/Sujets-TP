# HMP - Formation DevSecOps

## Pré-requis :
- Avoir Git 
- Avoir un PC sous Windows [W7 / W10 / W11]
- Python [Python 3.9.7]
- pip (penser à bien cocher la présence de pip durant l'installation de python)
- Avoir un compte github
- De préférence, avoir un éditeur de code (ex: VS Code)

>:pencil2: Quelle est la différence entre Git et Gitlab ou Github ?
>Git : Système de contrôle de version
>Github : Service cloud d'hebergement de repo
>Gitlab : Plateforme DevOps complète

## TP1 - DevOps

### I - Git clone et exécution

1) Faire un fork de ce [projet](https://github.com/HMP-DSO/Formation-DSO) vers votre répository HeadMind.

> :pencil2: Qu'est ce qu'un fork ? A quoi servent-ils ?
> Duplication du repo d'origine par / pour un utilisateur tiers. Ca sert à déconnecter la base de code est les précédents committers 

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
> Pour s'assurer que tout les dev utilisent le même environnement pour un projet. 

### II - Modification du code

7) Créer une nouvelle branch 'dev' et aller dessus
```
git branch dev-[pseudo git]
git checkout dev-[pseudo git]
```


8) Ajouter le code suivant dans le fichier application.py.
```
@app.route('/files')
def files():
    path = f'{Path(__file__).parent}'
    file_path = path + "\\files"
    print(file_path)
    fichier = []
    for item in os.listdir(file_path):
        fichier.append(item)
    return render_template('files.html', files=fichier)


@app.route('/view')
def view_file():
    filename = request.args.get('file')  # Paramètre `file` passé dans l'URL
    base_path = os.path.abspath('./files')  # Répertoire sécurisé
    requested_path = os.path.abspath(os.path.join(base_path, filename))


    try:
        with open(requested_path, 'r') as file:
            content = file.read()  # Lire le contenu du fichier
        return render_template('filecontent.html', filename=filename, content=content)
    except Exception as e:
        abort(500, description=str(e))
```

et décommenter les lignes suivantes dans les fichiers files.html/index.html et upload.html
```
<!-- <li><a href="/files">Files</a></li> -->
```

> :pencil2: Pourquoi avoir créé une nouvelle branche pour faire cette modification ?
> Tester l'implémentation de nouvelles fonctionnalités sans impacter la branche initiale. 
> :pencil2: Expliquer le code ajouté ci-dessus, que fait-il ?
> Permet de faire apparaitre les boutons dans le front, et ainsi accéder à la route file. 


9) Envoyer le code vers la branch dev que vous venez de créer. Attention, il sera demandé d'ajouter votre Personnal Access Token.
```
git add .
git commit -m "first commit dev"
git push
```
> :pencil2: Que fait la commande `git add .` ? Que pourrait-il mal se passer ?
> Ajoute le contenu du repo local dans la zone d'index pour le prochain commmit. Le "." ajoute tout les fichiers, ce qui peut être dangereux, il faut mieux ajouter uniquement les fichiers que l'on souhaite modifier. 

10) Aller dans github et vérifiez que le fichier application.py a bien été modifié.

11) Faire un merge de la branch dev vers la branch main. 
Aller dans git => pull request => créer une pull request => merge pull request => confirm merge

> :pencil2:    Quelle est l'utilité de faire une pull request ?
> C'est un moyen de demander une révision du code par d'autres collaborateur, qui pourront ensuite le merge au dépot si ce dernier est approuvé. 

12) Faites un git pull et vérifier que la nouvelle fonctionnalité a bien été implémentée.

> :pencil2:    Avec ce que l'on a vu, quel pourrait être les différents vecteur d'attaque que pourrait exploiter un un attaquant ? (3 réponses)
> Si un attaquant a accès à un compte qui gère le répo, il peut approuver du code malveillant et directement l'implémenter en bout de chaine : CICD-SEC-1: Insufficient Flow Control Mechanisms
> Si un attaquant trouve une faille dans l'une des dépendances présentes dans le requirement.txt, il pourra potentiellement l'exploit : CICD-SEC-3: Dependency Chain Abuse

# Cheatsheet :
Se connecter à son repo:
```
git remote set-url origin https://[USERNAME]:[TOKEN_GITHUB]@github.com/[USERNAME]/Formation-DSO.git
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
