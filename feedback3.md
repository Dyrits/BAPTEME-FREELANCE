# Retour sur l'atelier Triple Triad Deck-Builder

Salut <Apprenant 3> ! Merci pour ton travail et les efforts fournis. Même si cela ne fonctionne pas comme souhaité, je vais te fournir quelques retours et t'aider à comprendre pourquoi.

Je vois, via les commentaires dans ton code, que tu as essayé de t'organiser pour procéder étape par étape. On va faire cela dans l'ordre donc.  

L'étape préliminaire était d'analyser le code déjà existant et de le comprendre. Il ne faut pas hésiter à commenter un code déjà existant pour comprendre ce qu'il fait. Cela permet de ne pas se perdre dans le code et de pouvoir le modifier plus facilement.
Ensuite, il fallait mettre en place ta base de données. Je suppose que tu as réussi à faire cela, étant donné que tu as avancé sur les étapes suivantes.

Il y a deux lignes dans ton `./index.js` qui m'interpellent, à savoir :
```javascript
app.use(mainController.cardPage);
app.use(mainController.notFound);
```
`app.use()` est une méthode qui permet de définir des middlewares. Tu peux en lire plus dans la [documentation](https://expressjs.com/fr/guide/using-middleware.html), mais ne t'inquiète pas si c'est trop compliqué, tu n'as pas besoin de tout comprendre pour l'instant. Bien que cela puisse fonctionner, il n'est pas conventionnel de passer directement les fonctions de ton *controller*. Il est préférable de faire un middleware dédié (notamment pour les erreurs), ou d'utiliser un middleware existant, pour gérer les routes. C'est ce que tu as fait avec `express.Router()`. 

Il faut savoir que lorsqu'une requête est reçu, par exemple à l'URL `http://localhost:1234/deck`, ton serveur va parcourir ton code dans l'ordre. 

```javascript
app.use(router);
app.use(mainController.cardPage); 
app.use(mainController.notFound);
````
Aucune route correspondante ne va être trouvée dans ton router, car tu n'as pas de route avec "/deck". Il va donc passer à la ligne suivante
Le problème ici, c'est que la méthode `mainController.cardPage()` va être exécutée, même si aucune route ne correspond. Hors, celle-ci s'attend à recevoir un paramètre `request.params.id` qui n'existe pas dans ce cas, vu qu'aucune route ne gère cela. C'est pour cela que tu as une erreur, sur ta page `http://localhost:1234/deck`, correspondant à la ligne suivante de ta méthode :

```javascript
response.status(500).send('Oups, nous rencontrons un problème technique');
```

Désolé, c'est un peut-être un peu trop compliqué pour l'instant. On va passer à la suite et on reviendra là-dessus ensemble si tu le souhaites.

Ensuite, il fallait pouvoir afficher le détail d'une carte. Tu réussis bien à obtenir ta carte dans ton *controller*, et la stocker dans la variable `oneCard`. 

Prenons une carte au hasard, comme la carte 10. Voici ce que contient `oneCard`:
```javascript
{
  id: 10,
  name: 'Larva',
  element: null,
  level: 1,
  value_north: 4,
  value_east: 2,
  value_south: 4,
  value_west: 3,
  visual_name: 'Larva.jpg'
}
```
Tu envoies donc, à ta vue accessible via '/card/10' un object `oneCard` avec ces données.

Mais au lieu d'afficher ces données avec `oneCard` dans `/app/views/main/cardDetails.ejs`, je suis surpris de voir que tu fasses une boucle sur `card`, qui n'existe pas dans le contexte et qui n'est pas itérable. C'est pour cela que tu as une erreur. 

Dans `app/controllers/mainController.js`, quand tu renvoies ta vue avec le résultat obtenu, tu fais :

```javascript
if (oneCard){
    response.render('main/cardDetails', {oneCard});
}
```

C'est très, bien, mais si tu veux que cela fonctionne en utilisant le nom de variable`card` dans ta vue, il faut que tu renvoies `oneCard` en tant que `card`.

```javascript
if (oneCard){
    response.render('main/cardDetails', {card: oneCard});
}
```

Ta vue ne comprend que ce que tu lui envoies. 

Ensuite, fais attention aux propriétés de ton objet. Tu as une propriété `visual_name` qui contient le nom du fichier de l'image.
Quand tu affiches l'image, tu utilises :

```ejs
<img src="/visuals/<%= card.visual_name.toLowerCase() %>.jpg" alt="<%= card.name %> illustration">
```

Si l'on revient sur notre exemple, la propriété `visual_name`contient déjà l'extension (.jpg) du fichier.

```javascript
visual_name: 'Larva.jpg'
```

Tu n'as donc pas besoin d'ajouter manuellement celle-ci. Tu aurais pu t'inspirer de ce qui était fait dans `/app/views/cardList.ejs`.

Ne te fais pas de soucis. Une fois que tu auras bien saisi que tes fichiers `.ejs` ne peuvent utiliser que ce que tu leur envoies via ton *controller*, de la façon dont tu les as nommés, tu pourras continuer à avancer. 

```javascript
response.render('file', { name: value });
```

Ici, dans mon fichier `file.ejs`, je peux utiliser `name` pour afficher `value`.

Il y a plein d'autres petits détails à revoir, mais je pense que cela est déjà le plus important pour avancer avant de se focaliser sur la mise en page elle-même.

J'ai bien vu que tu as essayé de commencer les étapes suivantes, mais que cela n'a pas totalement abouti, notamment pour les sessions, que tu implémentes, mais n'utilises pas. On peut voir cela ensemble si tu le souhaites. Dans tous les cas, la correction est disponible, donc prend le temps de bien comprendre le code, et n'hésite pas à poser des questions si tu as des doutes. 

Parfois, il faut juste avoir un peu de recul pour comprendre ce qui ne va pas. Et avec un déclic, tout devient plus clair.


