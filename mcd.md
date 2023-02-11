# Retour sur Modèle Conceptuel de Données (MCD)

Bonjour <Apprenant>, voici un retour sur le MCD que tu as fourni.

Je ne sais pas si tu es l'auteur de la "Contextualisation" mais je vais me baser sur celle-ci pour te donner mon retour.
Cela dit, celle-ci indique notamment :

> Il peut aussi bien passer une commande sur un produit (un mug)

Cela reste assez flou sur le fait de pouvoir passer une commande sur un produit ou plusieurs produits. On va donc considérer, comme pour la plupart des plateformes de vente, qu'il est possible de passer une commande sur un ou plusieurs produits.

Il y a évidemment plusieurs façons de faire et ton approche n'est pas mauvaise.

On va revenir sur celle-ci, avant de proposer des alternatives.
Ton MCD contient 4 entités : `User`, `Address`, `Order` et `Product`.
En considérant les cardinalités, les relations sont les suivantes :
- Un utilisateur peut avoir 0 ou plusieurs addresses (0, N). Une adresse ne peut appartenir qu'à un seul utilisateur (1, 1).
- Un utilisateur peut avoir 0 ou plusieurs commandes (0, N). Une commande ne peut appartenir qu'à un seul utilisateur (1, 1).
- Une commande peut contenir 1 ou plusieurs produits (1, N). Un produit peut être contenu dans 0 ou plusieurs commandes (0, N).
- Un utilisateur peut aimer un seul produit (1, 1). Un produit peut être aimé par un seul utilisateur (1, 1).

Le problème ici est, avant tout, la dernière relation. En effet, comme il est noté sur ton MCD, la relation est de type "Many to Many". Cela signifie que tes cardinalités doivent être de 0,N et 0,N. Or, tu as mis 1,1 et 1,1. En théorie, tu peux avoir 0 ou plusieurs utilisateurs qui aiment un même produit, et inversement. 
Dans la phase de conception de ta base de données, cette relation sera gérer par une table intermédiaire (`users_products_likes` par exemple) avec les clés étrangères des deux tables.

Autre point à noter, c'est que tu as mis une relation entre `User` et `Order`. C'est correct dans l'idée, mais comment faire le lien entre les commandes et les adresses de livraison ? Si tu passes par l'utilisateur, tu ne sais pas quelle adresse est associée à quelle commande. Il faudrait donc ajouter une relation entre `Order` et `Address`. À toi de jouer pour définir les cardinalités de cette relation !

Libre à toi de laisser le lien entre `User` et `Order` ou non, sachant que tu pourras, dans ce cas, retrouver l'utilisateur à partir de l'adresse de livraison. Cela peut être intéressant de laisser ce lien, pour, par exemple, des commandes en cours, non validées, qui n'ont pas encore d'adresse de livraison, ou en cas de suppression d'une adresse. Cela dépend de comment seront gérées tes données à terme.

L'important est de bien définir tes règles de gestion, et à partir de là, tu peux facilement définir tes entités, leurs attributs et leurs relations.

Aussi, au sein de ton MCD, il est bien de visuellement avoir un aperçu des identifiants de tes entités (clés primaires), en les soulignant par exemple. Cela permet de savoir, au premier coup d'œil, quelles sont les clés primaires de tes entités. Pas mal d'outils permettent de créer des MCD notamment en ligne, et je recommande de rester sur des outils de modélisation, dans un premier temps, avant d'utiliser des outils plus poussés. 

Voici quelques-uns d'entre eux, que j'ai déjà utilisés. Tous ont leurs avantages et inconvénients, et peuvent demander plus ou moins de temps avant d'être confortablement utilisés :
 - [diagrams.net](https://app.diagrams.net/) est gratuit et te permet de faire des MCD, des UML, etc. Il est possible de le faire en ligne, mais il est aussi possible de l'installer sur son ordinateur. Je l'utilise régulièrement, car il est assez flexible, permettant une grande personnalisation et je le trouve très pratique.
 - [Visual Paradigm Online](https://online.visual-paradigm.com/drive/) est un outil très complet, partiellement gratuit et utilisable en ligne. Il est peut-être un peu plus complexe à prendre en main, mais il permet de faire pas mal de choses
 - [Lucid Chart](https://www.lucidchart.com/) est pas mal utilisé en entreprise, car il propose pas mal d'intégrations avec d'autres outils. Il est proposé une version gratuite limitée, mais qui peut suffire pour des MCD simples. Je trouve cependant qu'il manque d'options de personnalisation.
Il existe pas mal d'autres outils (comme [GitMind](https://gitmind.com/)), mais je ne les ai pas tous testés. Libre à toi de les tester et de voir ce qui te convient le mieux.

Bon courage, et n'hésite pas à revenir vers moi si tu as des questions ! Je sais que la modélisation n'est pas toujours évidente et que cela peut être fastidieux, mais c'est un exercice qui vaut le coup d'être fait, et qui permet de bien comprendre les bases de la conception de base de données.
