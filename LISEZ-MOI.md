# Morpion pour BDFD

Un jeu de Morpion pour Bot Designer For Discord (BDFD), avec un mode PvP et un mode contre IA à 4 difficultés. La difficulté Impossible utilise minimax avec une table précalculée de tous les 19.683 états du plateau, donc il ne perd jamais.

## Ce qu'il inclut

- 4 difficultés IA : Facile, Normal, Difficile, Impossible (minimax)
- Mode PvP avec boutons accepter/refuser pour les défis
- Attribution aléatoire de X/O à chaque partie
- Système de revanche : les deux joueurs doivent voter pour redémarrer
- Abandonner et redémarrer en mode IA
- La ligne gagnante est surlignée en vert
- Compteur de coups (X/9)
- Aucune variable de serveur nécessaire, l'état du jeu vit dans le footer de l'embed
- Parties simultanées illimitées, chaque plateau est indépendant

## Installation

Il te faut 3 commandes dans ton bot :

| # | Déclencheur | Langage | Fichier | Ce qu'il fait |
|---|-----------|----------|------|----------------|
| 1 | `!ttt` | BDScript 2 | `Morpion_BDFD.md` | Commande principale (menu IA ou défi PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menu, défis, abandon, revanche, redémarrage |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Coups + IA minimax |

**Pourquoi deux callbacks `$onInteraction` ?** BDFD permet d'en avoir plusieurs, et les exécute tous à chaque interaction. La table minimax fait 19.683 caractères et ne tient pas avec le reste du code dans un seul callback, donc elle est divisée en deux : le premier gère le menu de difficulté, accepter/refuser et abandon/revanche/redémarrage ; le second gère les clics sur les cases et la table minimax. Chacun utilise `$stop` pour ne pas se marcher sur les pieds.

Permissions nécessaires pour le bot : envoyer des messages, gérer les messages (pour éditer le plateau) et utiliser les composants (boutons/menus déroulants).

## Comment ça marche

### État du jeu

Stocker dans le footer de l'embed, avec ce format :

```
ID_joueurX|ID_joueurO|difficulté|case0.case1.case2.case3.case4.case5.case6.case7.case8|voteRevancheX|voteRevancheO
```

- `ID_joueurX` / `ID_joueurO` : ID utilisateur Discord (ou `IA`)
- `difficulté` : `e` Facile, `n` Normal, `h` Difficile, `i` Impossible
- `case0`–`case8` : `e` vide, `X` ou `O`
- `voteRevancheX` / `voteRevancheO` : `y` ou `n`

Comme ça ne dépend pas de variables de serveur, chaque partie est autonome et tu peux en avoir plusieurs en cours dans le même canal.

### Difficultés IA

| Difficulté | Stratégie |
|-----------|----------|
| Facile | Coups au hasard |
| Normal | Gagne si possible, bloque la menace, sinon au hasard |
| Difficile | Gagner, bloquer, créer des fourches, bloquer les fourches adverses, centre, coin opposé, coin, côté |
| Impossible | Minimax avec table précalculée pour les 19.683 états. Ne perd jamais. |

### L'algorithme minimax

Pour la difficulté Impossible :

1. `minimax_solver.py` lance un minimax exhaustif sur les 3⁹ = 19.683 états possibles du plateau, et pour chaque état où c'est à l'IA de jouer il calcule le coup optimal en supposant que l'adversaire joue parfaitement.
2. Ça est stocké comme une table de 19.683 caractères : chaque position correspond à un état (indexé par un hash en base 3), et le caractère à cet endroit est le coup optimal (0-8), ou `-` si l'état n'est pas valide.
3. Dans BDFD : les cases sont mappées vers des nombres (`e=0`, `iaSym=1`, `userSym=2`), le hash est calculé comme `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`, cherché avec `$splitText[$sum[$var[hash];1]]`, et ce coup est joué.

### Attribution aléatoire de X/O

Au début d'une partie (nouvelle, redémarrage ou revanche), `$random[0;2]` décide qui est X : la moitié du temps tu commences, l'autre moitié c'est l'IA (et elle joue le centre automatiquement). L'IA est paramétrée avec `$var[iaSym]` et `$var[userSym]`, donc elle fonctionne pareil qu'elle soit X ou O.

### Revanche (PvP)

Quand une partie PvP se termine, le bouton change en "Revanche (0/2)". Chaque joueur vote en appuyant dessus, et quand ça arrive à 2/2 le plateau se réinitialise avec une nouvelle attribution aléatoire de X/O. Seuls les deux joueurs originaux peuvent voter ; si quelqu'un d'autre essaie, il reçoit une erreur.

## Structure du repo

```
Repository/
│
├── README.md                          (version anglaise)
├── LISEZ-MOI.md                       (ce fichier)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md
│   ├── Callback 1 ($onInteraction).md
│   └── Callback 2 ($onInteraction).md
│
└── Français/
    ├── Morpion_BDFD.md
    ├── Callback 1 ($onInteraction).md
    └── Callback 2 ($onInteraction).md
```

## Utilisation

Contre l'IA : tape `!ttt` et choisis une difficulté dans le menu déroulant. Tu seras assigné X ou O au hasard.

Contre un autre joueur : `!ttt @utilisateur`. Le joueur défié accepte avec le bouton, et X/O est aussi attribué au hasard.

Pendant la partie : clique une case vide (⬜) pour jouer, "Abandonner" pour lâcher, et quand c'est fini "Redémarrer" (vs IA) ou "Revanche" (PvP) pour relancer.

## Détails techniques

- Langage : BDScript 2 (utilise `$eval`, `$var`, `$try`, `$async`)
- Chaque callback reste sous la limite de 65.500 caractères de BDFD
- Aucune dépendance externe, tout vit dans les 3 commandes
- Aucune variable de serveur, l'état vit dans les footers des embeds

## Crédits

Fait par l'équipe Nexusify. Tu es libre d'utiliser le code, garde juste les crédits.

## Licence

Utilisation libre avec crédits. Ne le présente pas comme le tien.
