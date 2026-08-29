# Proto 3 — La boucle donner / recevoir

## Contexte
Proto 2 a **validé le pari central** : recevoir une pierre imparfaite (tordue,
bâclée) est PLUS fun que recevoir une pierre propre. « Faire avec » est un vrai
plaisir de jeu. ✅

Mais deux choses restent **non testées**, et le jeu entier en dépend :
1. Dans le Proto 2, la pierre héritée était **codée en dur par moi**. Je savais
   que c'était un décor. Hériter du travail d'un **humain réel** peut être
   *plus* fort (« quelqu'un a vraiment fait ça ») ou *décevant* (les humains
   font des trucs moins intéressants à rattraper que des états conçus exprès).
2. Je n'ai testé que le côté **receveur**. Le côté **donneur** est vierge :
   est-ce que savoir que MA pierre part chez un inconnu change ma façon de
   tailler ? (= 2e moteur du DESIGN : la gratitude en aval.)

## Question testée (une seule)
**La pierre d'un vrai humain garde-t-elle le sel qu'avaient les états codés en
dur — et est-ce que donner à un inconnu change mon geste ?**

> On monte à l'étage 2 de la pyramide des moteurs (fierté ✅ → **gratitude** →
> contemplation). NE PAS sauter à l'étage 3 (présentation, 3D, « vrai jeu »).

---

## Étape A — d'abord : boucle solo-différée (aucun multijoueur)

Le plus petit test possible, sans réseau, sans compte, sans serveur.

Reprends le moteur du Proto 1/2 (cubes iso, outils, jauges). Ajoute une boucle :

1. Le joueur **commence** une pierre (la taille à moitié, puis la « dépose »
   dans un **pot commun** local — simple tableau en mémoire / localStorage).
2. À la session suivante (ou après un délai / un mélange), le jeu lui **ressert
   une pierre du pot** — idéalement une qu'il a lui-même commencée mais assez
   tard / mélangée pour qu'il en ait oublié les détails, « comme si » elle
   venait d'un autre.
3. Il la **finit**. Score + point faible hérité visible.

**Ce qu'on observe :** est-ce que recevoir une pierre qu'on a soi-même
commencée (mais oubliée) procure déjà un mini-effet « tiens, qui a fait ça » ?
Est-ce que déposer dans un pot « pour un autre » change quelque chose au soin
qu'on met ?

> ⚠️ localStorage est INTERDIT dans les artifacts claude.ai mais OK ici :
> ce proto tourne en local dans un navigateur classique via Claude Code. Simple
> fichier HTML, ouvert en local. (Si tu testes dans l'app, remplace par une
> variable JS en mémoire.)

---

## Étape B — ensuite : boucle à deux vrais joueurs

Seulement si l'étape A est concluante.

Deux personnes (toi + un ami), chacune son navigateur, un **pot partagé
minimal**. Pas de compte, pas d'auth, pas de matchmaking. Le plus simple qui
marche :
- soit un tout petit backend (un JSON partagé, un service type gist / pastebin /
  un mini serveur Node local sur le réseau),
- soit même du **copier-coller manuel** d'un état de pierre entre deux joueurs
  pour la toute première version (moche mais suffisant pour tester le ressenti).

Chacun : commence une pierre → l'envoie dans le pot → reçoit celle de l'autre →
la finit. **Anonyme** (on ne sait pas qui a fait quoi), conforme au DESIGN
(pas de messagerie, matching non choisi).

**Ce qu'on observe :** le sel du Proto 2 survit-il quand c'est un VRAI inconnu
qui a taillé ? Ressent-on l'« adresse à quelqu'un » — le fait de bien faire
pour un humain qu'on ne connaîtra jamais ?

---

## Contraintes (identiques aux protos précédents)
- Iso 2D façon Age of Empires, vue fixe, pas de 3D, pas de rotation.
- Un seul fichier HTML par étape (A et B peuvent être 2 fichiers).
- Zéro fioriture : pas de son, menu, niveaux, graphismes léchés.
- Réutiliser / copier le moteur de taille existant, ne pas le réécrire.

---

## Le verdict qui décide de l'étage 3

- **Étape A concluante ?** (recevoir une pierre « oubliée » a un mini-sel, et
  déposer « pour un autre » change le soin) → passer à B.
- **Étape B concluante ?** (la main d'un vrai inconnu garde le sel) → la brique
  SOCIALE est validée. On peut ENFIN penser « vrai jeu » : d'abord la chaîne de
  métiers (tailleur → maçon), PUIS seulement la présentation (3D, château qui
  monte).
- **Étape B décevante ?** (le sel s'évapore avec un humain réel) → on l'apprend
  sur un mini-proto, pas sur 6 mois de dev. Chercher pourquoi : voit-on assez la
  main de l'autre ? le rattrapage rapporte-t-il assez ? faut-il rendre l'inconnu
  plus « présent » (une marque, une trace, un horodatage) ?

---

## Ce qui a été construit — et un écart que j'ai commis

> ⚠️ **J'ai construit l'étape B avant d'avoir lu ce brief.** Tu me l'as déposé
> pendant que je codais, après m'avoir demandé oralement « le passage de la
> pierre joueur A à joueur B ». J'avais donc sauté l'étape A, et fait une
> étape B **nominative** où les deux joueurs se choisissent — alors que ton
> brief la veut **anonyme, matching non choisi**. Corrigé en partie ; ce qui
> reste à trancher est en bas.

### `etape-a-pot.html` — la boucle solo-différée (conforme au brief)

Aucun multijoueur, aucun réseau, aucun compte. Un **pot** gardé dans le
navigateur (`localStorage`, autorisé ici puisqu'on ouvre un fichier local).

- On **commence** une pierre, on taille ce qu'on veut, on la **dépose**.
- Le pot ne resert **jamais** une pierre déposée pendant la même ouverture de
  page : il faut fermer et revenir. Le délai fait l'oubli.
- On **ne choisit pas** ce qu'on reçoit : le pot tire au sort.
- Le pot est **anonyme par construction** — il ne transporte aucune marque. Le
  panneau dit seulement « une main avant toi — 34 % ».
- Une pierre n'est servie **qu'une fois**.

Vérifié sur deux ouvertures simulées : dépôt, refus de se resservir soi-même le
jour même, puis service à la session suivante, pierre **identique au cube près**,
sans marque, et retirée du pot.

### `etape-b-relais.html` — deux vrais joueurs (à revoir)

Fait avant le brief. Ce qui est bon : la pierre voyage dans un code d'environ
1 700 caractères, chaque cube retient quelle main l'a touché, la part de chaque
main est affichée, et celui qui reçoit reprend exactement où l'autre s'est
arrêté. L'aller-retour A → B → A a révélé deux défauts, corrigés : la jauge
repartait de zéro à la réception, et la main du premier disparaissait dès que le
second repassait partout.

**Ce qui n'est pas conforme à ton brief, et qu'il faut trancher :**

1. **Nominatif au lieu d'anonyme.** J'ai mis une *marque de tâcheron* qui voyage
   avec la pierre. Ton brief d'étape B dit « anonyme, on ne sait pas qui a fait
   quoi ». Mais le `DESIGN.md` §2 dit l'inverse : « chaque pierre est datée,
   **signée d'une main** (marque de tâcheron) ». **Tes deux documents se
   contredisent.**
   → Ma proposition, si tu la valides : **anonyme au moment de recevoir**
   (on ne sait pas de qui vient la pierre, on ne l'a pas choisie), **signée dans
   l'archive** (une fois la pierre finie, on peut lire les mains qui l'ont
   faite). Ça honore les deux : le lien est à l'inconnu, la mémoire est nominale.
2. **On choisit son partenaire.** Le relais par code suppose qu'on s'envoie le
   code, donc qu'on se connaît. Ton brief veut un *pot partagé* et un matching
   non choisi. Pour y arriver sans serveur, la piste la plus simple reste le pot
   commun — mais partagé, ce qui demande un backend minimal (gist, pastebin, un
   petit Node sur le réseau local), ce que ton brief autorise explicitement.

**Donc l'étape B est à refaire une fois l'étape A jugée**, et à refaire *après*
avoir tranché le point 1. Je ne l'ai pas fait tout seul : c'est une décision de
design, pas un détail d'implémentation.

---

## Verdict (à remplir APRÈS avoir joué)## Verdict (à remplir APRÈS avoir joué)

### Étape A — solo différé
- Recevoir une pierre qu'on a oubliée procure-t-il un mini « qui a fait ça » ? →
- Déposer « pour un autre » change-t-il le soin qu'on met ? →
- Concluant, on passe à B ? OUI / NON →

### Étape B — deux vrais joueurs
- Le sel du Proto 2 survit-il avec un vrai inconnu ? →
- Ressent-on l'« adresse à quelqu'un » (bien faire pour un humain) ? →
- La brique sociale est-elle validée ? OUI / NON →
- Notes libres :
