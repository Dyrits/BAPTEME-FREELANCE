# Retour sur l'atelier Triple Triad Deck-Builder

Salut <Apprenant 1>, voici ton feedback pour l'atelier Triple Trial Deck-Builder !
Globalement, c'est du très bon travail, alors félicitations pour tes efforts et continue comme ça ! Tu as même implémenté l'ensemble des filtres de recherche !

Au niveau fonctionnel, tout fonctionne correctement ! 

J'ai notamment apprécié le fait de n'afficher que le bouton d'ajout que sur la liste des cartes, et uniquement celui de suppression sur la page du deck. Cela dit, il aurait été bien de pouvoir aussi ajouter une carte au deck lorsqu'on la consulte individuellement (ce qui correspond à ta vue `./app/views/card/card-item.ejs`).

Concernant le respect des consignes, la recherche (dans `./app/dataMapper.js`) par valeur manque simplement d'un petit `>` pour être parfaite et transformer ton égalité en `>=`. D'ailleurs, ton approche en utilisant l'interpolation de la direction, dans cette même recherche, pour générer le nom de colonne, était bien trouvée !

Au niveau technique, il y a quelques petits points à améliorer, mais rien de bien méchant.

Je suppose que localement tu n'as pas eu de soucis, vu que tu utilisais probablement ton propre fichier `.env` de variables d'environnement, mais il manquait la variable `SESSION_SECRET` dans ton fichier `.env.example` (qui pourrait être utilisé par d'autres pour aider à configurer leur environnement, ou du moins pour savoir quelles sont les variables d'environnement à définir). Les joies du `.gitignore` !

Ton fichier d'entrée `.index.js` est bien établi. Créer un dossier et un fichier de *middleware* à part est une bonne idée. De manière générale, la structure de ton projet est réfléchie et organisée, notamment dans le dossier `./app/views` qui reflète ce que l'on trouve au sein de tes *controllers*, bien que j'aurais pris une approche différente :
```
./card
    /card-item.ejs
    /card-list.ejs
    /deck.ejs
    /search.ejs
./layout
    ./partials
        /footer.ejs
        /header.ejs
./error
    /404.ejs
    /500.ejs
```
Cela permet de regrouper les vues relatives aux cartes (comme tu as voulu le faire), et de séparer les vues relatives au *layout* et aux erreurs. Cela reste propre à chacun et à sa façon de s'organiser, mais c'est une approche que j'ai pu voir dans d'autres projets.  
Il est bien que tu aies pensé à une vue pour les erreurs 404. Il aurait été intéressant de créer une vue pour les erreurs 500 éventuellement, ou de disposer d'une page d'erreur générique. 

Quelques remarques plus spécifiques sur le code :
Dans `./app/controllers/cardController.js`, tu as une fonction `deckPage()` qui crée inutilement un tableau vide et l'assigne à une variable, plutôt qu'utiliser directement ce que tu as en session en réponse.
La vérification conditionnelle n'est, ici, pas forcément utile, à moins que tu doutes de ton *middleware*. Dans ce cas, tu peux aussi utiliser une syntaxe plus courte, qui est équivalente à ce que tu as fait :

```javascript
res.render('card/deck', { cards: req.session.cards || [] } ) 
```
Si la syntaxe ne t'est pas familière, elle est équivalente à :

```javascript
if (req.session.cards) {
    res.render('card/deck', { cards: req.session.cards } ) 
} else {
    res.render('card/deck', { cards: [] } ) 
}
```
N'hésite pas à venir me voir si tu as des questions sur ce point.

Mis à part ça, je note un petit oubli de `const` ou (`let`) devant plusieurs de tes variables dans `./app/controllers/searchController.js`. N'hésite pas à utiliser le linter pour t'aider à repérer ce type d'erreur.

Si tu as des questions ou des points sur lesquels tu désires revenir, n'hésite pas à venir me voir ! Mais je pense que tu t'en sors très bien, et que tu as bien compris les principes de base de Node.js et Express.js. Continue comme ça !
