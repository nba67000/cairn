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

## Verdict (à remplir APRÈS avoir joué)

### Étape A — solo différé
- Recevoir une pierre qu'on a oubliée procure-t-il un mini « qui a fait ça » ? →
- Déposer « pour un autre » change-t-il le soin qu'on met ? →
- Concluant, on passe à B ? OUI / NON →

### Étape B — deux vrais joueurs
- Le sel du Proto 2 survit-il avec un vrai inconnu ? →
- Ressent-on l'« adresse à quelqu'un » (bien faire pour un humain) ? →
- La brique sociale est-elle validée ? OUI / NON →
- Notes libres :
