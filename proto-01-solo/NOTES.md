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

## Retour de partie n°4 → l'outil touche ce qu'il touche, et l'arête casse

> « La sélection sélectionne parfois des cubes absolument à l'opposé du curseur ;
> il faut que ce soit juxtaposé au curseur et plus ou moins gros selon l'outil.
> Enlève le mesh, et un coup c'est un trou plus ou moins large concentrique avec
> pour épicentre le curseur. Si c'est au bord d'une arête, faut voir ce qu'il se
> passe réellement dans le réel et reproduire. »

### Le bug : l'outil mordait à l'opposé

Vrai bug, et sa cause était bête. Pour chaque colonne autour du curseur,
l'empreinte cherchait le cube le plus extérieur **sur toute la profondeur du
bloc**. Près d'une arête, une colonne voisine a sa surface 7 rangs en arrière —
l'outil allait donc la chercher là-bas.

Corrigé par la physique du geste : **l'outil est posé sur la pierre, il ne
touche que ce qui affleure sous lui.** Une colonne dont la surface est à plus de
2 rangs du point de contact n'est pas sous le fer, donc on n'y touche pas.

### Le mesh, et le coup comme cratère

- **Le maillage est supprimé.** Plus une seule arête de cube dessinée. La forme
  se lit maintenant par l'ombre : une colonne encaissée entre des voisines plus
  hautes s'assombrit. Résultat, la surface lit comme de la roche et non comme un
  mur de briques — c'était sans doute la moitié de l'effet casse-brique.
- **Chaque outil a son profil de cratère.** Un coup n'use pas uniformément son
  empreinte : il creuse fort au centre et s'éteint au bord, donc il ouvre un
  **trou concentrique dont l'épicentre est le curseur**. La chasse ouvre un
  cratère large, le ciseau rase à plat — c'est le geste réel de chacun.
- **L'aperçu montre ce profil**, pas juste la surface touchée : on voit le creux
  qu'on va faire avant de le faire.
- Les trois empreintes sont franchement différentes (2,6 / 1,55 / 1,15 colonnes)
  et le cercle dessiné sur la pierre est à la vraie échelle de l'outil.

### L'arête : ce qui se passe pour de vrai

Dans le métier, frapper près d'une arête vive, c'est l'**épaufrer** : la pierre
n'est plus épaulée de ce côté, elle ne résiste pas, et elle part en écaille bien
au-delà du coup. C'est la faute classique, et la raison pour laquelle on
n'approche jamais une arête à la chasse — on la finit au ciseau, en frappant
vers la masse et non vers le vide.

C'est reproduit tel quel : chaque colonne compte ses voisines manquantes, et
l'usure y est multipliée d'autant (×1,7 à la chasse, ×0,3 au ciseau). Effet
émergent vérifié — le bot, qui ne sait pas ménager les arêtes, ressort une
pierre dont **tous les angles sont épaufrés** et le reste intact. Le défaut
apparaît exactement là où il apparaît dans la réalité.

---

## Retour de partie n°3 → la pierre se DÉCOUVRE

> « J'ai beaucoup [aimé]. Par contre ne montre pas la partie blanche ni jaune,
> faut le découvrir au fur et à mesure ; dans un premier temps la partie marron
> à dégrossir. »

C'est l'inverse exact du tour n°2 — et c'est juste. Au tour n°2, la gangue était
devenue translucide *parce que le geste était illisible* : il fallait bien un
garde-fou. Maintenant que le geste marche, ce rayon X ne protège plus rien, il
**vole la découverte**. Or la découverte, c'est tout le sujet : la pierre est
déjà dans le bloc, on ne la fabrique pas, on la trouve.

Ce qui a permis d'enlever le rayon X sans revenir au problème du tour n°1
(« pas assez clair sur la zone à conserver ») : **le chiffre sous l'outil**. Il
n'existait pas au tour n°1. Un tailleur ne consulte pas un plan, il sonde son
ouvrage au point de travail. C'est maintenant l'instrument principal, et il est
affiché à côté du fer, sur la pierre, pour ne pas quitter l'ouvrage des yeux.

- **Le bloc est opaque.** Un cube ne dit ce qu'il est que lorsqu'il est en
  surface. Rien ne se devine à travers la matière.
- **Le départ est entièrement brun.** L'érosion de carrière s'arrête au 3e rang :
  au premier coup d'œil il n'y a que de la gangue épaisse à dégrossir. Ni
  pierre, ni dernier rang.
- **Le bloc s'ouvre par paliers** : brun épais → brun → ambre → la pierre. Ces
  couleurs ne sont plus une carte donnée d'avance, ce sont des paliers
  découverts un par un, exactement là où on a travaillé.
- Bloc passé à **11×11×13** pour que les 3 faces aient 4 rangs de gangue : de
  quoi garder un bloc irrégulier *et* un départ uniformément brun.

### Et une incohérence corrigée au passage

Les jauges ne comptaient que les cubes **entièrement** tombés, alors que
l'usure est fractionnaire : l'aiguille avançait par à-coups. Elles comptent
maintenant les copeaux. Conséquence directe : **on voit la jauge du trait
descendre dès qu'on entame la pierre**, sans attendre d'avoir perdu un cube —
et le cube entamé rougit progressivement. Le « arrête-toi » se voit venir.

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

- Bloc de carrière **11×11×13 = 1573 cubes**, pierre visée **7×7×9 = 441**,
  calée dans le coin arrière-bas : ses 3 faces à travailler sont donc toutes
  atteignables malgré la caméra fixe sans rotation. Les faces du bloc sont
  mangées irrégulièrement au tirage — chaque pierre est un caillou différent.
- **Le bloc est opaque, la pierre se découvre.** Au départ, rien que du brun à
  dégrossir. La surface mise à nu s'ouvre ensuite par paliers — brun épais,
  brun, ambre (dernier rang), puis la pierre, froide. Un cube de pierre entamé
  **rougit** à mesure qu'on le ronge, et l'entaille ne s'efface pas.
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
| Chasse partout, jusqu'au bout | 65 s | 233 | 15 | 73 | **42** |
| Chasse jusqu'au 2e rang | 62 s | 263 | 17 | 74 | **44** |
| Prudent (ciseau dès le 3e rang) | 66 s | 390 | 91 | 95 | **94** |
| …+ 3 s de ciseau de trop | 69 s | 423 | 49 | 88 | **66** |
| …+ 5 s encore | 74 s | 478 | 0 | 75 | **34** |

Trois choses à retenir :
1. **La progression chasse → pointe → ciseau est obligatoire**, pas décorative :
   42 contre 94 pour le même temps de travail.
2. **Le gros outil près du trait ne pardonne pas** — surtout depuis l'épaufrure :
   descendre à la chasse jusqu'au 2e rang ne gagne rien et ruine les arêtes.
3. **S'attarder coûte cher** : 3 s de trop, 28 points ; 8 s, 60 points. Et comme
   les jauges comptent les copeaux, ça se voit venir coup par coup.

Une pierre demande **300 à 500 coups de maillet**, 70 à 90 s de contact.

---

## VERDICT — rendu le 29/08/2026

> « Il y a un aspect redondant. À ta question "ai-je envie de tailler une
> nouvelle pierre" : **non**. »

- **Envie d'une 2e pierre sans se forcer ? → NON.**
- Le geste lui-même : jugé bon au tour n°4 (« c'est bien », « encourageant »).
  Ce n'est donc **pas** le geste qui est fade — c'est sa **répétition**.
- Décision : **le Proto 1 ne passe pas son propre juge.**

### D'où vient la redondance — analyse honnête

Quatre causes, de la plus superficielle à la plus profonde. Les trois dernières
sont **structurelles** : aucune n'était réparable par du réglage.

1. **Un seul modèle, à vie.** La pierre visée est toujours le même 7×7×9, dans le
   même coin. La 2e pierre pose exactement le problème de la 1re. Le bloc de
   carrière varie, mais c'est cosmétique : le travail à faire est identique.
   → À comparer avec la référence : Michelangelo va d'un pilier au David. Le
   « oui » qu'il a prouvé commercialement portait sur une suite de tâches
   **différentes**. Mon Proto 1 en est une version dégénérée.

2. **Trois faces, un problème répété trois fois.** Même *à l'intérieur* d'une
   pierre, les faces +x, +y et +z posent la tâche identique. La redondance
   commence donc avant la 2e pierre.

3. **Une seule stratégie gagnante, connue dès la 1re pierre.** C'est mon erreur
   de conception, et mes propres mesures la prouvent : une ligne à 94, toutes les
   autres entre 42 et 44. J'ai réglé le jeu pour que « la bonne méthode soit
   récompensée » — et j'ai obtenu « il n'existe qu'une méthode ». Une table de
   stratégies saine montrerait plusieurs lignes viables et des arbitrages ; la
   mienne montre une réponse juste et des fautes. Ce n'est pas un skill, c'est
   une exécution.

4. **Rien ne survit à la pierre.** On finit, on a un score, il s'évapore. Or le
   DESIGN (§5) dit que le premier battement de cœur *doit* être la fierté du
   geste — mais la fierté d'une pierre qui ne va nulle part n'est pas de la
   fierté. Dans le vrai jeu, la pierre part dans le château et porte la marque
   du tailleur. Le Proto 1 n'a **pas d'aval**.

### Ce que ce NON prouve, et ce qu'il ne prouve pas

**Il prouve** que le geste solo, seul, ne porte pas le jeu. C'est exactement le
risque que le DESIGN avait nommé (§7, « déficit d'anticipation ») et la raison
pour laquelle le brief disait « ne pas s'y attarder ».

**Il ne prouve pas** que le geste est mauvais — le retour du tour n°4 dit
l'inverse. Trois des quatre sources d'intérêt de Cairn (la variété venue de
l'héritage, la gratitude en aval, la contemplation du château) sont **absentes
par construction** du Proto 1. Le NON peut être la réaction à leur absence
plutôt qu'au geste.

⚠️ **Piège à éviter** : c'est exactement le raisonnement qu'un créateur se tient
pour ne pas entendre un NON. Il ne doit pas servir à passer outre le protocole.
Il doit servir à **choisir le bon test suivant** — voir la décision ci-dessous.

- Décision : passer au Proto 2 ? → **EN ATTENTE** (voir ci-dessous)
- Notes libres :
