# Proto 2 — La pierre récupérée (juge de paix)

## Question testée (LE pari central du jeu)
**Hériter de la pierre à moitié taillée d'un inconnu, et devoir « faire avec »,
a-t-il un sel que tailler de zéro n'a pas ?**

> ⚠️ Ne coder ce proto QUE si le Proto 1 est concluant. Sinon on teste un étage
> sur des fondations non vérifiées.

---

## Brief pour Claude Code

Reprends **exactement le moteur de taille du Proto 1** (mêmes cubes iso, mêmes
outils, mêmes jauges, même vue fixe iso 2D). On **copie** le code, on ne
factorise pas — vitesse avant élégance.

**Seule différence, et c'est tout le sujet :** la pierre ne démarre pas brute.
Elle démarre **à moitié taillée, comme abandonnée par un autre joueur**,
imparfaitement.

**3 états hérités, codés en dur, sélectionnables au lancement :**
- **« Le propre »** — bien entamé, dans les règles, facile à finir.
- **« Le tordu »** — quelqu'un a trop mangé d'un côté ; la matière manque, il
  faut composer avec le manque et viser un résultat décalé mais valable.
- **« Le bâclé »** — dégrossi n'importe comment, surface en dents de scie, gros
  travail de rattrapage.

**Les cubes déjà retirés par « l'autre » doivent être VISIBLES comme tels** :
les cubes qu'il a touchés / les bords qu'il a laissés sont marqués (teinte ou
hachure différente de mes propres coups), pour que je voie le travail hérité et
m'y adapte.

**Le score récompense le rattrapage :** finir « Le tordu » ou « Le bâclé »
correctement doit rapporter **PLUS** que finir « Le propre » (c'est plus dur).

**Interdits :** identiques au Proto 1 (son, menu, niveaux, sauvegarde,
framework, 3D, rotation).

---

## Ce qui a été construit

`index.html` — moteur du Proto 1 **copié tel quel**, sans factorisation (le brief
l'exige : vitesse avant élégance). Ouvrir dans un navigateur, choisir une des
trois pierres.

### Décision d'expérience : mêmes cotes pour les trois

La commande et les cotes sont **identiques** pour les trois états hérités
(pierre de parement, 7×7×9). Sinon la comparaison « Le tordu » contre « Le
propre » serait polluée par la variation de taille, et le juge de paix ne
vaudrait plus rien. Seule change la façon dont l'inconnu s'y est pris.

### L'autre tailleur n'est pas dessiné, il est **rejoué**

Les trois états ne sont pas des motifs codés à la main : on **rejoue quelqu'un**
sur le bloc, avec ses vrais outils et son tempérament. Ce qu'il laisse est donc
ce qu'un coup de chasse laisse vraiment. C'est ce qui permet de *lire* ce qu'il
a fait, et pas seulement de le constater.

- **Le propre** — chasse, pointe, puis il a dressé au ciseau avant de s'arrêter.
  Surfaces régulières, de la marge partout. Il n'y a plus qu'à finir.
- **Le tordu** — le même bon travail, puis il a insisté au même endroit sur une
  face et l'a mangée de deux rangs.
- **Le bâclé** — la chasse partout, jamais d'autre outil, plus quelques coups de
  trop lâchés au hasard. Creux, bosses, rien de plan.

### Sa trace se voit — c'est la condition explicite du brief

Une taille fraîche est vive ; une taille qui a pris l'air est mate et grise.
Chaque cube porte donc une main : **0 intact, 1 l'autre, 2 moi**. Au départ tout
est à lui — et à mesure qu'on reprend, sa surface terne cède la place à la
nôtre, vive. On voit littéralement sa part reculer.

### « Faire avec » : la pierre est déclassée, pas jetée

Sur « Le tordu », la cote annoncée **n'est plus tenable** : il a mangé la
matière. Le jeu calcule ce que la pierre peut encore donner et déclasse la
commande (7×7×9 → 5×7×9). Il faut alors couper le reste à cette mesure-là.
C'est le geste du métier : on ne jette pas, on sort la plus grande pierre saine
qui reste dedans. Le déclassement est **borné à deux cotes** — au-delà ce n'est
plus un rattrapage, c'est un caillou (DESIGN §2, « contrainte sans blocage »).

### Le rattrapage se paie

| Héritage | Reste à ôter au départ | Travail soigné | …bâclé sans soin |
|---|---|---|---|
| Le propre | 107 | **95 pts** | 77 |
| Le tordu (déclassé) | 261 | **112 pts** ×1,18 | 87 |
| Le bâclé | 205 | **108 pts** ×1,12 | 79 |

L'exigence du brief est tenue : finir « Le tordu » ou « Le bâclé » correctement
rapporte plus que finir « Le propre ». Et elle n'est pas contournable — bâcler
une pierre difficile (87) reste sous un travail soigné sur la facile (95).

La note n'est **pas plafonnée à 100** : une pierre rattrapée peut valoir plus
que la commande n'exigeait. Plafonner écraserait justement ce que le brief
demande de rendre visible.

Compter environ **100 à 120 s de taille par pierre** — les trois se font en une
séance, ce qu'il faut pour comparer à chaud.

### Écarts au brief, assumés

- **Le moteur est celui du Proto 1 d'aujourd'hui**, donc il embarque la commande,
  la tolérance à la cote et le journal — tout ce que le Proto 1 a gagné en cours
  de route. C'est bien « exactement le moteur du Proto 1 », mais sa version
  finale, pas celle du brief d'origine.
- **Pas de hachure** pour marquer la main de l'autre, mais un contraste
  mat/vif. Une hachure sur des cubes de 40 px se lit mal ; la fraîcheur de
  coupe est plus juste et plus lisible.

---

## Le verdict qui décide de TOUT le projet

**« Le tordu » doit être PLUS prenant que « Le propre ».**

- Si OUI → le pari est gagné. L'imperfection héritée crée de la valeur. Cairn a
  un cœur que personne n'a. On peut construire les étages.
- Si NON (« Le propre » plus agréable que « Le tordu ») → l'hypothèse centrale
  du jeu est **fausse**. Il faut la revoir AVANT de construire quoi que ce soit.
  Mieux vaut le découvrir ici, sur un proto moche, que sur un jeu fini.

---

## Verdict (à remplir APRÈS avoir joué)

- « Le tordu » vs « Le propre » : lequel est le plus prenant ? →
- Voir les traces de l'autre change-t-il le ressenti, ou c'est indifférent ? →
- Ressent-on un mini-lien / une adresse à l'inconnu ? (« bon, je récupère ») →
- « Le bâclé » : rattrapage satisfaisant ou juste pénible ? →
- Décision : l'hypothèse tient ? OUI / NON →
- Notes libres :
