# Fetch API

## Les promesses

Alors, pour expliquer `fetch`, en JavaScript, il faut d'abord que tu comprennes le concept de `Promise` (promesse). Je suppose que tu as déjà plus ou moins compris le concept vu que ton code contient des opérations asynchrones, mais je vais quand même rappeler brièvement ce que c'est.

Libre à toi de lire cette partie si tu penses en avoir besoin...

<details>
<summary>Les promesses</summary>
Une `Promise` est un objet qui représente une valeur qui sera disponible dans le futur. C'est un peu comme une promesse dans la vie réelle : tu sais que tu vas faire quelque chose, mais tu ne sais pas quand tu auras terminé, donc tu fais une promesse. Tu peux généralement toujours faire quelque chose d'autre en attendant que ta promesse soit tenue. C'est exactement la même chose avec une `Promise` en JavaScript : tu peux faire d'autres choses en attendant que la promesse soit résolue ou rejetée.

Vu que JavaScript n'a qu'un seul *thread*, il ne peut pas faire deux choses en même temps. C'est pour ça qu'il délègue certaines tâches à ton navigateur par exemple, et que tu peux te permettre d'attendre sans avoir à attendre.

Au sein de ce projet, tu cherches une carte par niveau, et tu utilises `await` dans une fonction asynchrone (`sync`) :
```javascript
const searchedResults = await dataMapper.searchByLevel(searchedLevel);
```
Ton code, au sein de ce scope, va attendre que la promesse soit résolue avant de continuer. Ce n'est pas pour autant que ton serveur va attendre que la promesse soit résolue avant de continuer à traiter les autres requêtes. C'est comme si tu sortais de ta fonction pour continuer à faire autre chose, et quand le résultat de la promesse est disponible, tu reviens dans ta fonction et tu continues à exécuter le code.

Il faut laisser à la base de données le temps de trouver tes données. Si tu n'attends pas (avec `await`) le résultat d'une fonction asynchrone, tu auras simplement une promesse non résolue, sans valeur, et tu ne pourras pas faire grand-chose avec.

Une autre façon de faire et d'utiliser `.then()`. Avec ton code, cela donnerait ça :
```javascript
dataMapper.searchByLevel(searchedLevel).then((searchedResults) => {
    // Fais quelque chose avec searchedResults~
});
```
En fait, le `.then()`, ce n'est pas "fais ceci, puis cela", mais plutôt, "attend que ceci soit résolu, pour faire cela". Tu donnes à la promesse, en avance, une fonction qui sera exécutée quand la promesse sera résolue.

Dans les deux cas, tu peux gérer les erreurs, avec le `catch` d'un `try {} catch {}` ou avec `.catch()` à la suite de ta `Promise`.
```javascript
dataMapper.searchByLevel(searchedLevel).then((searchedResults) => {
    // Fais quelque chose avec searchedResults~
}).catch((error) => {
    // Gère l'erreur
});
```
Tu as déjà fait cela dans la plupart de tes fonctions asynchrones, avec le `try {} catch {}` dans ton projet, donc je vais en venir au sujet principal de ces explications : `fetch()`.
</details>

## Fetch

Fetch c'est avant tout une interface te permettant d'aller chercher des ressources. Tu veux aller chercher une image, une vidéo, un fichier, une page web, une API, etc. Tu peux faire tout cela avec la méthode `fetch()`. C'est une fonction qui prend en paramètre l'URL de la ressource que tu veux aller chercher, et qui retourne une promesse qui sera remplie avec la réponse quand elle sera disponible. Donc `fetch()`, ça te retourne une `Promise` qui sera remplie avec une `Response` quand elle sera disponible. Pour en savoir plus sur ce qu'est une `Response`, je te conseille de lire la [page dédiée sur MDN](https://developer.mozilla.org/fr/docs/Web/API/Response).
Cela dit, tu n'as pas besoin de savoir tout ça pour utiliser `fetch()`.

Avant `fectch()`, on utilisait `XMLHttpRequest` pour faire des requêtes HTTP. C'est une API assez complexe qui est un peu plus bas niveau que Fetch. On avait aussi `jQuery.ajax()`, avec jQuery (qui est une librairie JavaScript), qui était un peu un `fetch()` avant l'heure, bien que quelque peu différent.

Pour faire une requête HTTP avec `fetch()`, tu peux faire quelque chose comme ça :
```javascript
fetch(resource)
fetch(resource, options)
```
C'est aussi simple que ça (malgré la multitude d'options disponible). Comme je l'ai dit, `fetch()` est asynchrone, donc tu dois utiliser `await` ou `.then()` pour récupérer la réponse.

Avec Node.js, et Express, tu as probablement pu voir les différentes méthodes HTTP, comme `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, etc. Tu peux utiliser `fetch()` pour faire des requêtes de n'importe quelle méthode HTTP. Par défaut, `fetch()` utilise la méthode `GET`, mais tu peux changer ça en utilisant l'option `method` :
```javascript
fetch(resource, {
    method: 'POST'
});
```
Il y a tout un tas d'options disponibles, comme `headers`, `body`, `mode`, `credentials`, `cache`, `redirect`, `referrer`, `referrerPolicy`, `integrity`, `keepalive`, etc. Je ne vais pas tout lister ici, mais tu peux aller voir la [page dédiée sur MDN](https://developer.mozilla.org/fr/docs/Web/API/fetch).


### Récupérer des ressources

Si l'on considère que tu travailles localement, ton serveur est toujours sur le port 1234, ton URL pour ta page d'accueil sera donc `http://localhost:1234`.
Si tu utilises `fetch()` avec cette URL au sein d'une fonction asynchrone, quelle sera le résultat selon toi ? 

```javascript
const response = await fetch('http://localhost:1234');
```
Ici, c'est ton routeur qui va intercepter la requête.

```javascript
router.get('/', mainController.homePage);
````

Ton *controller* est prévu pour renvoyer une page HTML, donc c'est cela que nous allons recevoir. Ta variable`response`contiendra dans son header `Content-Type` la valeur `text/html`. Si tu veux voir le contenu de ta réponse, tu peux utiliser la méthode `text()` de l'objet `Response` :

```javascript
const response = await fetch('http://localhost:1234');
const html = await response.text();
```

La variable `html` contiendra ainsi le contenu de ta page HTML.

Attention, n'oublie pas que tu ne peux pas utiliser await dans une fonction synchrone. Tu peux utiliser `.then()` à la place au besoin.

Je t'invite à taper `node` dans ton terminal et essayer de faire des requêtes HTTP avec `fetch()`.

```javascript
fetch('http://localhost:1234')
  .then((response) => {
    console.log(response);
});
```

Comme ça, tu auras l'aperçu de ce que tu reçois en `Response`. Tu peux changer l'URL pour tester d'autres routes de ton projet ou avec d'autres ressources en ligne. 
En fonction du type de contenu, essaye les méthodes `text()`, `json()`, `blob()`, `formData()`, `arrayBuffer()`, etc. pour récupérer le contenu de la réponse.

```javascript
fetch('http://localhost:1234')
  .then((response) => response.text())
  .then((html) => { 
    console.log(html); 
  });
```

### Utiliser des APIs publiques

Maintenant, tu es prêt à utiliser des APIs publiques pour récupérer leur contenu. Essaye https://swapi.dev/ si tu aimes Star Wars par exemple, qui est une API REST et fournit donc des données au format JSON.

```javascript
fetch('https://swapi.dev/api/people/3')
  .then((response) => response.json())
  .then((json) => {
    console.log(json);
  });
```
Le résultat ? Un objet JavaScript avec les données de R2D2 qui correspond à l'ID 3 dans l'API. 
<details>
<summary>Réponse obtenue~</summary>

```javascript
 {
  name: 'R2-D2',
  height: '96',
  mass: '32',
  hair_color: 'n/a',
  skin_color: 'white, blue',
  eye_color: 'red',
  birth_year: '33BBY',
  gender: 'n/a',
  homeworld: 'https://swapi.dev/api/planets/8/',
  films: [
    'https://swapi.dev/api/films/1/',
    'https://swapi.dev/api/films/2/',
    'https://swapi.dev/api/films/3/',
    'https://swapi.dev/api/films/4/',
    'https://swapi.dev/api/films/5/',
    'https://swapi.dev/api/films/6/'
  ],
  species: [ 'https://swapi.dev/api/species/2/' ],
  vehicles: [],
  starships: [],
  created: '2014-12-10T15:11:50.376000Z',
  edited: '2014-12-20T21:17:50.311000Z',
  url: 'https://swapi.dev/api/people/3/'
}
```
</details>

Ce qui est bien avec tout ça, c'est que des milliers d'API en ligne peuvent être utilisées avec `fetch()` pour récupérer des données, et tu peux même en créer toi-même. Tu pourras ensuite utiliser ces données pour les afficher dans ton navigateur, ou les utiliser pour créer des applications plus complexes.

Par exemple, tu pourrais créer des routes dédiées afin de renvoyer les données de ton projet au format JSON. Tu pourrais ensuite utiliser `fetch()` pour récupérer ces données et les afficher dans ton navigateur, sans avoir à utiliser de moteur de *template* (ejs).


### Envoyer des données au format JSON

Vu que tu n'utilises que des méthodes `GET` pour l'instant, au sein de ton projet, tu n'as pas besoin de spécifier d'options pour `fetch()`. Mais je te donne quand même un cas d'utilisation plus avancé, mais standard, avec `POST`. Ne te prends pas trop la tête là-dessus pour le moment, ça viendra en temps et en heure.

<details>
<summary>Cas d'utilisation courant</summary>

Le plus courant, en JavaScript, et avec les API REST, c'est d'utiliser `fetch()` avec du JSON, pour envoyer des données dans ce format, à ton serveur par exemple. Pour cela, tu dois utiliser l'option `headers` pour spécifier que tu envoies du JSON et l'option `body` pour spécifier le contenu de ton JSON.

Imagine que je veuille t'envoyer les données d'une carte, à ton serveur, pour qu'il puisse l'enregistrer dans ta session.
Il te suffit d'une route, et d'un *controller* dédié à cela.

Si l'on considère que tu travailles localement, ton serveur est toujours sur le port 1234, et que tu as une route `/cards`, ton URL sera donc `http://localhost:1234/cards`. C'est cette URL que tu dois passer en paramètre à `fetch()`.

```javascript
try {
    const response = await fetch("http://localhost:3000/cards", {
        method: 'POST',
        headers: {
            "Content-Type": "application/json"
        },

        body: JSON.stringify({
            card: {
              name: "Bogomile",
              values: { north: 1, east: 4, south: 1, west: 5 }, 
              element: null,
              level: 1,
              visual: "Bogomile.jpg"
            }
        })
    });
    if (response.ok) {
      const data = await response.json();
    } else {
      throw new Error("Une erreur s'est produite...");
    }
} catch (error) {
    console.error(error);
}
```

Mais c'est trop compliqué tout ça ! Non, ne t'inquiète pas, on va voir ça étape par étape.

D'abord, on passe notre URL à `fetch()`, et on lui passe en paramètre un objet qui contient les options de notre requête. 

On peut passer des options, ou pas. Si on ne passe pas d'options, `fetch()` utilisera la méthode `GET` par défaut. Ici, on veut envoyer une ressource et donc utiliser la méthode `POST`. On passe ainsi l'option `method` à `POST`. 

On veut aussi envoyer du JSON, donc on passe l'option `headers` avec le header `Content-Type` à `application/json`. 

Et enfin, on passe l'option `body` avec le contenu de notre JSON. On utilise `JSON.stringify()` pour transformer notre objet JavaScript en chaîne de caractères, qui sera envoyée au serveur. On utilise `await` pour attendre que la promesse soit remplie, et on récupère la réponse avec `response`.   

La `response` sera celle de ton serveur, qui te dira si tout s'est bien passé ou non.

Il faut savoir que si tu as une erreur 4xx ou 5xx, la promesse sera quand même résolue, mais tu peux vérifier si la réponse est bonne avec `response.ok` qui contient `true` pour toutes les réponses 2xx.

Si la réponse est bonne, on peut récupérer les données avec `response.json()`, qui renvoie aussi une promesse. On utilise `await` pour attendre que la promesse soit remplie, et on récupère les données avec `data`. `.json()`est une méthode de `Response` permettant d'avoir une réponse sous la forme d'objet JavaScript.

Mais comment je sais que je vais recevoir du JSON en réponse ? Et bien, c'est à toi de le spécifier dans ton *controller*.
Dans ce cas, il te faudrait une route `POST` et un *controller* dédié à cela.

```javascript
router.post("/cards", controller.addCard);
```
Au sein de ton *controller*, tu peux récupérer les données de la carte avec `req.body.card`. On va d'ailleurs réutiliser la même logique que lorsque tu récupères une carte depuis ta base de données, sauf qu'elle sera, cette fois, envoyée par ton client directement avec `fetch()` !

```javascript
const addCard = async (req, res) => {
  try {
    const card = req.body.card;
    if(card){
      // On vérifie que la carte n'est pas déjà dans le deck:
      const findCardInDeck = req.session.cards.find(card => card.id === cardId);
      if(!findCardInDeck && req.session.cards.length < 5){
        // On ajoute la carte au deck:
        req.session.cards.push(card);
      }
      // On renvoie le deck au client, avec la nouvelle carte incluse si ajoutée, et un code 201:
      res.status(201).json(req.session.cards);
    }
    else {
      // On renvoie une erreur si aucune carte n'a été reçue.
      throw new Error("Aucune carte reçue...");
    }
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};
```

Attention, si tu souhaites utiliser `req.body.card` avec le bon format, il peut être nécessaire de parser le corps de la requête avec `body-parser` (ou `express.json()`).
Il est possible de configurer le middleware au sein de ton `index.js`. Je te recommande de lire la documentation d'Express pour plus d'informations.

Ici, on renvoie le deck complet en JSON, mais tu peux très bien envoyer ce que tu veux, comme un message de succès ou un message d'erreur (comme nous le faisons ici si aucune carte n'a été reçue par exemple).
</details>

Pour aller plus loin tu peux utiliser une librairie qui te simplifie la vie, comme `axios` par exemple. C'est une librairie qui est basée sur `fetch()`, mais qui te simplifie pas mal les choses, et qui te permet de faire des requêtes HTTP plus facilement. Mais je te laisse avec `fetch()`pour le moment.

Tout cela est peut-être un peu compliqué, mais je reste disponible pour expliquer ça plus en détail si tu as des questions. N'hésite pas à lire ces explications plusieurs fois au besoin et prends ton temps.
