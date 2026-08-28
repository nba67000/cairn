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
   qu'on vient d'ôter, poussière, et secousse proportionnelle à la force.
6. **Moins de démolition, plus de taille.** Le bloc est passé de 1872 à 1452
   cubes et la chasse arrache désormais un éclat *large et peu profond* (le vrai
   geste) au lieu d'un trou rond. Une pierre se fait en **40 à 60 coups** au
   lieu de ~90, et la plupart de ces coups sont des décisions près du trait,
   pas du déblaiement.
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
- **Aperçu du coup** : les cubes que le coup emportera s'allument en blanc, et
  l'anneau de charge sous le curseur passe du pâle au rouge. On voit la morsure
  grossir tant qu'on maintient — c'est là que se joue « je lâche ou j'attends ».
- **Le coup a du poids** : éclats qui volent (à la couleur de ce qui vient
  d'être ôté), poussière, secousse proportionnelle à la force.
- Un éclat qui n'est plus porté par le sol **tombe** et ne revient pas.

### Écart assumé au brief : le ciseau *dresse*, il ne retire pas 1 cube

Le brief disait « Ciseau — retire 1 cube ». Testé, mesuré : finir la pierre
cube par cube demandait **576 clics**. Injouable, et faux par rapport au métier
— un ciseau ne pique pas un point, il court le long de la face et **rase ce qui
dépasse**.

Le ciseau se règle donc sur le cube où on le **pose** : tout ce qui dépasse de
ce niveau, dans son disque, saute. **Et s'il n'y a plus rien à raser, c'est le
cube visé lui-même qui part.** C'est exactement le « coup de trop » que le brief
cherchait — mais un coup de trop *volontaire*, choisi en connaissance de cause,
pas un dérapage subi.

La chasse a elle aussi bougé : elle arrache un éclat **large et peu profond**
au lieu d'une sphère. C'est le vrai geste, et ça la rend enfin utilisable pour
balayer une face sans plonger droit dans la pierre.

### Ce que le moteur mesure (vérifié hors navigateur, parties types rejouées)

| Manière de jouer | Coups | Trait | Planéité | Sous le trait | Score |
|---|---|---|---|---|---|
| Chasse à fond partout, sans regarder | 31 | 0 | 46 | 240 | **34** |
| Tailleur qui lit l'aperçu | 53 | 90 | 96 | 11 | **94** |
| Prudent (garde 1 cube de marge) | 99 | 90 | 95 | 11 | **89** |
| …le même, + 5 coups de trop | 104 | 57 | 87 | 48 | **69** |

Trois choses à retenir :
1. Taper fort et vite est **puni** (34) — la progression chasse → pointe →
   ciseau n'est pas décorative, elle est obligatoire.
2. **Lire l'aperçu est le skill.** Le prudent qui double le nombre de coups
   n'obtient pas une meilleure pierre : il perd juste sur l'économie du geste.
3. **Cinq coups de trop coûtent 20 à 25 points.** Le frisson du dernier coup est
   dans le modèle, et c'est une décision, pas un accident. Reste à savoir s'il
   se *ressent* — c'est la question ci-dessous, et seule une partie y répond.

Une pierre se taille en **40 à 60 coups**, quelques minutes.

---

## Verdict (à remplir APRÈS avoir joué)

- Envie d'une 2e pierre sans se forcer ? →
- Frisson du dernier coup présent ? →
- Le dosage force/outil crée-t-il un vrai arbitrage, ou on tape au hasard ? →
- Décision : passer au Proto 2 ? OUI / NON →
- Notes libres :
