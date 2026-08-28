# Proto 1 — Tailler une pierre de A à Z

## Question testée (une seule)
**Tailler une pierre au clic est-il satisfaisant en solo ?**

Juge honnête (anti-biais de créateur) :
- Ai-je envie de tailler une **2e pierre** sans me forcer ?
- Le **frisson du dernier coup** près du trait est-il là ? (« je m'arrête ou je
  tente un cube de plus au risque de dépasser »)

> Rappel : Michelangelo (2025) a déjà prouvé commercialement que oui. Ce proto
> est surtout le **socle technique** du Proto 2. Ne pas s'y attarder.

---

## Brief pour Claude Code

Prototype web, **un seul fichier HTML**, canvas 2D pur, aucun framework, aucun
build, aucun moteur 3D. Rendu en **projection isométrique 2D façon Age of
Empires** : la pierre est un **bloc de petits cubes empilés** (grille de
voxels, ex. 8×8×8) dessinés en losanges iso, **vue fixe à 45°, pas de rotation
de caméra**. Test de game-feel, pas un jeu — zéro fioriture, priorité au
ressenti du geste.

**La pierre :** un bloc plein de cubes au départ, plus large que la cible. Une
silhouette-guide (cubes à conserver vs à enlever, en surbrillance légère) et un
« trait » = la limite à ne pas dépasser.

**Tailler = retirer des cubes.** Clic sur un cube visible = un coup avec
l'outil courant, qui retire un ou plusieurs cubes autour du point cliqué.

**3 outils sélectionnables (ordre logique) :**
- **Chasse** — retire un gros paquet de cubes, imprécis, pour dégrossir loin
  du trait.
- **Pointe** — retire un paquet moyen, pour approcher.
- **Ciseau** — retire 1 cube, précis, pour finir au trait.

**Force du coup** via curseur OU durée d'appui (bref = léger, maintenu = fort).
Cubes retirés = outil × force.

**3 jauges, mises à jour à CHAQUE coup, feedback couleur immédiat
(vert monte / rouge descend) :**
- **Matière restante** — ne fait que baisser, c'est le budget.
- **Proximité du trait** — monte en approchant, **CHUTE si on dépasse**
  (trop taillé).
- **Planéité** — monte avec le ciseau, plafonne avec la chasse.

**Fin :** bouton « Pierre finie » → fige l'état, affiche un score global
(ex. 84 %) + une phrase sur le point faible (« solide, mais un côté un peu
trop mangé »).

**Interdits :** son, menu, niveaux, sauvegarde, framework, 3D, rotation.

---

## Retour de partie n°2 → on change le VERBE

> « Ça reste un casse-brique, inspire-toi de Michelangelo: Stonemason Simulator. »

Deux tours de correction visuelle n'avaient rien changé — preuve que le problème
n'était pas la lecture mais **le geste lui-même**. Cliquer pour faire disparaître
un paquet de cubes *est* un casse-brique, quelle que soit la couleur.

Ce que fait réellement Michelangelo (vérifié, pas supposé) : c'est **voxel** et
**au clic**, décrit comme « *tap away* », « *shaving away at blocks of stone* »,
« meditative ». Donc la différence n'était pas clic-contre-glisser. Elle était
dans le **rapport coup / matière** :

| | Ma version 2 | Michelangelo |
|---|---|---|
| Rythme | ~50 coups isolés | des centaines de petits coups |
| Par coup | un paquet de 20-40 cubes disparaît | un **copeau** |
| Rituel | charger, viser, relâcher | on tape, en cadence |
| Sensation | démolir | raboter |

Je faisais **peu de coups qui suppriment de gros paquets**, avec une cérémonie
de charge à chaque fois. C'est la définition du casse-brique.

**Le nouveau verbe : on maintient et on balaie.** L'outil frappe en cadence
(3,6 coups/s pour la chasse, 11 pour le ciseau) et chaque coup **use** la
surface d'une fraction de rang. Un cube ne tombe que lorsqu'il a été
entièrement rongé, souvent après plusieurs passages. C'est le *passage* qui
creuse, plus le coup.

Conséquences :

1. **L'outil est posé à plat sur la face**, il n'attaque que la peau — le cube
   du dessus de chaque colonne de son empreinte. Il ne creuse plus jamais un
   trou sphérique dans la masse : il rabote une surface.
2. **Granularité sous-cube.** Un cube à demi rongé est dessiné *reculé* d'autant
   dans l'axe où on l'use. La surface s'enfonce par fractions au lieu de sauter
   d'un rang : on voit un copeau partir, pas une brique disparaître. C'est ça
   qui tue l'effet Lego.
3. **La cérémonie de charge est supprimée.** Plus de jauge de force, plus de
   « viser-charger-lâcher ». À la place, un chiffre de tailleur : **« reste à
   ôter : 1,4 rang »** sous l'outil. On mesure, puis on rabote.
4. **Le frisson devient continu.** Le ciseau ne prend que les bosses ; sur une
   face déjà dressée il n'a plus de bosse, alors il entame la face — 2,4× plus
   vite. Traîner 3 s de trop coûte 7 points, 8 s en coûtent 34. Ce n'est plus
   un pari à l'aveugle, c'est un « arrête-toi maintenant » qu'on sent monter.
5. **Le rythme est celui du métier** : ~300 à 500 coups de maillet, 70 à 90 s
   de contact. On tape, en continu, et la pierre sort.

### Ce qui reste ouvert : le son

Le brief interdit le son. Pour un jeu de taille c'est un handicap réel — la
moitié du plaisir du geste est dans le tac-tac du maillet, et Michelangelo s'en
sert massivement. Je ne l'ai pas ajouté (c'est un interdit explicite), mais si
le geste reste tiède après cette version, **c'est le premier levier à essayer**
avant de conclure quoi que ce soit sur le Proto 1.

---

## Retour de partie n°1 → refonte

> « Bof, c'est un jeu de casse-brique, et ce n'est pas assez clair sur la zone
> à conserver ; le modèle en pointillé ne sert pas. »

Diagnostic accepté, et la cause était unique : **la pierre était invisible.**
On cassait un mur opaque en espérant qu'elle soit dedans, et le trait en
pointillé flottait dans le vide sans se rattacher à la matière. D'où le
casse-brique : pas de pierre à dégager, juste des cubes à faire tomber.

Ce qui a changé :

1. **La pierre est visible dès la première seconde.** La gangue est devenue
   translucide ; la pierre visée est dessinée dessous, en dur. On ne devine
   plus, on voit ce qu'on dégage.
2. **Le pointillé est supprimé.** Il ne servait à rien, c'était juste.
   Le trait, maintenant, c'est la matière elle-même.
3. **Pierre FROIDE, gangue CHAUDE.** Deux familles de couleur qui ne se
   confondent jamais. Tout ce qui est froid est acquis, tout ce qui est chaud
   reste à ôter — lisible d'un coup d'œil à n'importe quel stade.
4. **La gangue dit son épaisseur** : brun sombre = épais (chasse), brun moyen =
   2 rangs (pointe), ambre vif = dernier rang (ciseau). La couleur dit quel
   outil prendre, sans jauge à lire.
5. **Le coup a du poids** : éclats de pierre qui volent avec la couleur de ce
   qu'on vient d'ôter, poussière, secousse.
6. **Moins de démolition, plus de taille.** Le bloc est passé de 1872 à 1452
   cubes. *(Le reste de ce point a été dépassé par le retour n°2 ci-dessus :
   ce n'est pas la taille des bouchées qui faisait le casse-brique, c'est le
   fait qu'il y ait des bouchées.)*
7. **Chaque bloc est un caillou différent** : les faces sortent de carrière
   irrégulières, tirées au sort. La 2e pierre n'est pas la 1re.

---

## Ce qui a été construit

`index.html` — un seul fichier, canvas 2D, zéro dépendance. Ouvrir dans un
navigateur, rien à installer.

- Bloc de carrière **11×11×12 = 1452 cubes**, pierre visée **7×7×9 = 441**,
  calée dans le coin arrière-bas : ses 3 faces à travailler sont donc toutes
  atteignables malgré la caméra fixe sans rotation. Les faces du bloc sont
  mangées irrégulièrement au tirage — chaque pierre est un caillou différent.
- **On voit la pierre à travers la gangue.** La pierre est dessinée en dur,
  froide ; la gangue par-dessus, chaude et translucide, teintée par ce qu'il
  reste à ôter dessous (brun sombre = épais, ambre vif = dernier rang). Un cube
  pris sous le trait laisse une **entaille rouge** qui ne s'efface pas.
- **L'empreinte de l'outil** est dessinée sur la face, à sa vraie largeur, et
  les colonnes qu'elle attaque s'allument. On voit *où* on rabote, pas *combien*
  on va faire sauter.
- **On maintient et on balaie** : l'outil frappe en cadence et use la surface
  par fractions de rang. Éclats, poussière et secousse à chaque coup de maillet.
- **« Reste à ôter : 1,4 rang »** sous l'outil : le tailleur mesure avant de
  frapper. C'est ce chiffre qui a remplacé le trait tracé.
- Un éclat qui n'est plus porté par le sol **tombe** et ne revient pas.

### Écart assumé au brief : la force, et le ciseau

Le brief demandait « Ciseau — retire 1 cube » et « force du coup via curseur ou
durée d'appui ». Les deux ont sauté, pour la même raison : ils faisaient du
casse-brique.

- **Le ciseau ne retire pas 1 cube.** Testé : finir cube par cube demandait 576
  clics. Et c'est faux par rapport au métier — un ciseau ne pique pas un point,
  il court le long de la face et rase ce qui dépasse. Il ne touche donc qu'aux
  bosses ; sur une face déjà dressée, il n'a plus rien à raser et entame la face.
- **La force n'est plus un curseur qu'on charge.** La charge faisait de chaque
  coup un événement — exactement le rituel du casse-brique. La dose, maintenant,
  c'est **combien de temps on reste au même endroit** : dwell = profondeur.
  C'est plus analogique, et c'est le vrai geste.

### Ce que le moteur mesure (parties types rejouées hors navigateur)

| Manière de jouer | Temps | Coups | Trait | Planéité | Score |
|---|---|---|---|---|---|
| Chasse partout, jusqu'au bout | 63 s | 225 | 22 | 67 | **45** |
| Bon outil au bon moment | 67 s | 288 | 69 | 77 | **76** |
| Prudent (ciseau dès 2 rangs) | 80 s | 492 | 100 | 97 | **99** |
| …+ 3 s de ciseau de trop | 83 s | 525 | 89 | 92 | **92** |
| …+ 5 s encore | 88 s | 580 | 51 | 81 | **65** |

Trois choses à retenir :
1. **La progression chasse → pointe → ciseau est obligatoire**, pas décorative :
   45 contre 99 pour le même temps de travail.
2. **Aller plus fin coûte du temps** (80 s contre 67) et rapporte de la qualité.
   C'est l'arbitrage central, et il est continu.
3. **S'attarder coûte cher** : 3 s de trop, 7 points ; 8 s, 34 points. Le frisson
   du dernier coup n'est plus un pari à l'aveugle mais une pression qui monte.
   Reste à savoir s'il se *ressent* — seule une partie le dira.

Une pierre demande **300 à 500 coups de maillet**, 70 à 90 s de contact.

---

## Verdict (à remplir APRÈS avoir joué)

- Envie d'une 2e pierre sans se forcer ? →
- Frisson du dernier coup présent ? →
- Le dosage force/outil crée-t-il un vrai arbitrage, ou on tape au hasard ? →
- Décision : passer au Proto 2 ? OUI / NON →
- Notes libres :
