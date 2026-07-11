# Morpion pour BDFD - Version Definitive

Un jeu complet de Morpion pour **Bot Designer For Discord** (BDFD) avec mode **PvP** et mode **vs IA** (4 difficultes).

La difficulte **Impossible** utilise un **veritable algorithme minimax** avec une table precalculee de 19,683 etats, ce qui le rend mathematiquement parfait — il ne perd jamais.

---

## Caracteristiques

- **4 difficultes IA**: Facile, Normal, Difficile, Impossible (minimax)
- **Mode PvP**: Defie un autre joueur avec boutons accepter/refuser
- **Attribution aleatoire X/O**: Parfois tu commences, parfois l'IA commence
- **Systeme de revanche**: Les deux joueurs doivent voter pour redemarrer (PvP)
- **Abandonner/Redemarrer**: Abandonne a tout moment, redemarre apres la fin (vs IA)
- **Surlignage de la ligne gagnante**: Les 3 cases gagnantes deviennent vertes
- **Compteur de coups**: Montre X/9 coups joues
- **Aucune variable de serveur necessaire**: L'etat du jeu est stocke dans le footer de l'embed
- **Parties simultanees illimitees**: Chaque plateau est independant

---

## Configuration

### 1. Creer les commandes

Tu dois creer **3 commandes** dans ton bot BDFD:

| # | Declencheur | Langage | Fichier | Description |
|---|------------|---------|---------|-------------|
| 1 | `!ttt` | BDScript 2 | `Morpion_BDFD.md` | Commande principale (menu IA ou defi PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Gere menu, defis, abandon, revanche, redemarrage |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Gere les coups + IA minimax |

### 2. Pourquoi deux callbacks `$onInteraction`?

BDFD supporte plusieurs callbacks `$onInteraction` — les deux s'executent a chaque interaction. La table minimax (19,683 caracteres) est trop grande pour tenir dans un seul callback, donc le code est divise:

- **Callback 1**: Gere les routes A (menu difficulte), B (accepter defi), C (refuser defi), E (abandon/revanche/redemarrage)
- **Callback 2**: Gere la route D (jouer case) + la table minimax

Chaque callback utilise `$stop` pour ne pas interferer avec l'autre.

### 3. Permissions du bot

Le bot a besoin de ces permissions:
- Envoyer des messages
- Gerer les messages (pour modifier le plateau)
- Utiliser les composants (boutons / menus deroulants)

---

## Comment ca marche

### Stockage de l'etat

L'etat du jeu est stocke dans le **footer de l'embed** (pas dans des variables de serveur), au format:

```
ID_joueurX|ID_joueurO|difficulte|case0.case1.case2.case3.case4.case5.case6.case7.case8|voteRevancheX|voteRevancheO
```

- `ID_joueurX` / `ID_joueurO`: IDs utilisateur Discord (ou `IA` pour le mode IA)
- `difficulte`: `e` (Facile), `n` (Normal), `h` (Difficile), `i` (Impossible)
- `case0`-`case8`: `e` (vide), `X`, ou `O`
- `voteRevancheX` / `voteRevancheO`: `y` (vote) ou `n` (pas vote)

Cela rend chaque partie independante — parties simultanees illimitees dans le meme canal.

### Difficultes IA

| Difficulte | Strategie |
|-----------|-----------|
| **Facile** | Coups au hasard |
| **Normal** | Gagner si possible, bloquer si menace, sinon au hasard |
| **Difficile** | Gagner, bloquer, creer des fourches, bloquer les fourches de l'adversaire, centre, coin oppose, coin, cote |
| **Impossible** | **Minimax reel** — coup optimal precalcule pour les 19,683 etats. Ne perd jamais. |

### Algorithme minimax

L'IA Impossible utilise une **table minimax** precalculee en Python:

1. **Solver Python**: Execute un minimax exhaustif sur les 19,683 etats possibles du plateau (3^9). Pour chaque etat valide ou c'est le tour de l'IA, il calcule le coup optimal en supposant un jeu parfait de l'adversaire.

2. **Table precalculee** (19,683 caracteres): Chaque position dans la chaine represente un etat du plateau (indexe par hash base-3). Le caractere a cette position est le coup optimal (0-8) ou `-` si l'etat est invalide.

3. **Dans BDFD**, pour la difficulte Impossible:
   - Mappe les cases vers des nombres: `e=0`, `iaSym=1`, `userSym=2`
   - Calcule le hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Consulte la table: `$splitText[$sum[$var[hash];1]]`
   - Joue ce coup optimal

### Attribution aleatoire X/O

Au demarrage d'une partie (nouvelle, redemarrage ou revanche), un nombre aleatoire (`$random[0;2]`) determine qui est X et qui est O:

- **50% de chance**: L'utilisateur est X (commence en premier)
- **50% de chance**: L'IA est X (commence en premier, joue le centre automatiquement)

L'IA est entierement parametree avec `$var[iaSym]` et `$var[userSym]` pour fonctionner correctement qu'elle soit X ou O.

### Systeme de revanche (PvP)

Apres la fin d'une partie PvP:
1. Le bouton change en "Revanche (0/2)"
2. Chaque joueur appuie pour voter — le bouton se met a jour en "Revanche (1/2)"
3. Quand les deux votent (2/2), le plateau se reinitialise avec une nouvelle attribution aleatoire X/O
4. Seuls les deux joueurs peuvent voter; les autres recoivent une erreur

---

## Utilisation

### Jouer vs IA

```
!ttt
```

Choisis une difficulte dans le menu deroulant. Tu seras attribue X ou O au hasard.

### Jouer vs un autre joueur

```
!ttt @utilisateur
```

Le joueur defie doit appuyer sur "Accepter" pour commencer. X/O est attribue au hasard.

### Pendant la partie

- Clique sur n'importe quelle case vide (⬜) pour placer ta marque
- Appuie sur "Abandonner" pour abandonner
- Apres la fin de la partie, appuie sur "Redemarrer" (vs IA) ou "Revanche" (PvP) pour rejouer

---

## Credits

Cree par **Nexusify team**.

Tu es libre d'utiliser ce code, mais merci de ne pas retirer les credits. Cela a pris 3 mois de travail — respecte l'effort.

---

## Licence

Utilisation libre avec credits. Ne reclame pas le code comme le tien.
