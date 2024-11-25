# HMP - Formation DevSecOps

## Pré-requis :
- Avoir Git [git version 2.33.0.windows.2]
- Avoir un PC sous Windows [W7 / W10]
- Avoir docker
- Python [Python 3.9.7]
- Avoir un compte github
- De préférence, avoir un éditeur de code (ex: VS Code)


1. Install [Docker](https://www.docker.com), [Git](https://git-scm.com/downloads), [VSCode](https://code.visualstudio.com/), 
2. Crée toi un github et génère un Personnal Acces Token (Clique sur ta photo de profile => Settings => Developer settings => Personal access tokens => tokens classic). Coche toute les cases pour donner l'ensemble des droits à ton token (en réalité, c'est fortement déconseillé de faire ça).
Conserve bien ton token, il te servieras par la suite.
3. Crée toi un compte [Dockerhub](https://hub.docker.com/) et génère un token avec les droits READ/WRITE & DELETE (clique sur ta photo de profil => account settings => Personal access tokens)
4. Fork le github [JuiceShop](https://github.com/juice-shop/juice-shop) dans ton github à toi et appel le "juice-shop-copy"
5. Va dans les settings => secrets and variables => actions et ajouter ces deux variables :

DOCKERHUB_TOKEN = [Mettre ton secret dockerhub]
DOCKERHUB_USERNAME = [Mettre ton username dockerhub]

Cliquer sur le bouton vert et faire un copie sur le bouton à droite puis éxécuter sur ton PC la commande git clone ...

6. Toujours sur Github, cliquer sur code (bouton vert), dans HTTPS copier le l'URL (.git) et aller dans votre invite de commande et taper la commande :

```
git clone [URL .git]
```

7. Connecte toi à ton répertoire : "git remote set-url origin https://[USERNAME]:[TOKEN_GITHUB]@github.com/[USERNAME]/juice-shop-copy.git"

8. Ouvre VS Code, va dans Explorer, Workspace et ajoute le dossier juice-shop-copy.
9. Crée le fichier "dockerhub.yml" dans le dossier .github/workflows et ajoute le code suivant:
```
name: Build and push Docker image to dockerhub

on:
  push:
    branches: [ "master" ]
    
jobs:

  build:

    runs-on: ubuntu-latest

    steps:
    - name: Check out the repo
      uses: actions/checkout@v4

    - name: Login to dockerhub
      uses: docker/login-action@v3
      with:
       username: ${{ secrets.DOCKERHUB_USERNAME }}
       password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push to dockerhub
      uses: docker/build-push-action@v5
      with:
       context: 
       push: true
       tags: [USERNAME_DOCKERHUB]/juice-shop-hmp:latest
```

10. Pense à bien modifier la dernière ligne avec ton username dockerhub
11. Dans l'invite de commande (CMD) va dans ton dossier juice-shop-copy et tape les commandes suivante :
```
git add .
git commit -m "first commit"
git push
```
12. Exécute ensuite les commandes suivantes :
```
docker start
docker pull [USERNAME_DOCKERHUB]/juice-shop-hmp
docker run --rm -p 127.0.0.1:3000:3000 [USERNAME_DOCKERHUB]/juice-shop-hmp
```

13. Va dans ton navigateur et écris cette URL : "127.0.0.1:3000". Tu devrais arriver sur le site juice-shop 

Pour stopper le docker, ouvrir un autre invite de commande et tapper :
``` 
docker ps -a
docker stop [ID Docker]
```

![Juice Shop Logo](https://raw.githubusercontent.com/bkimminich/juice-shop/master/frontend/src/assets/public/images/JuiceShop_Logo_400px.png)

________________________________________________________________________
   ![HMP](https://github.com/user-attachments/assets/e7576c9a-c7bd-4150-aba2-9adee745a976)