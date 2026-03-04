# 🎮 Super Morpion — Ultimate Tic-Tac-Toe

> Un jeu de Morpion ultime en HTML/CSS/JS vanilla — aucune dépendance, zéro framework, 100% fun.

---

## 🕹️ Aperçu

**Super Morpion** (aussi connu sous le nom *Ultimate Tic-Tac-Toe*) est une version avancée du célèbre morpion. Le plateau de jeu est une grille **3×3 de mini-morpions**, ce qui ajoute une couche de stratégie redoutable : chaque coup que vous jouez dicte **où votre adversaire devra jouer** au prochain tour.

```
┌───────┬───────┬───────┐
│  ╔═╗  │  ╔═╗  │  ╔═╗  │
│  ║ ║  │  ║ ║  │  ║ ║  │
│  ╚═╝  │  ╚═╝  │  ╚═╝  │
├───────┼───────┼───────┤
│  ╔═╗  │  ╔═╗  │  ╔═╗  │
│  ║ ║  │  ║ ║  │  ║ ║  │
│  ╚═╝  │  ╚═╝  │  ╚═╝  │
├───────┼───────┼───────┤
│  ╔═╗  │  ╔═╗  │  ╔═╗  │
│  ║ ║  │  ║ ║  │  ║ ║  │
│  ╚═╝  │  ╚═╝  │  ╚═╝  │
└───────┴───────┴───────┘
     9 mini-plateaux
     81 cases au total
```

---

## ✨ Fonctionnalités

| Fonctionnalité | Détail |
|---|---|
| 🎯 Règles officielles | Redirection de tour, zones libres si plateau terminé |
| 🏆 Détection victoire | Locale (mini-plateau) + Globale (grille principale) |
| 🌟 Zones jouables | Bordure dorée lumineuse (glow) sur le mini-plateau actif |
| 🚫 Zones bloquées | Grisées automatiquement, non cliquables |
| 🎭 Overlay victoire | Grand X ou O affiché sur chaque mini-plateau gagné |
| 💥 Confettis | Animation de fête en cas de victoire |
| 📊 Compteur de scores | Persistant entre les parties de la même session |
| 🔄 Reset instantané | Bouton "Nouvelle partie" + bouton "Rejouer" dans le modal |
| 📱 Responsive | Fonctionne sur mobile, tablette et desktop |
| ⚡ Zéro dépendance | Pas de framework, pas de CDN, pas de fichier externe |

---

## 📐 Règles du jeu

### Principe de base

1. Le plateau est une grille **3×3** contenant **9 mini-morpions** (chacun 3×3).
2. **Joueur 1** joue `X`, **Joueur 2** joue `O`. X commence toujours.
3. Gagnez des mini-plateaux en alignant 3 symboles. Gagnez la partie en alignant 3 mini-plateaux.

### La règle clé ⚡

> **La case choisie dans un mini-plateau détermine le mini-plateau où l'adversaire doit jouer.**

**Exemple :** Si vous jouez dans la case **en haut à droite** d'un mini-plateau, votre adversaire devra jouer dans le **mini-plateau en haut à droite** de la grande grille.

```
Vous jouez ici →  ┌─────┐      Adversaire doit jouer ici ↓
                  │ · · X│      ┌─────┬─────┬─────┐
                  │ · · · │     │     │     │ ◄── │
                  │ · · · │     │     │     │     │
                  └─────┘      └─────┴─────┴─────┘
```

### Cas particuliers

- **Mini-plateau déjà gagné ou plein** → le joueur peut jouer **n'importe où** sur un mini-plateau encore disponible.
- **Match nul local** → le mini-plateau est marqué `—` et compte comme neutre (ne profite à personne).
- **Match nul global** → si tous les mini-plateaux sont terminés sans vainqueur, la partie est nulle.

### Conditions de victoire

Alignez **3 mini-plateaux gagnés** en ligne, colonne ou diagonale :

```
X | O | X        X | · | ·        X | · | ·
---------        ---------        ---------
X | O | ·        · | X | ·        · | X | ·
---------        ---------        ---------
X | · | O        · | · | X        X | · | X
 Colonne 0        Diagonale ↘      Non-valide
```

---

## 🚀 Installation & Lancement

Aucune installation requise. Le jeu est **un seul fichier HTML autonome**.

```bash
# Option 1 — Ouvrir directement dans le navigateur
open super-morpion.html

# Option 2 — Serveur local (optionnel)
python3 -m http.server 8080
# puis ouvrir http://localhost:8080/super-morpion.html

# Option 3 — Via VS Code Live Server
# Clic droit sur le fichier → "Open with Live Server"
```

Compatibilité navigateurs :

| Navigateur | Support |
|---|---|
| Chrome 90+ | ✅ Complet |
| Firefox 88+ | ✅ Complet |
| Safari 14+ | ✅ Complet |
| Edge 90+ | ✅ Complet |
| IE 11 | ❌ Non supporté |

---

## 🏗️ Architecture du code

Le fichier est organisé en sections clairement délimitées par des commentaires :

```
super-morpion.html
├── <style>
│   ├── Variables CSS & Reset
│   ├── Header
│   ├── Status bar & scores
│   ├── Plateau principal (#main-board)
│   ├── Mini-plateaux (.mini-board)
│   │   ├── .playable   → zone jouable (glow doré)
│   │   ├── .blocked    → zone grisée
│   │   ├── .won-x / .won-o / .draw
│   ├── Cellules (.cell)
│   ├── Overlay victoire locale (.mini-board-overlay)
│   ├── Modal de victoire (#victory-modal)
│   ├── Confettis (.confetti-piece)
│   └── Media queries (responsive)
│
└── <script>
    ├── CONSTANTES          (WIN_COMBOS, PLAYER, EMPTY)
    ├── ÉTAT DU JEU         (state object)
    ├── initState()         → initialisation / reset de l'état
    ├── buildBoard()        → génération du DOM (une seule fois)
    ├── render()            → orchestrateur de rendu
    │   ├── renderCells()
    │   ├── renderBoardStates()
    │   ├── renderTurnDisplay()
    │   └── renderScores()
    ├── handleCellClick()   → gestionnaire de clic
    ├── isMoveValid()       → validation du coup
    ├── playMove()          → enregistrement + vérification
    ├── checkWinner()       → algorithme de victoire (local + global)
    ├── isBoardFull()       → détection de remplissage
    ├── endGame()           → fin de partie + scores
    ├── showVictoryModal()  → affichage du modal
    ├── launchConfetti()    → animation de confettis
    ├── resetGame()         → réinitialisation
    └── Helpers DOM         (getMiniBoard, getCell)
```

### Structure de données

```javascript
state = {
  boards: [             // 9 mini-plateaux × 9 cases
    [null, 'X', 'O', ...],  // null = vide
    ...
  ],
  boardWinners: [       // Résultat de chaque mini-plateau
    null,               // null = en cours
    'X',                // 'X' ou 'O' = gagné
    'draw',             // 'draw' = match nul
    ...
  ],
  currentPlayer: 0,     // 0 = X, 1 = O
  activeBoard: -1,      // -1 = libre, 0-8 = plateau imposé
  winner: null,         // null | 'X' | 'O' | 'draw'
  scores: { X: 0, O: 0 }
}
```

---

## 🎨 Design System

### Palette de couleurs

| Variable CSS | Valeur | Usage |
|---|---|---|
| `--bg` | `#0a0a0f` | Fond principal |
| `--surface` | `#12121c` | Surfaces élevées |
| `--x-color` | `#ff4d6d` | Joueur X (rouge-rose) |
| `--o-color` | `#00e5ff` | Joueur O (cyan) |
| `--active-border` | `#ffd700` | Zone jouable (or) |
| `--muted` | `#555570` | Textes secondaires |

### Typographie

- **Orbitron** — Titres, symboles, boutons (style sci-fi/gaming)
- **Share Tech Mono** — Corps de texte, interface (monospace tech)

### Animations

| Animation | Déclencheur | Durée |
|---|---|---|
| `cellPop` | Cellule jouée | 250ms |
| `cardIn` | Ouverture modal | 400ms (spring) |
| `confettiFall` | Victoire | 1.5–3.5s |
| Glow pulsant | Zone jouable | CSS continu |

---

## 🧠 Stratégie & Conseils

- **Contrôlez le centre** : jouer en case centrale envoie l'adversaire sur le mini-plateau central — le plus stratégique.
- **Forcer les mauvaises zones** : envoyez votre adversaire sur un plateau déjà terminé pour lui donner la liberté de jouer partout — évitez ça.
- **Pensez deux coups à l'avance** : où enverrez-vous votre adversaire, et où vous enverra-t-il en retour ?
- **Gagnez les coins** : comme au morpion classique, les mini-plateaux aux coins offrent le plus de combinaisons gagnantes.

---

## 📁 Fichiers du projet

```
projet/
├── super-morpion.html   # Le jeu complet (tout-en-un)
└── README.md            # Ce fichier
```

---

## 🛠️ Pistes d'évolution

- [ ] Mode solo contre une IA (Minimax ou MCTS)
- [ ] Mode multijoueur en ligne (WebSocket)
- [ ] Historique des coups + bouton "Annuler"
- [ ] Thèmes de couleurs alternatifs
- [ ] Sons d'ambiance et effets audio
- [ ] Sauvegarde de la partie (localStorage)
- [ ] Tournoi (meilleur de 3 / 5 / 7)

---

## 📜 Licence

Projet libre — Faites-en ce que vous voulez. Jouez, modifiez, partagez. 🎉

---

<div align="center">

**Fait avec ❤️ en HTML, CSS & JavaScript vanilla**

*Aucun framework n'a été blessé lors de la création de ce jeu.*

</div>
