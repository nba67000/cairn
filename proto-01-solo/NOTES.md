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

## Verdict (à remplir APRÈS avoir joué)

- Envie d'une 2e pierre sans se forcer ? →
- Frisson du dernier coup présent ? →
- Le dosage force/outil crée-t-il un vrai arbitrage, ou on tape au hasard ? →
- Décision : passer au Proto 2 ? OUI / NON →
- Notes libres :
