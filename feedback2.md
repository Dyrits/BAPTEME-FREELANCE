# Retour sur l'atelier Triple Triad Deck-Builder

Salut <Apprenant 2> ! Merci pour ton travail. Même s'il est légèrement incomplet, tu as quand même bien travaillé, et j'espère que tu auras appris pas mal de choses en faisant ce projet. 

Je pense que tu as globalement compris le fonctionnement et saisi la plupart des concepts mis en avant par cet atelier, mais voici quelques remarques sur ton travail.

Tu as réussi à configurer les sessions, c'est bien ! Cela dit, ton `secret` est en clair dans ton code. Il est préférable de le mettre dans une variable d'environnement pour les données pouvant être sensible et n'ayant pas vocation à être partagé. Tu peux profiter de la librairie `dotenv` que tu utilises déjà, pour cela.

Un autre point, concernant la sécurité et les données, la requête pour `getOneCard()` dans ton fichier `./app/dataMapper.js` est correcte, mais il est préférable de ne pas interpoler directement tes variables au sein de ta requête.
Plutôt que de faire :
```javascript
const query = `SELECT * FROM "<TABLE>" WHERE "id" = ${id}`;
const results = await database.query(query);
```
Pense à utiliser les paramètres de requêtes, comme ceci :
```javascript
const query = {
  text: `SELECT * FROM "<TABLE>" WHERE "id" = $1`,
  values: [id]
};
const results = await database.query(query);
```
Cela permet de faire des requêtes plus complexes et plus sécurisées et éviter, notamment, de possibles injections SQL. L'article Wikipédia sur les injections SQL (que tu trouveras [ici](https://fr.wikipedia.org/wiki/Injection_SQL)) est plutôt intéressant si ça t'intéresse. Ton code ici n'est pas forcément vulnérable, mais il vaut mieux être attentif à ce genre de choses.

Pour rester sur la recherche, dans `/app/controllers/searchController.js`, tu effectues une recherche en filtrant avec du JavaScript directement. Ne t'inquiète pas, pas de soucis avec ça ! Cela convient pour les petites applications comme celles-ci. Juste un conseil pour le jour où tu travailleras sur des applications plus grosses, n'hésite pas à exploiter au mieux les capacités de ton système de gestion de base de données. Les bases de données sont optimisées pour indexer les données et effectuer des recherches sur des quantités importantes de données. Imagine si tu devais filtrer 100 000 000 de cartes avec du JavaScript, ça ne serait pas très efficace. Il est donc préférable d'envoyer tes requêtes au serveur. Ici, tu fais un premier pas, en filtrant au préalable les données pour avoir uniquement les éléments non `NULL`. Hélas, cela amène un autre problème, tu n'as pas les résultats pour les cartes sans éléments !

Je vois que tu utilises `next()` dans `addDeck()` (`/app/controllers/deckController.js`). Hélas, tu ne sembles pas avoir de route après celle-ci qui pourrait gérer la suite en cas d'erreur. Il aurait fallu implémenter une gestion d'erreur, comme une page avec la fameuse erreur 404. Sinon, la logique pour ton ajout de carte est bien construite, mais tu as oublié de vérifier que ton nombre de cartes dans ton deck ne soit pas déjà de 5. Aussi, pense à être explicite dans le nommage de tes variables. `addDeck()` n'ajout pas un deck mais une carte au deck. `addCard()`, par exemple, aurait été un nom plus approprié. Un nommage plus explicite permet de gagner du temps à la lecture du code.

Enfin, tu as pu faire une page pour afficher ton deck, et c'est très bien. Dommage que tu n'aies pas pu la terminer et implémenter la suppression d'une carte du deck. Je t'invite à consulter la correction pour avoir des détails des différentes implémentations relatives aux recherches (bonus) que n'as pas implémenté dans ton projet.

Si besoin, n'hésite pas à me remonter les problèmes que tu as rencontrés et les points qui t'ont posé un problème afin qu'on puisse éclaircir les choses.

Encore bon travail et bon courage pour la suite.
