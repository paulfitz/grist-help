# Règles d'accès

!!! avertissement "Les règles d'accès sont une fonctionnalité expérimentale" La conception des règles d'accès est susceptible d'évoluer, et parfois cela nécessitera des mises à jour des documents qui les utilisent. Nous n'apporterons pas de tels changements à la légère.

Chaque document Grist peut être [partagé avec d'autres](sharing.md) à l'aide de l'option `Manage Users` dans le menu de partage (<span class="grist-icon" style="--icon: var(--icon-Share)"></span> ). Les utilisateurs peuvent être invités en tant que lecteurs, éditeurs ou propriétaires (voir [Partage d'un document](sharing.md) pour un rappel sur ces rôles), ou un document peut être [partagé publiquement](sharing.md#public-access-and-link-sharing) avec des autorisations d'affichage ou de modification. Parfois, vous avez besoin de plus de nuances pour savoir qui peut voir ou modifier des parties individuelles d'un document. Les règles d'accès nous donnent ce pouvoir.

Seuls les propriétaires d'un document peuvent modifier ses règles d'accès. Lorsqu'un document est chargé, les propriétaires voient un outil appelé <span class="app-menu-item"><span class="grist-icon" style="--icon: var(--icon-EyeShow)"></span>Règles d'accès</span> dans la barre de gauche. Cliquez dessus pour afficher et modifier les règles d'accès. Les règles sont également accessibles via l'option `Manage Users` du menu de partage<span class="grist-icon" style="--icon: var(--icon-Share)"></span> avec le bouton `Open Access Rules` (disponible uniquement pour les propriétaires de documents).

Supposons que nous dirigeons une petite entreprise qui recherche et livre des objets inhabituels, organisés à l'aide d'un document à deux tables, `Orders` et `Financials` . Maintenant, nous embauchons plus d'employés et voulons partager le document avec eux tout en limitant leur accès à ce dont ils ont besoin.

![Règles d'accès](images/access-rules/access-rules-example.png)

## Règles par défaut

Pour voir les règles d'accès d'un document, visitez sa page de règles d'accès en cliquant sur <span class="app-menu-item"><span class="grist-icon" style="--icon: var(--icon-EyeShow)"></span>Règles d'accès</span> dans la barre latérale gauche. Lorsqu'aucune règle personnalisée n'a encore été créée, la page des règles d'accès contient les `Default Rules` pour notre document :

![Règles d'accès](images/access-rules/access-rules-page.png)

Ces règles disent, en résumé, que les propriétaires et les éditeurs peuvent faire n'importe quoi dans le document, que les spectateurs ne peuvent que lire le document et que tout le monde est interdit d'accès. Ces règles ne peuvent pas être modifiées, mais elles peuvent être outrepassées. Pour comprendre si un groupe de règles autorise une certaine autorisation ( [Lire, Mettre à jour, Créer, Supprimer ou Structure](access-rules.md#access-rule-permissions) ), lisez les règles de haut en bas et recherchez la première règle applicable qui autorise (vert) ou refuse (rouge) cette autorisation. . Nous verrons plein d'exemples au fur et à mesure.

## Verrouiller la structure

Par défaut, les propriétaires et les éditeurs sont tout aussi puissants dans un document, avec la possibilité de créer ou de supprimer des tableaux ou des colonnes, d'écrire des formules, de réorganiser des pages, etc.

Supposons que nous souhaitions que seuls les propriétaires originaux du document soient autorisés à modifier sa structure, car nous prévoyons d'inviter d'autres collaborateurs spécialisés en tant qu'éditeurs. Pour ce faire, nous ajoutons une règle par défaut spécifiant que tout utilisateur qui n'est pas propriétaire ( `user.Access != OWNER` ) doit se voir refuser les autorisations de structure ( `S` ). Cliquez sur le petit signe `+` dans la `CONDITION` des `Default Rules` et remplissez la condition et les permissions comme suit :

![Règles d'accès](images/access-rules/access-rules-lock-structure.png)

Pour plus d'informations sur les formules possibles, consultez [Conditions des règles d'accès](access-rules.md#access-rule-conditions) .

Une fois que nous avons apporté des modifications, le bouton `SAVE` devient un vert invitant (s'il est grisé et que vous venez de composer une formule de condition, appuyez sur Entrée ou cliquez ailleurs pour faire savoir à Grist que vous avez terminé). Nous cliquons sur `SAVE` pour que la règle prenne effet. Si Grist trouve des erreurs, vous serez alerté et les modifications ne seront pas enregistrées.

![Règles d'accès](images/access-rules/access-rules-lock-structure-saved.png)

**Important.** Il s'agit d'une première étape importante pour tout document dans lequel vous avez l'intention de bloquer tout accès aux éditeurs. Sans leur refuser l'autorisation de structure ( `S` ), toute personne disposant d'un accès en modification pourra créer ou modifier des formules. Comme les calculs de formules ne sont pas limités par des règles de contrôle d'accès, un utilisateur déterminé peut les utiliser pour récupérer n'importe quelle donnée d'un document. Pour vous protéger contre cela, refusez l'autorisation de structure aux utilisateurs dont l'accès doit être limité.

## Créer une table privée

Pour garantir que seuls les propriétaires peuvent accéder à une table, telle que la table `Financials` dans notre exemple, nous cliquons sur `Add Table Rules` et sélectionnons le nom de la table, `Financials` . Cela crée un nouveau groupe vide de règles appelé `Rules for table Financials` . Ensuite, nous ajoutons une condition pour tout utilisateur qui n'est pas un propriétaire ( `user.Access != OWNER` ), avec toutes les autorisations refusées. La sélection de `Deny All` dans le menu déroulant à côté de `R` `U` `C` `D` est un moyen rapide de définir toutes les autorisations sur refusées, ou vous pouvez cliquer sur chaque autorisation individuellement pour les rendre rouges. `R` correspond à Lire, `U` à Mettre à jour, `C` à Créer et `D` à Supprimer (voir [Autorisations des règles d'accès](access-rules.md#access-rule-permissions) ). Les autorisations Structure ( `S` ) ne sont pas disponibles au niveau de la table. Une fois que vous avez terminé, cliquez sur `SAVE` .

![Règles d'accès](images/access-rules/access-rules-private-table.png)

Maintenant, nous pourrions aller de l'avant et partager le document avec un membre de l'équipe spécialisé dans les livraisons, par exemple. Nous [partageons le document](sharing.md) avec eux en tant qu'éditeur afin que les restrictions que nous avons définies s'appliquent à eux. Ils ne verront pas le tableau `Financials` dans la barre de gauche et les tentatives d'ouverture seront refusées :

![Règles d'accès](images/access-rules/access-rules-private-table-is-hidden.png)

## Restreindre l'accès aux colonnes

Nous pouvons restreindre l'accès d'un collaborateur aux colonnes. Dans notre exemple, nous pourrions souhaiter donner à un spécialiste de la livraison un accès plus limité à la table `Orders` . Peut-être qu'ils n'ont pas besoin de voir une colonne `Email` ou une colonne `Piece` avec des détails sur le contenu du colis.

Cliquez sur `Add Table Rules` et sélectionnez `Orders` pour créer un groupe de règles pour la table `Orders` . Maintenant, dans le groupe `Rules for table Orders` , cliquez sur l'icône à trois points (...), et sélectionnez `Add Column Rule` :

![Règles d'accès](images/access-rules/access-rules-limit-columns-rules.png)

Dans la zone `Columns` , nous avons une nouvelle liste déroulante `[Add Column]` pour ajouter toutes les colonnes auxquelles nous voulons que la règle s'applique (dans notre cas `Email` and `Piece` ). Pour la condition, nous pouvons utiliser `user.Email == 'kiwi@getgrist.com'` . Ceci vérifie l'adresse e-mail de Kimberly, notre spécialiste de la livraison fictive ; nous pourrions également vérifier par nom ou par identifiant numérique. Nous désactivons toutes les autorisations disponibles pour cet utilisateur sur ces colonnes :

![Règles d'accès](images/access-rules/access-rules-limit-columns-full-rules.png)

Maintenant que les règles sont prêtes, cliquez sur `Save` .

Si nous avons un autre employé spécialisé dans la recherche d'objets et qui a besoin de voir un ensemble de colonnes différent, nous pouvons le faire. Par exemple, nous ajoutons ici une règle pour retenir les colonnes `Address` et `Phone` de l'utilisateur `Charon` :

![Règles d'accès](images/access-rules/access-rules-sourcing-single.png)

## Afficher comme un autre utilisateur

Un moyen pratique de vérifier si les règles d'accès fonctionnent comme prévu consiste à utiliser la fonctionnalité `View As` que, disponible dans la liste déroulante `Users` . Cela permet à un propriétaire d'ouvrir le document comme s'il était l'une des personnes avec lesquelles il est partagé, pour voir ce que son collègue verrait. Le propriétaire ne "devient" pas ce collègue - toute modification qu'il apporte sera enregistrée comme venant de lui-même et non du collègue - mais il voit le document du point de vue du collègue.

![Règles d'accès](images/access-rules/access-rules-view-as.png)

Dans notre exemple, nous pourrions cliquer sur `View As` sur la ligne Kiwi, et le document rouvre, avec une petite étiquette rouge avertissant que nous le visualisons comme `kiwi@getgrist.com` . Les colonnes `Piece` et `Email` sont manquantes et la table `Financials` est supprimée :

![Règles d'accès](images/access-rules/access-rules-view-as-kiwi.png)

Lorsque nous sommes satisfaits que tout se présente comme prévu, nous cliquons sur le `x` dans la balise rouge pour fermer cet aperçu et le document se rechargera.

## Tables d'attributs utilisateur

Si nous réussissons et embauchons de nombreux fournisseurs et livreurs, les ajouter un par un aux règles serait fastidieux. Une solution consiste à utiliser des "tables d'attributs utilisateur". Vous pouvez ajouter un tableau à votre document qui classe les utilisateurs comme vous le souhaitez, puis utiliser ces classes dans vos règles d'accès. Par exemple, nous pouvons créer une table appelée `Team` et lui donner deux colonnes, `Email` et `Role` , où `Role` est un choix entre `Sourcing` et `Delivery` .

![Règles d'accès](images/access-rules/access-rules-team-table.png)

Nous pouvons maintenant dire à Grist de rendre les informations de cette table disponibles pour les règles d'accès, en cliquant sur `Add User Attributes` . Donnez à l'attribut le nom de votre choix (ce sera ainsi que nous l'appellerons dans les formules), comme `Team` . Choisissez le tableau à lire ( `Team` également dans ce cas). Donnez une propriété utilisateur à faire correspondre aux lignes de ce tableau - dans notre cas, nous utiliserons `user.Email` . Et la colonne à comparer, `Email` .

![Règles d'accès](images/access-rules/access-rules-team-attribute.png)

Sauf que. Maintenant, nous pouvons mettre à jour nos règles pour être plus générales. Nous constatons avec la saisie semi-automatique que nous avons une nouvelle variable `user.Team` disponible dans condtions. Il rend les colonnes de l' `Team` disponibles, telles que `user.Team.Role` . Nous pouvons maintenant vérifier si l'utilisateur a un rôle particulier et appliquer les autorisations qui vont avec :

![Règles d'accès](images/access-rules/access-rules-team-rules.png)

Génial! En faisant une vérification ponctuelle, Charon voit les colonnes attendues pour quelqu'un dans Sourcing. Et si nous recrutons quelqu'un d'autre pour travailler avec eux, nous pouvons simplement les ajouter dans le tableau de l' `Team` , aucun changement de règle n'est nécessaire.

![Règles d'accès](images/access-rules/access-rules-view-as-charon.png)

## Contrôle d'accès au niveau de la ligne

Dans notre exemple, au fur et à mesure que les commandes sont traitées, elles passent des phases d'approvisionnement aux phases de livraison. Il n'est donc vraiment pas nécessaire que les deux groupes voient toutes les commandes en même temps. Ajoutons une colonne appelée `Stage` qui peut être définie sur `Sourcing` ou `Delivery` , afin que nous puissions mettre à jour les règles d'accès pour afficher uniquement les commandes pertinentes.

![Règles d'accès](images/access-rules/access-rules-stage-column.png)

Dans le groupe `Rules for table Orders` , cliquez sur l'icône à trois points (...), puis sélectionnez `Add Default Rule` pour ajouter une règle qui n'est pas limitée à des colonnes spécifiques. Comme point de départ, refusons l'accès à toutes les lignes pour les non-propriétaires, puis rajoutons celles que nous voulons. Nous pouvons le faire avec la condition `user.Access != OWNER` avec les autorisations `Deny All` . Ensuite, nous ajoutons une autre règle par défaut en cliquant sur `+` et ajoutons la condition `user.Team.Role == rec.Stage` . La variable `rec` nous permet d'exprimer des règles qui dépendent du contenu d'un enregistrement particulier. Ici, nous vérifions si la colonne `Stage` d'un enregistrement correspond au rôle de l'utilisateur. Si c'est le cas, nous autorisons l'accès `R` Read :

![Règles d'accès](images/access-rules/access-rules-stage-rules.png)

Voici à quoi ressemble le tableau maintenant en tant que Kimberly (effectuant des livraisons) :

![Règles d'accès](images/access-rules/access-rules-stage-kiwi.png)

Et voici à quoi ressemble le tableau en tant que Charon (faisant l'approvisionnement) :

![Règles d'accès](images/access-rules/access-rules-stage-charon.png)

Kimberly et Charon ont désormais un accès en lecture seule au tableau. Les propriétaires ont toujours un accès en écriture complet à toutes les lignes et colonnes.

!!! note "Comprendre les colonnes de référence dans les règles d'accès" Vous pouvez limiter l'accès des membres de l'équipe de données aux seules lignes pertinentes pour leur travail. Une façon de procéder consiste à associer tous les enregistrements de toutes les tables aux membres de leur équipe respective. Par exemple, les enregistrements de prospects et de ventes peuvent faire référence au commercial responsable de ces enregistrements. Cette vidéo rapide explique comment.

```
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZL3rHdAZzfY?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

## Vérification des nouvelles valeurs

Les règles d'accès peuvent être utilisées pour n'autoriser que certaines modifications du document. Supposons que nous voulions que les `Delivery` puissent changer `Stage` de `Delivery` à `Done` , sans leur donner les droits arbitraires pour modifier cette colonne. Nous pouvons leur accorder ce droit exceptionnel comme suit. Dans le groupe `Rules for table Orders` , cliquez sur l'icône à trois points (...) et sélectionnez `Add Column Rule` . Définissez `[Add Column]` sur `Stage` et marquez l'autorisation U/Update à accorder. Pour la condition, utilisez ceci:

```
(user.Team.Role == 'Delivery' and
  rec.Stage == 'Delivery' and
  newRec.Stage == 'Done')
```

Cela vérifie si l'utilisateur a le rôle de livraison, et si l'enregistrement est à l'étape de livraison, et que l'utilisateur essaie de changer l'étape en `Done` . La variable `newRec` est une variante de `rec` disponible lorsque l'utilisateur propose de modifier un enregistrement, avec `rec` contenant son état avant le changement, et `newRec` son état après le changement proposé.

![Règles d'accès](images/access-rules/access-rules-delivery-done-rule.png)

Maintenant, si nous considérons la table comme Kiwi et essayons de changer une `Stage` en `Sourcing` , nous sommes refusés :

![Règles d'accès](images/access-rules/access-rules-delivery-done-forbid.png)

Si nous changeons une `Stage` en `Done` , cela fonctionne et l'enregistrement disparaît de la vue car il n'est plus à l'étape de `Delivery` :

![Règles d'accès](images/access-rules/access-rules-delivery-done-allow.png)

## Clés de liaison

Parfois, il est utile de donner accès à une petite tranche spécifique du document, par exemple une seule ligne d'un tableau. Grist propose une fonctionnalité appelée "clés de lien" qui peut vous aider. Tous les paramètres d'une URL de document Grist qui se terminent par un trait de soulignement sont rendus disponibles pour accéder aux règles dans une variable `user.LinkKey` . Ainsi, par exemple, si l'URL d'un document se termine par `....?Token_=xx-xx-xx-xx&Flavor_=vanilla` , alors `user.LinkKey.Token` sera défini sur `xx-xx-xx-xx` et `user.LinkKey.Flavor` sur `vanilla` . Prenons un exemple pour voir comment cela peut être utile.

Supposons que nous ayons une table de `Orders` et que nous aimerions partager occasionnellement des informations sur une seule commande avec quelqu'un. Pour ce faire, avec les clés de lien, nous avons besoin d'une sorte de code difficile à deviner pour chaque commande, qui peut être utilisé pour y accéder. Grist a une fonction [`UUID()`](functions.md#uuid) qui donne un identifiant unique, aléatoire et difficile à deviner, alors ajoutons une colonne `UUID` avec la formule `=UUID()` :

![Règles d'accès](images/access-rules/access-rules-linkkey-uuid-formula.png)

En fait, nous voulons que `UUID()` soit appelé une seule fois par commande, lorsque nous le créons, et jamais recalculé (car cela changerait). Ainsi, dans la barre latérale de droite, nous convertissons la colonne de formules en colonne de données, en gelant ses valeurs :

![Règles d'accès](images/access-rules/access-rules-linkkey-uuid-convert.png)

Cela convertit notre formule en une formule de déclenchement. Nous définissons la formule à appliquer aux nouveaux enregistrements :

![Règles d'accès](images/access-rules/access-rules-linkkey-uuid-data.png)

À ce stade, nous avons un code solide difficile à deviner pour chaque commande dans la colonne `UUID` , qui sera créé au fur et à mesure que nous ajouterons de nouvelles commandes. Il peut être pratique à ce stade de construire des liens vers le document avec ce code intégré. Grist a un assistant pour cela appelé [`SELF_HYPERLINK`](functions.md#self_hyperlink) . Pour ajouter une clé de lien appelée `<NAME>` , utilisez simplement cette fonction avec un `LinkKey_<NAME>` . Dans notre cas, nous passons `LinkKey_UUID=$UUID` pour intégrer la valeur de la colonne `UUID` dans l'URL. Nous définissons également `label=$Ref` pour contrôler l'étiquette de texte du lien dans la feuille de calcul. Pour afficher le lien, nous définissons le type de colonne sur `Text` et définissons l'option `HyperLink` :

![Règles d'accès](images/access-rules/access-rules-linkkey-link.png)

Une fois que nous avons ces liens, nous pouvons ranger un peu en masquant les colonnes `UUID` et `Ref` (voir [Opérations sur les colonnes](widget-table.md#column-operations) pour un rappel sur la façon de procéder) :

![Règles d'accès](images/access-rules/access-rules-linkkey-prune.png)

Les liens ne font encore rien de spécial, mais nous avons tout ce dont nous avons besoin pour que cela se produise maintenant. Voici un exemple de règles d'accès pour permettre à toute personne ayant un UUID dans son URL de lire toute commande avec un UUID correspondant (sinon seuls les propriétaires peuvent lire les commandes dans ce cas) :

![Règles d'accès](images/access-rules/access-rules-linkkey-rule.png)

Et voici ce que voit désormais un non-propriétaire, avec l'UUID de premier ordre dans son URL :

![Règles d'accès](images/access-rules/access-rules-linkkey-use.png)

Ce n'est que le début des possibilités. Les clés de liaison peuvent donner accès à plusieurs lignes dans de nombreuses tables. Ils peuvent être utilisés dans [les tables d'attributs utilisateur](#user-attribute-tables) . Et les données auxquelles ils donnent accès peuvent se trouver dans des tableaux, des cartes, des listes de cartes, des graphiques et des widgets personnalisés.

Découvrez un [autre exemple](https://support.getgrist.com/examples/2021-04-link-keys/) pour approfondir encore plus votre compréhension des clés de lien.

## Conditions des règles d'accès

Les conditions de règle d'accès contiennent une formule indiquant quand la règle doit s'appliquer. Une condition vide s'appliquera toujours. Lorsqu'une condition s'applique à une action, les autorisations associées à la condition sont définies sur autorisées ou refusées pour cette action si aucune règle antérieure du même groupe ne les a encore définies. Lorsqu'une condition ne s'applique pas, aucune autorisation n'est définie par cette règle, mais d'autres règles peuvent les définir.

Les formules sont écrites dans un sous-ensemble restreint de Python. Les variables qui peuvent être disponibles dans les règles d'accès sont `user` , `rec` et `newRec` .

La variable `user` contient les membres suivants :

- `user.Access` : l'un des `owners` , `editors` ou `viewers` , indiquant comment le document a été partagé avec l'utilisateur (voir [Partager un document](sharing.md) ).
- `user.Email` : l'adresse email de l'utilisateur (ou `anon@getgrist.com` pour les utilisateurs non connectés).
- `user.UserID` : un identifiant numérique associé à l'utilisateur.
- `user.Name` : le nom de l'utilisateur (ou `Anonymous` si indisponible).
- `user.LinkKey` : un objet avec tous les paramètres d'URL de contrôle d'accès. Les paramètres d'URL de contrôle d'accès se terminent par un trait de soulignement (qui est ensuite supprimé). Disponible uniquement dans le client Web, pas dans l'API.
- `user.Origin` : Le contenu de l'en-tête de la requête Origin. Uniquement disponible dans l'API, pas dans le client Web.
- `user.SessionID` : une chaîne unique attribuée aux utilisateurs anonymes pour la durée de la session de cet utilisateur. Pour les utilisateurs connectés, `user.SessionID` est toujours `"u"` + l'identifiant numérique de l'utilisateur.

Pour un exemple d'utilisation de la variable `user` , lisez [Règles par défaut](access-rules.md#default-rules) .

La variable `rec` contient l'état d'un enregistrement/ligne individuel, pour les conditions qui doivent en tenir compte. Lorsqu'elle est utilisée, cette règle devient spécifique à la ligne. Cela permet par exemple de rendre certaines lignes visibles uniquement à certains utilisateurs, ou d'interdire la modification de certaines lignes par certains utilisateurs. Pour un exemple d'utilisation de la variable `rec` , consultez [Contrôle d'accès au niveau de la ligne](access-rules.md#row-level-access-control) .

La variable `newRec` est disponible pour la création et la mise à jour d'enregistrements/lignes, et contient l'état d'une ligne après une modification proposée, vous permettant d'autoriser ou de refuser sélectivement certaines modifications. Pour un exemple d'utilisation de la variable `newRec` , consultez [Vérification des nouvelles valeurs](access-rules.md#checking-new-values) .

Les opérations prises en charge dans les formalités de condition sont actuellement : `and` , `or` , `+` , `-` , `*` , `/` , `%` , `==` , `!=` , `<` , `<=` , `>` , `>=` , `is` , `is not` , `in` , `not in` . Les variables prises en charge sont : `user` , `rec` , `newRec` avec leurs membres accessibles avec `.` . Les chaînes, les nombres et les listes sont également pris en charge. Si une opération dont vous avez besoin n'est pas disponible, déterminez si vous pouvez effectuer une partie du travail dans une formule dans la table elle-même (voir [les mémos de règle d'accès](access-rules.md#access-rule-memos) pour un exemple).

Les commentaires sont autorisés, en utilisant `#` ou `"""` . S'il y a un commentaire dans une règle, alors le premier commentaire dans une règle qui entraîne le refus d'une action sera signalé à l'utilisateur comme un conseil expliquant pourquoi l'action n'a pas été Voir [les mémos de règle d'accès](access-rules.md#access-rule-memos) pour un exemple.

## Autorisations des règles d'accès

Une autorisation contrôle si un utilisateur peut effectuer un type d'action particulier. Les règles d'accès de Grist traitent actuellement de 5 types d'action, auxquels sont attribués des acronymes à une seule lettre pour plus de commodité :

- `R` - autorisation de lire les cellules.
- `U` - autorisation de mettre à jour les cellules.
- `C` - autorisation de créer des lignes.
- `D` - autorisation de supprimer des lignes.
- `S` - autorisation de modifier la structure de la table.

L'autorisation de structure `S` est disponible dans le groupe de règles d'accès par défaut. Les règles de colonne ne disposent pas des autorisations de création `C` et de suppression `D` , qui doivent être gérées dans les règles de table par défaut.

**Remarque :** La permission `S` est très puissante. Il permet d'écrire des formules, qui peuvent accéder à toutes les données du document, quelles que soient les règles. Étant donné que l'autorisation `S` est activée par défaut pour les éditeurs et les propriétaires, tout utilisateur de ce type pourrait modifier une formule et ainsi récupérer toutes les données.

En d'autres termes, avoir la permission `S` permet de contourner d'autres règles qui empêchent l'accès aux données. Pour cette raison, la désactiver - comme décrit ci-dessus dans [Verrouillage de la structure](#lock-down-structure) - est une première étape importante pour limiter l'accès aux données.

## Accéder aux notes de règles

Lorsqu'un utilisateur reçoit un message d'erreur lui refusant l'accès en raison d'une règle, il peut être utile de donner des détails spécifiques qui l'aideront à comprendre le problème. Vous pouvez le faire en ajoutant un commentaire dans la formule de condition. Le premier commentaire d'une condition sera transmis à l'utilisateur en cas de condition entraînant un refus d'accès (soit par correspondance, soit par défaut de correspondance). Les commentaires sont de style python, commençant par un `#` . Ils peuvent venir après la formule comme ceci :

```py
newRec.Count > 1  # No duplicates allowed
```

Ou avant, comme ceci :

```py
# Talk to Arjun to get full access
user.Access == OWNER
```

À titre d'exemple complet, supposons que nous ayons une table répertoriant les aéroports et que nous souhaitions interdire la saisie d'un nouvel enregistrement avec le même code d'aéroport qu'un aéroport existant. Dans le tableau, nous pouvons ajouter une formule `Count` qui compte combien d'enregistrements ont le même code les uns que les autres :

![Table d'aéroport](images/access-rules/access-rules-dupe-setup.png)

Maintenant, nous pouvons ajouter une règle d'accès pour interdire toute mise à jour ou création d'enregistrement qui entraînerait un `Count` supérieur à un. Nous pouvons également inclure un mémo pour expliquer le problème :

![Règle en double](images/access-rules/access-rules-dupe-rule.png)

Maintenant, si nous essayons d'ajouter une nouvelle ligne avec un code existant, nous obtenons une erreur utile :

![Erreur de doublon](images/access-rules/access-rules-dupe-forbidden.png)

Cet exemple montre également que parfois la condition d'une règle d'accès doit être exprimée en deux parties : une formule indépendante de l'utilisateur dans la table elle-même (qui a accès à toutes les fonctionnalités de la feuille de calcul mais n'a pas accès à l'identité ou aux attributs de l'utilisateur), et une formule dépendante de l'utilisateur dans la page de règle d'accès (qui a accès à l'identité et aux attributs de l'utilisateur, mais a des limites à sa structure). Voir [Conditions de règle d'accès](access-rules.md#access-rule-conditions) pour plus de détails sur les restrictions sur les formules de condition et [Formules](formulas.md) pour plus de détails sur les formules complètes standard.

## Exemples de règles d'accès

En plus de l'exemple détaillé d'utilisation des règles d'accès dans cette section, nous rassemblerons ici des exemples complets de modèles et de guides de règles d'accès.

- [Listes](examples/2021-03-leads.md) de prospects : Une liste de prospects très simple, attribuée aux personnes à suivre, avec contrôle des affectations réservées aux propriétaires des documents.
- [Équipe de vente basée sur le compte](https://templates.getgrist.com/38Dz6nMtzvwC/Account-based-Sales-Team) : CRM de vente avec des offres et des contacts attribués aux commerciaux. Les représentants ne peuvent voir que leurs propres contacts et offres, mais les responsables peuvent tout voir.
- [Public Giveaway](https://templates.getgrist.com/vP7WpQp89hLi/Public-Giveaway) : Un organisateur public de cadeaux qui utilise des règles d'accès pour appliquer les règles de cadeaux sans obliger les demandeurs à se connecter à Grist.
- [Sondage simple](https://templates.getgrist.com/jd234iH1zDsL/Simple-Poll) : Un sondage simple géré dans Grist avec des règles d'accès pour limiter une réponse par visiteur.
- [Liste crowdsourcée : Liste](https://templates.getgrist.com/dKztiPYamcCp/Crowdsourced-List) publiquement crowdsourcée avec des règles d'accès permettant aux modérateurs de modifier presque tout, mais limitant les visiteurs à ne faire et modifier que leurs propres contributions.
- [Feuilles de temps](https://templates.getgrist.com/oGxD8EnzeVs6/Time-Sheets) : modèle pour capturer les feuilles de temps des sous-traitants. Les règles d'accès permettent aux sous-traitants de visualiser uniquement leurs feuilles de temps historiques et de modifier uniquement le mois actif.
- [Gestion](https://templates.getgrist.com/hifkng53AxyQ/Project-Management/) de projet : suivez les tâches par événement et signalez les tâches à risque. Les règles d'accès limitent les autorisations par service et étendent les autorisations des responsables.
