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

## Ce qui a été construit

`index.html` — un seul fichier, canvas 2D, zéro dépendance. Ouvrir dans un
navigateur, rien à installer.

- Bloc brut **12×12×13 = 1872 cubes**, pierre visée **7×7×9 = 441** (calée dans
  le coin arrière-bas, donc ses 3 faces à travailler sont toutes visibles sous
  la caméra fixe). Croûte à faire sauter : **1431 cubes**.
- **Le trait** est gravé dans la pierre : l'arête ocre de la pierre visée,
  visible *à travers* la matière. La croûte est sombre, son dernier rang avant
  le trait est ambré, la pierre à garder est claire, et un cube passé sous le
  trait laisse une **entaille rouge** qui ne s'efface pas.
- **Aperçu du coup** : les cubes que le coup emportera s'allument en blanc, et
  l'anneau de charge sous le curseur passe du pâle au rouge. On voit la morsure
  grossir tant qu'on maintient — c'est là que se joue « je lâche ou j'attends ».
- Un éclat qui n'est plus porté par le sol **tombe** et ne revient pas.

### Écart assumé au brief : le ciseau *dresse*, il ne retire pas 1 cube

Le brief disait « Ciseau — retire 1 cube ». Testé, mesuré : finir la pierre
cube par cube demandait **576 clics**. Injouable, et faux par rapport au métier
— un ciseau ne pique pas un point, il court le long de la face et **abat ce qui
dépasse**.

Le ciseau abat donc, dans son disque, tout ce qui dépasse jusqu'au creux le plus
bas ; il ne touche pas au creux lui-même. **Et si la face est déjà dressée, il
n'a plus de bosse à prendre : il descend d'un rang entier.** C'est exactement
le « coup de trop » que le brief cherchait, et il coûte cher. Le reste est
inchangé : la chasse et la pointe arrachent bien une bouchée sphérique.

### Ce que le moteur mesure (vérifié hors navigateur, 3 parties types)

| Manière de jouer | Coups | Trait | Planéité | Sous le trait | Score |
|---|---|---|---|---|---|
| Chasse à fond partout | 32 | 0 | 54 | 259 | **36** |
| Tailleur sans marge de sécurité | 54 | 0 | 64 | 112 | **39** |
| Prudent (garde 1 cube de marge) | 91 | 66 | 85 | 38 | **75** |
| …le même, + 5 coups de trop | 96 | 24 | 78 | 85 | **51** |

Trois choses à retenir de ce tableau :
1. Taper fort et vite est **puni** — la progression chasse → pointe → ciseau
   n'est pas décorative, elle est obligatoire.
2. Lire l'aperçu ne suffit pas : l'imprécision des outils oblige à **garder une
   marge**. C'est ça, le skill.
3. **Cinq coups de trop coûtent 24 points.** Le frisson du dernier coup est bien
   dans le modèle. Reste à savoir s'il se ressent — c'est la question ci-dessous.

Une bonne pierre se taille en **~80-90 coups**, soit quelques minutes.

---

## Verdict (à remplir APRÈS avoir joué)

- Envie d'une 2e pierre sans se forcer ? →
- Frisson du dernier coup présent ? →
- Le dosage force/outil crée-t-il un vrai arbitrage, ou on tape au hasard ? →
- Décision : passer au Proto 2 ? OUI / NON →
- Notes libres :
