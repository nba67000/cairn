# Proto 3 · Étape B — La boucle à deux vrais joueurs (anonyme)

## Pré-requis
- Proto 2 : pari central validé ✅ (le tordu/bâclé bat le propre)
- Proto 3 · Étape A : validée ✅ (recevoir une pierre oubliée garde du sel ;
  déposer « pour un autre » change le soin)

## Ce que l'étape A ne pouvait PAS tester
L'étape A n'avait qu'un seul humain : ton oubli *imitait* l'inconnu, il ne le
*remplaçait* pas. Ton toi-d'hier taille comme toi ; un vrai inconnu taille
**autrement**. C'est cette altérité réelle qu'on teste ici, et elle seule
valide (ou non) la brique sociale du jeu.

## Question testée (une seule)
**Le sel du Proto 2 survit-il quand c'est un VRAI inconnu qui a taillé la pierre
que je reçois — et est-ce que bien faire "pour quelqu'un" a un poids réel ?**

---

## ⚠️ Invariants NON NÉGOCIABLES (source des dérapages précédents)

Une première tentative avait fait une étape B **nominative** où les joueurs se
choisissent. C'est un AUTRE jeu, écarté. Ces trois règles viennent du DESIGN
(section Sécurité) et ne sont pas des détails d'implémentation — elles sont la
nature même de Cairn :

1. **Anonyme.** Le joueur ne voit JAMAIS qui a taillé la pierre qu'il reçoit.
   Aucun pseudo, aucun identifiant, aucune marque nominative transportée avec la
   pierre.
2. **Non choisi.** Le joueur ne choisit JAMAIS de qui il reçoit. Tirage à
   l'aveugle dans un pot commun. Pas de liste d'amis, pas d'invitation, pas de
   sélection de partenaire, pas de profils à parcourir.
3. **Pas d'adresse à une personne.** Aucun canal ne permet d'envoyer quoi que
   ce soit à quelqu'un de précis. **Le seul lien autorisé entre deux joueurs
   est la pierre elle-même.**

> Test de conformité : si à un moment un joueur peut *désigner* un autre joueur
> (le choisir, le nommer, lui parler), c'est hors-cadre → refaire.

---

## Principe : tester le RESSENTI avant la plomberie

On ne monte PAS de backend pour la première itération. La question est « un
vrai inconnu garde-t-il le sel », c'est un test de ressenti, pas de technique.

### Itération B1 — copier-coller manuel (à faire en premier)
Le pot partagé est... deux humains et un presse-papier.

1. Le moteur exporte l'état d'une pierre taillée à moitié en une **chaîne de
   texte** (un blob : JSON compact ou base64 encodant la grille de cubes +
   quelles cellules ont été retirées).
2. Joueur 1 taille à moitié, clique « Exporter », copie le blob, l'envoie à
   Joueur 2 **par n'importe quel canal** (SMS, chat) — hors du jeu, donc
   l'anonymat du *jeu* est préservé même si dans ce test bricolé les deux se
   connaissent (c'est un test de ressenti, pas la vraie boucle).
3. Joueur 2 clique « Importer », colle le blob, **reçoit la pierre à moitié
   taillée d'un vrai autre humain**, la finit. Réciproquement.

**Ce qu'on observe :** quand tu importes le blob et que la pierre apparaît —
taillée par une vraie autre personne, à sa façon, pas la tienne — est-ce que le
« bordel, il l'a commencée comme ça, bon je compose » du Proto 2 est **toujours
là, voire plus fort** parce que c'est réel ? Ou est-ce que ça retombe à plat ?

> C'est LE moment de vérité. Import du blob → première pierre d'un vrai inconnu.
> Note ta réaction à chaud, avant de rationaliser.

### Itération B2 — pot partagé réel (seulement si B1 a du sel)
Si et seulement si B1 procure l'étincelle, on remplace le copier-coller par un
vrai pot commun anonyme :
- Un stockage partagé minimal : un petit JSON hébergé (gist, jsonbin, un mini
  serveur Node local sur le réseau, un Supabase gratuit — au plus simple).
- Dépôt : la pierre à moitié taillée part dans le pot, **sans aucune donnée
  d'auteur**.
- Retrait : tirage aléatoire d'une pierre du pot, jamais la sienne si évitable,
  jamais choisie.
- Le panneau affiche seulement, comme en étape A : « une main avant toi — X% ».

Toujours : anonyme, non choisi, pas d'adresse. Le blob ne transporte que la
**géométrie de la pierre**, jamais d'identité.

---

## Contraintes techniques (identiques aux protos précédents)
- Iso 2D façon Age of Empires, vue fixe, pas de 3D, pas de rotation.
- Réutiliser / copier le moteur de taille des protos 1-2. Ne pas le réécrire.
- Zéro fioriture : pas de son, pas de menu, pas de comptes, pas de graphismes
  léchés.
- B1 : un seul fichier HTML avec boutons Exporter / Importer + zone de texte
  pour le blob.
- Le blob doit être **auto-suffisant** (contenir toute la géométrie pour
  reconstruire la pierre) et **anonyme** (zéro champ auteur/pseudo/id).

---

## Le verdict qui débloque le « vrai jeu »

- **B1 a du sel ?** (la pierre d'un vrai inconnu garde/amplifie le « faire
  avec ») → monter B2 (pot réel), confirmer, puis la **brique sociale est
  validée**. On peut ENFIN penser « vrai jeu » : d'abord la **chaîne de métiers**
  (tailleur → maçon, où ma pierre devient la matière d'un AUTRE métier), PUIS
  seulement la présentation (3D, château qui monte).
- **B1 retombe à plat ?** (le sel s'évapore avec un humain réel) → on l'apprend
  ici, pas sur 6 mois de dev. Pistes à creuser AVANT d'aller plus loin :
  - voit-on assez la main de l'autre ? (rendre ses coups plus lisibles)
  - le rattrapage rapporte-t-il assez ? (récompense du « faire avec »)
  - faut-il rendre l'inconnu plus « présent » sans le nommer ? (une trace
    anonyme, un horodatage « il y a 2 jours », le % où il s'est arrêté)

> Rappel discipline : un pari gagné pousse à sauter en « vrai jeu » / 3D.
> On est à l'étage 2 de la pyramide (fierté ✅ → gratitude → contemplation).
> La présentation vient en DERNIER, sur un mécanisme prouvé.

---

## Ce qui a été construit — `etape-b1-blob.html`

Ta remarque sur la tentative nominative était juste : **je l'ai supprimée**, elle
ne traîne plus dans le dépôt.

### L'anonymat est devenu une propriété du format, pas une intention

Plutôt que « ne pas afficher les noms », j'ai retiré au blob **la place où loger
une identité**. Il transportait une liste de marques ; il transporte maintenant
un simple **compte de mains**. L'encodeur n'écrit plus un seul caractère de
texte — rien que des nombres. Il devient impossible d'y glisser un nom, même par
accident, même plus tard.

Conséquence directe et voulue : **on ne peut pas se reconnaître soi-même.** Si
la pierre te revient, tu es une main de plus, pas « la main 1 ». Vérifié.

- Aucun champ de saisie libre dans l'interface.
- Rien qui liste, choisisse ou nomme un joueur.
- Aucun canal vers une personne : le blob sort, on le colle où l'on veut.

Un test automatique relit les **fichiers** — pas mes intentions — et vérifie ces
trois points à chaque fois. Les deux étapes passent.

### Le reste

- Le blob fait environ **1 600 caractères**. Il contient la matière, les cotes,
  l'usure de chaque cube et quelle main a touché quoi.
- Celui qui reçoit **reprend exactement où l'autre s'est arrêté** : la jauge ne
  repart pas de zéro, le travail déjà fourni compte. C'est une pierre, pas deux
  chantiers.
- La taille de l'autre est **mate**, la tienne **vive**. Et la part de chaque
  main est affichée en pourcentage — ce qui survit même si on repasse partout.
- **Une seule note, pour la pierre.** Jamais par joueur.

Vérifié de bout en bout : A dégrossit → exporte → B importe (pierre identique au
cube près) → B travaille une face → réexporte → réimport. Les mains restent
lisibles, la note est celle de la pierre.

### Une contradiction que ton brief tranche

J'avais signalé que le `DESIGN.md` §2 veut des pierres « signées d'une main
(marque de tâcheron) » alors que ce brief veut l'anonymat. **Ton brief tranche :
anonymat.** Ma proposition d'un compromis (« anonyme à la réception, signée dans
l'archive ») est abandonnée — je la note ici seulement pour que la question ne
soit pas perdue quand viendra le vrai jeu et son archive-mémoire.

---

## Verdict (à remplir APRÈS avoir joué)

### Itération B1 — copier-coller manuel
- À l'import du blob, la pierre d'un vrai inconnu garde-t-elle le sel du P2 ? →
- Est-ce plus fort que l'étape A (vrai autre vs toi-oublié) ? →
- Ressent-on « je finis pour quelqu'un » / une adresse à un inconnu ? →
- On monte le pot réel B2 ? OUI / NON →

### Itération B2 — pot partagé réel
- Le sel survit-il au pot anonyme (vs copier-coller) ? →
- Anonymat + non-choix respectés de bout en bout ? →
- **Brique sociale validée ?** OUI / NON →
- Notes libres :
