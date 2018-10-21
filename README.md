voir documentation gloable sur [projet chapeau volobo](https://github.com/prodageo/volobo/blob/master/README.md).
Les [tickets](https://github.com/prodageo/volodb/issues) sont en revanche gérés en local.

# Exploiter la base de données

## Base de données
### Interrogation
 - https://volodb.herokuapp.com/resa : pour récupérer toutes les réservations
 - https://volodb.herokuapp.com/resa/1 : pour récupérer la réservation numéro 1
 ### Persistence
  - https://github.com/typicode/json-server/issues/297 : sous Heroku, le fichier db.json ne peut être stocké ! Il faudrait donc maj le fichier db.json sur github après chaque modif.

## Create a client in command line
 - https://github.com/typicode/jsonplaceholder#how-to : exemples de call REST adapté à json-server
 - https://gist.github.com/subfuzion/08c5d85437d5d4f00e58 : exemples de call cURL REST
 ### consulter une collection (ie resa ou comments)
 - curl -X GET "https://volodb.herokuapp.com/resa"
 ### ajouter un élément dans une collection (ie un comment stocké dans le fichier data.json)
 - curl -X POST -H "Content-Type: application/json" -d "@data.json" "https://volodb.herokuapp.com/comments"
  - ne pas fournir id, il est ajouté automatiquement par le serveur.

## Create a client in PHP (browser client)
 - https://juliendubreuil.fr/blog/developpement/guzzle-des-webservices-facilement/
### Initialisation du projet
 - composer require guzzlehttp/guzzle:~6.0


## Create a client in Expo (mobile client)
 - https://docs.expo.io/versions/latest/react-native/network : comment utiliser le verbe POST ?




## Create a sync manager between json-server and Jekyll
The sync manager regularly request the json-server to retrieve the last unsaved resas to be persisted in the resas collection on Jekyll (hosted on Heroku with github authentication) 

 - [json2yaml en PHP](https://www.sitepoint.com/using-yaml-in-php-projects/)
   - [Example of usage of sf library](https://github.com/webreactor/json2yml/blob/master/src/Utilities.php)
   - [Corresponding compposer.json](https://github.com/webreactor/json2yml/blob/master/composer.json)
 - [iterate on collection in Jekyll](https://simpleit.rocks/jekyll-collections-versus-posts/)
 
```txt 
 └── _resas_
   ├── open-source-in-governments.md
   └── why-every-developer-should-use-emacs.md
```
 
 a collection in _resas_ folder, with the following configuration:

In /_config.yml:

```yml
  collections:
    resas:
      output: true
```



# Deploy `json-server` to `{{ free hosting site }}`

> Instructions how to deploy the full fake REST API [json-server](https://github.com/typicode/json-server) to various free hosting sites. Should only be used in development purpose but can act as a simpler database for smaller applications.

<img src="https://raw.githubusercontent.com/typicode/json-server/master/src/server/public/images/json.png" align="right">

* [**Create your database**](#create-your-database)
* [Deploy to **Heroku**](#deploy-to-heroku)

## Create your database

1 . Clone this repo to anywhere on your computer.

```bash
git clone https://github.com/jesperorb/json-server-heroku.git
```

2 . Change `db.json` to **your own content** according to the [`json-server example`](https://github.com/typicode/json-server#example) and then `commit` your changes to git. 

_this example will create `/posts` route , each resource will have `id`, `title` and `content`. `id` will auto increment!_
```json
{
  "posts":[
    {
      "id" : 0,
      "title": "First post!",
      "content" : "My first content!"
    }
  ]
}
```

3 . Check the JSON conformity
[https://jsonlint.com/](https://jsonlint.com/)

---

## Deploy to **Heroku**

<img align="right" width="100px" height="auto" src="https://cdn.worldvectorlogo.com/logos/heroku.svg" alt="Heroku">

Heroku is a free hosting service for hosting small projects. Easy setup and deploy from the command line via _git_.

###### Pros

* Easy setup
* Free

###### Cons

* App has to sleep a couple of hours every day.
* "Powers down" after 30 mins of inactivity. Starts back up when you visit the site but it takes a few extra seconds. Can maybe be solved with [**Kaffeine**](http://kaffeine.herokuapp.com/)

---

### Install Heroku

1 . [Create your database](#create-your-database)

2 . Create an account on <br/>[https://heroku.com](https://heroku.com)

3 . Install the Heroku CLI on your computer: <br/>[https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

4 . Connect the Heroku CLI to your account by writing the following command in your terminal and follow the instructions on the command line:
```bash
heroku login
```

5 . Then create a remote heroku project, kinda like creating a git repository on GitHub. This will create a project on Heroku with a random name. If you want to name your app you have to supply your own name like `heroku create project-name`:
```bash
heroku create my-cool-project
```

6 . Push your app to __Heroku__ (you will see a wall of code)
```bash
git push heroku master
```

7 . Visit your newly create app by opening it via heroku:
```bash
heroku open
```

8 . For debugging if something went wrong:
```bash
heroku logs --tail
```

---

#### How it works

Heroku will look for a startup-script, this is by default `npm start` so make sure you have that in your `package.json` (assuming your script is called `server.js`):
```json
 "scripts": {
    "start" : "node server.js"
 }
```

You also have to make changes to the port, you can't hardcode a dev-port. But you can reference herokus port. So the code will have the following:
```js
const port = process.env.PORT || 4000;
```

---
