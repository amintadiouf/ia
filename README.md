# Projet IA : Puissance 4 augmenté

## Règlement v1

**Membres du groupe :**

- Nour Karbil
- Aminta Diouf
- Mehmet Bozkurt

---

## Conventions communes à toutes les règles

**Plateau.** Le plateau compte 7 colonnes numérotées de 0 à 6 de gauche à droite, et 6 lignes numérotées de 0 (en haut) à 5 (en bas), comme dans `moteur.py`. La position [l,c] désigne la case `grille[l][c]`, c'est-à-dire la ligne l et la colonne c.

**Deux jokers différents.** Le règlement contient deux éléments appelés joker, qu'il ne faut pas confondre :

- la **boule joker** (règles 1 et 2) : un pouvoir que chaque joueur possède et qu'il choisit d'utiliser ; elle élimine toute une ligne et disparaît avec elle ;
- la **case joker** (règle 5) : une case fixe du plateau, en position [3,3], qui prend la couleur d'un joueur et redevient neutre quand une boule joker est jouée sous elle.

**Alignement.** Un alignement est une suite d'au moins quatre jetons consécutifs d'un même joueur dans une même direction : horizontale, verticale, diagonale montante ou diagonale descendante (voir règle 4).

**Score.** Chaque joueur possède un score : le nombre d'alignements qui lui ont été comptés depuis le début de la partie. Un alignement compté reste acquis jusqu'à la fin de la partie, même si les jetons qui le formaient disparaissent ou se déplacent ensuite. Le score ne diminue jamais.

**Notation des coups.** Dans la liste des coups utilisée par `rejouer()`, un coup normal dans la colonne c est noté `c` (de 0 à 6) et une boule joker jouée dans la colonne c est notée `c + 7` (de 7 à 13).

**Gravité.** Un jeton joué tombe sur la case libre la plus basse de sa colonne. Quand des jetons sont éliminés, les jetons situés au-dessus descendent pour combler les cases libérées, en conservant leur ordre. La case joker ne bouge jamais : dans la colonne 3, les jetons occupent toujours les cases libres les plus basses en sautant la case joker (voir règle 5).

### Déroulement d'un tour (ordre d'application des règles)

1. Le joueur joue un jeton normal ou sa boule joker dans une colonne non pleine. Le jeton tombe sur la case libre la plus basse.
2. Si c'est une boule joker, elle s'autodétruit en éliminant tous les jetons de la ligne où elle s'est arrêtée, quelle que soit leur couleur, puis les jetons situés au-dessus descendent (règle 1). Si cette ligne est la ligne 3, 4 ou 5, la case joker redevient neutre (règle 5).
3. Si la case joker est neutre et qu'un joueur possède trois jetons consécutifs juste à côté d'elle, du même côté, elle prend la couleur de ce joueur (règle 5).
4. Une fois tous les jetons immobiles, les nouveaux alignements des deux joueurs sont comptés, en tenant compte de la case joker si elle est colorée (règles 4 et 5).
5. Si le score d'un joueur vient d'atteindre 2 ou plus, il récupère sa boule joker s'il l'avait utilisée (règle 2).
6. Si toutes les cases sont occupées, la partie se termine et le vainqueur est désigné (règle 3). Sinon, la main passe à l'adversaire.

---

## Règle 1 : La boule joker

**Catégorie :** Jeton spécial

**Énoncé :** Chaque joueur dispose d'une boule joker, utilisable une seule fois pendant la partie à la place d'un coup normal. La boule joker est déposée dans une colonne et tombe comme un jeton ordinaire, puis elle s'autodétruit en éliminant tous les jetons de la ligne où elle s'est arrêtée, sans distinction de couleur. Les jetons situés au-dessus de la ligne éliminée descendent d'une case pour occuper les cases libérées.

**Déclenchement :** Le joueur peut utiliser sa boule joker pendant son tour, à la place de son coup normal, s'il en dispose encore et si la colonne choisie n'est pas pleine. Utiliser la boule joker compte comme le coup du tour. Dans la liste des coups, une boule joker jouée dans la colonne c est notée `c + 7`.

**Cas limite :**

- La ligne éliminée est la ligne de la case où la boule joker s'arrête en tombant.
- Tous les jetons de cette ligne sont éliminés, ceux de l'adversaire comme ceux du joueur qui utilise la boule joker. La boule joker est éliminée avec eux et ne reste jamais sur le plateau.
- Dans chaque colonne, tous les jetons situés au-dessus de la ligne éliminée, quelle que soit leur couleur, descendent d'une case en conservant leur ordre.
- Si la boule joker est seule sur sa ligne, elle s'autodétruit quand même et elle est consommée : le plateau reste tel qu'il était avant le coup, et la main passe à l'adversaire.
- La boule joker n'élimine jamais la case joker de la règle 5, qui n'est pas un jeton. Si la ligne éliminée est la ligne 3, 4 ou 5, la case joker reste à sa place mais redevient neutre (règle 5).
- Dans la colonne 3, les jetons qui descendent sautent la case joker et vont sur la case libre la plus basse.
- Une fois tous les jetons immobiles, les nouveaux alignements créés par la chute sont comptés pour leur propriétaire, que ce soit le joueur qui a utilisé la boule joker ou son adversaire (règle 4). Les alignements déjà comptés que la boule joker a détruits restent acquis.
- Une boule joker ne peut jamais remplir la grille, puisqu'elle libère au moins sa propre case : elle ne termine donc jamais la partie (règle 3).
- La boule joker ne peut pas être utilisée une fois la partie terminée. Une boule joker jouée dans une colonne pleine, ou par un joueur qui n'en dispose plus, est un coup illégal : il est refusé et le joueur doit jouer un autre coup.

**Interagit avec :** Règles 2, 3, 4 et 5.

---

## Règle 2 : Récupération de la boule joker

**Catégorie :** Jeton spécial

**Énoncé :** Lorsque le score d'un joueur atteint deux alignements de quatre au cours de la partie, il récupère sa boule joker s'il l'avait déjà utilisée. Le joueur pourra utiliser cette boule joker dès son prochain tour. Cette récupération n'a lieu qu'une seule fois par partie, si bien qu'un joueur ne peut jamais utiliser plus de deux boules joker.

**Déclenchement :** Le passage du score d'un joueur à 2 ou plus, vérifié à la fin de chaque tour, après le comptage de tous les alignements.

**Cas limite :**

- Le score peut atteindre 2 de plusieurs façons : un alignement puis un autre plus tard, deux alignements d'un seul coup (de 0 à 2), ou deux alignements alors qu'il en avait déjà un (de 1 à 3). Dans tous ces cas, le joueur récupère sa boule joker.
- Un alignement qui utilise la case joker (règle 5) compte dans le score comme n'importe quel autre alignement.
- Le deuxième alignement peut être obtenu pendant le tour de l'adversaire, par exemple quand la boule joker adverse fait descendre des jetons. Le joueur récupère alors sa boule joker et peut l'utiliser dès son tour suivant.
- Si la boule joker n'avait pas encore été utilisée au moment où le score atteint 2, le joueur la conserve, ne récupère pas de boule joker supplémentaire, et la récupération est perdue : il ne disposera que d'une seule boule joker dans la partie.
- Atteindre 3, 4 alignements ou plus ne redonne pas de boule joker.
- Si le deuxième alignement est réalisé au dernier coup de la partie, la boule joker récupérée ne peut plus servir.

**Interagit avec :** Règles 1, 3, 4 et 5.

---

## Règle 3 : Fin de partie et victoire aux alignements

**Catégorie :** Condition de victoire

**Énoncé :** La partie ne se termine pas lorsqu'un joueur réalise un premier alignement de quatre. Elle continue jusqu'à ce que toutes les cases du plateau soient remplies. À la fin de la partie, le joueur ayant le plus grand nombre d'alignements de quatre remporte la partie, et si les deux joueurs en ont le même nombre, la partie est déclarée nulle.

**Déclenchement :** La partie se termine à la fin du tour où les 42 cases du plateau sont occupées, une fois toutes les règles du tour appliquées et tous les alignements comptés.

**Cas limite :**

- Si un joueur réalise un ou plusieurs alignements de quatre lors du dernier coup, ceux-ci sont comptabilisés avant de déterminer le vainqueur.
- Si les deux joueurs ont le même nombre d'alignements de quatre, y compris zéro chacun, aucun joueur ne remporte la partie et le résultat est nul.
- La case joker occupe toujours la position [3,3] et compte comme une case occupée, qu'elle soit neutre ou colorée : la grille est donc pleine avec 41 jetons et la case joker.
- Une boule joker ne peut jamais terminer la partie, puisqu'elle s'autodétruit et libère au moins sa propre case (règle 1).
- Tant que la partie n'est pas terminée, aucun vainqueur n'est désigné, quel que soit l'écart de score.

**Interagit avec :** Règles 1, 4 et 5.

---

## Règle 4 : Double alignement

**Catégorie :** Géométrie / victoire

**Énoncé :** Lorsqu'un joueur réalise deux alignements de quatre différents avec un seul coup, chaque alignement est comptabilisé séparément. Ces alignements peuvent partager certains jetons, mais doivent correspondre à deux lignes différentes, c'est-à-dire à deux directions différentes ou à deux suites sans aucune case commune. Une suite de cinq, six ou sept jetons consécutifs dans une même direction ne compte que pour un seul alignement.

**Déclenchement :** Après chaque coup, une fois tous les jetons immobiles et la couleur de la case joker mise à jour, le nombre de nouveaux alignements de quatre de chaque joueur est vérifié.

**Cas limite :**

- Si un seul coup crée plusieurs alignements de quatre, tous les alignements correspondant à des lignes différentes sont comptabilisés. Par exemple, un jeton qui termine à la fois une ligne horizontale et une diagonale rapporte deux alignements.
- Un alignement déjà comptabilisé lors d'un tour précédent n'est pas comptabilisé une nouvelle fois s'il reste présent. Allonger un alignement existant (passer de quatre à cinq jetons) ne rapporte rien.
- Plus précisément, une suite ne rapporte un point que si aucune de ses cases n'appartient déjà, dans la même direction, à un alignement compté pour le même joueur. Un alignement détruit par une boule joker puis reformé sur les mêmes cases ne rapporte donc pas de nouveau point.
- Si deux suites de trois jetons sont réunies par un seul jeton en une suite de sept, cela ne rapporte qu'un seul alignement.
- Si un même tour crée des alignements pour les deux joueurs (cas possible après une boule joker), chacun est compté pour son propriétaire.
- Quand la case joker est neutre, elle n'appartient à personne et coupe les suites qui passent par elle. Quand elle est colorée, elle compte comme un jeton du joueur dont elle porte la couleur (règle 5).

**Interagit avec :** Règles 1, 2, 3 et 5.

---

## Règle 5 : La case joker

**Catégorie :** Jeton spécial

**Énoncé :** À la position [3,3], un emplacement joker prend la couleur du joueur qui réalise un alignement de 3 jetons. Le joker compte alors comme un quatrième jeton, permettant de compléter un alignement de 4. Cette règle est valable dans toutes les directions.

**Déclenchement :** La case joker occupe la position [3,3] dès le début de la partie et elle est neutre. Elle prend une couleur à la fin du tour où trois jetons consécutifs d'un même joueur occupent les trois cases qui la touchent dans une même direction, que ce trio vienne d'un coup normal ou d'une chute de jetons après une boule joker. Elle redevient neutre dès qu'une boule joker élimine la ligne 3, 4 ou 5, c'est-à-dire sa propre ligne ou une ligne située sous elle.

**Cas limite :**

- Un alignement composé de 2 jetons de la même couleur d'un côté du joker et d'un seul jeton de la même couleur de l'autre côté du joker ne permet pas de réaliser un alignement de 4. Le joker ne peut prendre une couleur que lorsqu'il permet de compléter un alignement de 3 jetons situés du même côté.
- Les trois jetons doivent occuper les trois cases immédiatement voisines de la case joker dans cette direction. Seules cinq directions sont possibles, car il n'y a que deux cases sous la case joker :
  - à gauche : [3,0], [3,1], [3,2] ;
  - à droite : [3,4], [3,5], [3,6] ;
  - au-dessus : [2,3], [1,3], [0,3] ;
  - en diagonale vers le haut à gauche : [2,2], [1,1], [0,0] ;
  - en diagonale vers le haut à droite : [2,4], [1,5], [0,6].
- L'alignement de 4 complété par la case joker est compté pour ce joueur dans le même tour (règle 4).
- Tant qu'elle est colorée, la case joker garde sa couleur, même si l'adversaire forme ensuite un trio à côté d'elle. Elle compte comme un jeton ordinaire de ce joueur pour tous ses alignements, dans toutes les directions.
- Quand une boule joker élimine la ligne 3, 4 ou 5, tous les jetons concernés descendent et la case joker redevient neutre, jusqu'au prochain alignement de 3 jetons d'une même couleur à côté d'elle. Si la chute crée immédiatement un nouveau trio à côté d'elle, elle reprend une couleur dans le même tour.
- Une boule joker qui élimine la ligne 0, 1 ou 2, au-dessus de la case joker, ne change pas sa couleur.
- Les alignements déjà comptés grâce à la case joker restent acquis quand elle redevient neutre.
- Si la case joker complète en même temps deux trios du même joueur dans deux directions différentes, les deux alignements sont comptés (règle 4).
- Si un même tour fait apparaître un trio pour chacun des deux joueurs alors que la case joker est neutre, elle prend la couleur du joueur dont c'est le tour.
- Si le trio est suivi d'autres jetons du même joueur de l'autre côté de la case joker, l'ensemble forme une seule suite et ne compte que pour un seul alignement (règle 4).
- La case joker ne bouge jamais et n'est jamais éliminée : elle ne tombe pas, la boule joker ne l'élimine pas, même quand elle est sur la ligne 3, et dans la colonne 3 les jetons occupent les cases libres les plus basses en la sautant. La colonne 3 reçoit donc cinq jetons en plus de la case joker.

**Interagit avec :** Règles 1, 2, 3 et 4.
