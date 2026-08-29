# Proto 3 — Le relais : la pierre passe de main en main

> ⚠️ **Ce brief n'est pas de toi.** Les protos 1 et 2 avaient leur brief écrit
> d'avance ; celui-ci, je l'ai rédigé. Il est donc à discuter, pas à appliquer.
> Ce qui suit est ma lecture de l'inconnue restante — corrige-la si elle est
> fausse, c'est le moment.

## Question testée (une seule)

**Quand l'autre est une vraie personne, est-ce que ça change quelque chose ?**

Le Proto 2 a prouvé qu'hériter d'une pierre imparfaite est prenant — mais
l'inconnu y était un **bot**. Toute la thèse sociale du `DESIGN.md` §1, la
*solidarité d'inconnus*, repose sur une hypothèse jamais testée : qu'une main
humaine derrière la pierre ajoute quelque chose qu'un algorithme n'ajoute pas.

C'est aujourd'hui **le seul vrai risque du projet**. Tout le reste est du
confort à côté.

Juge honnête (anti-biais de créateur) :
- Recevoir la pierre d'une **vraie** personne fait-il un effet différent du
  Proto 2, ou est-ce que ça revient au même ?
- Est-ce que je taille **différemment** en sachant que quelqu'un va reprendre
  derrière moi ?
- Quand la pierre me **revient** travaillée par l'autre, est-ce que je ressens
  quelque chose — curiosité, agacement, gratitude, n'importe quoi de non neutre ?
- Est-ce que j'ai envie de **soigner le passage** (laisser une pierre propre)
  ou de me débarrasser du sale boulot ?

## Le dispositif

Pas de serveur, un seul fichier : la pierre voyage dans un **code à
copier-coller**. On le passe par le canal qu'on veut — message, mail, papier.

C'est asynchrone, c'est à distance, et c'est fidèle au `DESIGN.md` §6 : aucune
messagerie libre entre inconnus, seulement un **objet défini par le jeu**. Le
seul mot qui voyage est la **marque de tâcheron** — la signature de la main.

1. **A** prend un bloc neuf, taille ce qu'il veut, appuie sur « Passer la
   pierre », copie le code, l'envoie à **B**.
2. **B** colle le code, reçoit la pierre exactement là où A l'a laissée, voit
   *quels cubes A a touchés*, continue, et repasse.
3. Et ainsi de suite. La pierre peut revenir à A : il verra alors ce que B a
   fait de son travail.

## Ce que le proto NE teste pas

- L'anonymat réel : on sait forcément à qui on envoie le code. Le Proto 3
  teste **la main humaine**, pas l'inconnu. C'est une étape, pas la fin.
- Le château, la chaîne de métiers, la gratitude différée. Hors sujet ici.

## Interdits

Identiques : son, menu, niveaux, sauvegarde, framework, 3D, rotation.
Et un de plus, propre à ce proto : **aucun score par joueur.** La pierre a UNE
note, celle de la pierre, faite à plusieurs. Le `DESIGN.md` §1 est explicite :
coopératif, sans compétition. Comparer A et B tuerait le sujet.

---

## Ce qui a été construit

`index.html` — moteur du Proto 2, un seul fichier. Trois portes :
**prendre un bloc neuf**, **recevoir une pierre** (coller un code), et à tout
moment **passer la pierre** (produire un code).

- **La pierre tient dans un code** d'environ **1 700 caractères** : ses cotes,
  sa matière, l'usure de chaque cube, et quelle main a touché quoi. Compressé
  par plages, parce qu'une pierre est faite de longues zones identiques.
- **Chaque cube retient sa main.** La tienne est vive — coupe fraîche ; celles
  des autres sont mates, chacune avec sa nuance. On lit qui a fait quoi.
- **La part de chaque main est affichée** en pourcentage de la surface visible.
  C'est le repli robuste : même si quelqu'un repasse partout et efface la
  texture, la chaîne dit ce que chacun a laissé.
- **Tu retrouves ta place dans la chaîne.** Si la pierre te revient, tu redeviens
  la main que tu étais — la chaîne se lit « Nico → Marie → Nico », pas trois
  inconnus.
- **La pierre se déclasse** si une main a trop mangé, comme au Proto 2, et le
  déclassement voyage avec elle.
- **UNE note, pour la pierre.** Jamais par joueur. Elle porte une prime quand la
  pierre a été rattrapée et quand elle a porté plusieurs mains — le `DESIGN.md`
  §2 dit que le château est l'archive des gestes ; une pierre à plusieurs mains
  vaut plus qu'une pierre solitaire.

### Ce que l'aller-retour a révélé

Deux défauts que seul un vrai A → B → A pouvait montrer :

1. **La jauge repartait de zéro** à la réception : A passait une pierre à 92 %
   de la cote, B la recevait à 0 %. Le travail déjà fourni n'était pas compté.
   → La mesure du travail dû **voyage désormais avec la pierre**. C'est UNE
   pierre, pas deux chantiers.
2. **La main du premier disparaissait** dès que le second repassait partout.
   C'est en partie juste — redresser une face efface les marques de l'autre —
   mais ça vidait le proto de son sujet. → D'où l'affichage de la **part de
   chaque main**, qui survit à tout.

Vérifié : la pierre revient **identique au cube près** après le voyage, les deux
mains restent lisibles, et A retrouve sa place au lieu d'en créer une nouvelle.

### Limites connues

- **Le code fait ~1 700 caractères.** Ça passe par mail ou par note, c'est long
  pour un chat. Compressible davantage si ça gêne à l'usage.
- **On sait à qui on envoie.** Ce proto teste *la main humaine*, pas l'inconnu
  (voir plus haut). L'anonymat est une couche au-dessus.

---

## Verdict (à remplir APRÈS avoir joué à deux)## Verdict (à remplir APRÈS avoir joué à deux)

- Recevoir d'une vraie personne : différent du Proto 2 ? →
- Ai-je taillé différemment en sachant qu'on reprendrait derrière moi ? →
- Le retour de la pierre travaillée par l'autre : ressenti ou indifférent ? →
- Envie de soigner le passage, ou de refiler le sale boulot ? →
- Décision : la thèse sociale tient-elle ? OUI / NON →
- Notes libres :
