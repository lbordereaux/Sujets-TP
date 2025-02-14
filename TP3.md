# HMP - Formation DevSecOps

## Pré-requis :
- Avoir Git [git version 2.33.0.windows.2]
- Avoir un PC sous Windows [W7 / W10]
- Docker (vérifier que vous avez accès à docker en ligne de commande ET à l'interface graphique)
- Python [Python 3.9.7]
- Avoir un compte github
- De préférence, avoir un éditeur de code (ex: VS Code)


1. Installer [Docker](https://www.docker.com), [Git](https://git-scm.com/downloads), [VSCode](https://code.visualstudio.com/).

> :package: Quelle est la différence entre Docker et une machine virtuelle ?
> Un container est un service distinct sur un même hote qui partage le même noyau
> Une VM est une instance qui est isolée du noyau 

2. Crée toi un github et génère un Personnal Acces Token (Clique sur ta photo de profile => Settings => Developer settings => Personal access tokens => tokens classic). Coche toute les cases pour donner l'ensemble des droits à ton token (en réalité, c'est fortement déconseillé de faire ça).
Conserve bien ton token, il te servieras par la suite.
3. Crée toi un compte [Dockerhub](https://hub.docker.com/) et génère un token avec les droits READ/WRITE & DELETE (clique sur ta photo de profil => account settings => Personal access tokens)

> ✏️ Qu'est ce que Dockerhub ? De quoi faut-il se méfier ?
> A la manière du github qui est une plateforme d'hébergement de code source, dockerhub est une plateforme d'hebergement d'images docker. Il faut se méfier des repos publiques qui contiennent des images de containers potentiellement malveillants 

4. Fork le github [JuiceShop](https://github.com/juice-shop/juice-shop) dans ton github à toi et appel le "juice-shop-copy"

> ✏️ Quelle image docker est utilisée par juice-shop ? Est-elle sécurisée ?
> L'image est build depuis le dockerfile, aucune idée.

5. Va dans les settings => secrets and variables => actions et ajouter ces deux variables :

DOCKERHUB_TOKEN = [Mettre ton secret dockerhub]
DOCKERHUB_USERNAME = [Mettre ton username dockerhub]

6. Activer les alertes dependabot dans settings => Code security and analysis => Dependabot alerts.

7. Toujours sur Github, cliquer sur code (bouton vert), dans HTTPS copier le l'URL (.git) et aller dans votre invite de commande et taper la commande :

```
git clone [URL .git]
```

8. Connecte toi à ton répertoire : "git remote set-url origin https://[USERNAME]:[TOKEN_GITHUB]@github.com/[USERNAME]/juice-shop-copy.git"

9. Ouvre VS Code, va dans Explorer, Workspace et ajoute le dossier juice-shop-copy.

10. Crée le fichier "dockerhub.yml" dans le dossier .github/workflows et ajoute le code suivant:
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
       username: ""
       password: ""

    - name: Build and push to dockerhub
      uses: docker/build-push-action@v5
      with:
       context: 
       push: true
       tags: ""/juice-shop-hmp:latest
```

> ✏️ Quelle image est utilisée pour l'éxécution de ce worflow ? Le système d'exploitation est-il le même que pour l'application juice-shop ?
> ubuntu

> ✏️ Quelles sont les trois étapes décrites dans ce workflow ?
> Vérifie si le répo existe, se login sur dockerhub, build et push l'image sur dockerhub


11. Compléte le fichier yaml en renseignant les secrets en se basant sur les variables d'environnement définies plus tôt. 

12. Commit tes changements et pousse les sur github.

13. Sur Github, configure CodeQL. Dans Actions => New Workflow => chercher "CodeQL" => Cliquer sur "Configure" => "Commit changes ...".  

> ✏️ De quel type d'outil est CodeQL ? (SCA/SAST ou DAST) SAST

14. Quelle est la commande qui permet de récupérer le conteneur qui a ainsi été construit depuis Dockerhub ? Exécuter alors cette commande puis la commande suivante

> docker image pull lbordereaux/juice-shop-hmp

```
docker run --rm -p 127.0.0.1:3000:3000 [USERNAME_DOCKERHUB]/juice-shop-hmp
```

15. Va dans ton navigateur et écris cette URL : "127.0.0.1:3000". Tu devrais arriver sur le site juice-shop 

Pour stopper le docker, ouvrir un autre invite de commande et tapper :
``` 
docker ps -a
docker stop [ID Docker]
```
________________________________________________________________________
   ![HMP](https://github.com/user-attachments/assets/e7576c9a-c7bd-4150-aba2-9adee745a976)
