# Commencer un projet symfony  

made by @kevinwolff38 & @lmichallon

### I - Installer Composer sur votre pc  *(si nécessaire)*

```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

```shell  
sudo mkdir -p /usr/local/bin  
```

```shell  
sudo composer.phar /usr/local/bin/composer  
```

### II - Installer Symfony CLI sur votre pc  *(si nécessaire)*

```shell  
wget https://get.symfony.com/cli/installer -0 - | bash  
```

```shell  
sudo mv /home/*username*/.symfony/bin/symfony /usr/local/bin/symfony  
```

##### *TCHECK : si vous avez déjà installer Symfony et Composer ou souhaitez vérifier l'installation*

```shell  
symfony -v 
composer --version  
```

*Si ces commandes vous indiques la version de Symfony et Composer alors l'installation est bonne*

### III - Paramétrage du projet  

#### A - Initialiser un projet symfony  

*Attention d'initialiser votre projet où vous le souhaitez*  

```shell  
symfony new nomduprojet --full  
```

Maintenant rendez-vous dans votre projet

#### B - Modifier la connexion à la base de données

Copier ***.env*** dans le même endroit et nomer le ***.env.local***

Rendez-vous dans ***.env.local*** pour changer la connexion à la base de données :  

```shell  
DATABASE_URL=mysql://*nom*:*mot-de-pass*@127.0.0.1:*port*/db_name?serverVersion=5.7  
```

#### C - Installer npm et webpack encore  

```shell  
composer require symfony/webpack-encore-bundle  
```

```shell  
npm install  
```

#### D - Installer sass-loader et node-sass  

```shell  
 npm install sass-loader@^8.0.0 node-sass --save-dev
```

Rendez-vous dans ***webpack.config.js*** et enlever le commentaire pour **.enableSassLoader()**  

#### E - Mise en place fichier Sass et Javascript  

- rendez-vous dans le dossiers ***assets*** et supprimer le dossier ***css*** et son fichier ***app.css***
  
- dans le dossier ***assets*** créer un dossier ***scss***. Puis dans le dossier ***scss*** créer un fichier ***app.scss***  
  
- dans le dossier ***assets*** créer un dossier ***js***. Puis dans le dossier ***js*** créer un fichier ***app.js*** 
  
- ouvrez le fichier ***app.js*** pour y ajouter :  

  ```  import '../sass/app.scss'; ```  

- rendez-vous dans le fichier ***webpack.config.js***, modifier le chemin de ***.addEntry()***

  ```shell
  .addEntry('app', './assets/js/app.js')
  ```

##### *TCHECK : pour vérifier que la mise en place est ok lancer la commande*

```shell
npm run watch
```

*Si un fichier **app.js** et **app.css** apparaissent dans **public/build** alors la mise en place est bonne. Votre fichier **assets/js/app.js** importe **assets/sass/app.scss** correctement et le tout est compilé dans  **public/build** **app.js** et **app.css***

### IV - Créer sa première page html

#### A - Créer un controller

```shell  
php bin/console make:controller  
```
Cette commande vous demande le nom que vous voulez donner à votre controller, par exemple "Home" crééra :  
- un fichier **HomeController.php** dans le dossier ***src/Controller***  
- un dossier **home** dans le dossier ***templates***, contenant un fichier **index.html.twig**
  
La route vers ***home/index.html.twig*** est déjà créée dans le **HomeController.php**

#### B - Lier vos fichier Sass et Javascript compilés

Rendez-vous dans votre ***base.html.twig***

- pour lier le fichier Sass compilé en css dans ***public/build/app.css***

  ```shell
   {% block stylesheets %}{{ encore_entry_link_tags('app') }}{% endblock %}
  ```

- pour lier le fichier Javascript compilé dans ***public/build/app.js***

  ```shell
  {% block javascripts %}{{ encore_entry_script_tags('app') }}{% endblock %}
  ```

##### *TCHECK : pour vérifier que tout est correctement lié*  

``` shell  
symfony serve  
```

*Vous venez de lancer le serveur de votre projet, cliquez sur le lien. Vous arriverez sur la page d'accueil de votre projet, dans l'url ajouter la route vers votre page **index.html.twig** (voir le **src/controller/nomDuController.php**). Si tout c'est bien passé vous constaterez que le style et Javascript fonctionnent.*
