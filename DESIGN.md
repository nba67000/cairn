# DESIGN — Cairn

> Le fichier le plus important du dépôt. Le code des protos est jetable ; ces
> décisions, non. Chaque ligne ici est le fruit d'un arbitrage tranché, pas
> une idée en l'air. Quand un choix est fait, on note **pourquoi l'inverse a
> été écarté** — c'est ça qui protège des rechutes.

---

## 1. La thèse du jeu

Un jeu sur **la fierté de bien faire pour quelqu'un qu'on ne connaîtra
jamais**. Coopération sans compétition, sans jugement, sans perte possible.
On ne peut pas gagner, on ne peut pas perdre, on ne peut pas être attaqué,
on ne peut pas se faire vandaliser. On ne peut que **contribuer**.

Besoin d'époque visé : la génération ado / jeune adulte a perdu le lien social
local, spontané, à faible enjeu — les « troisièmes lieux », la coopération non
performative — remplacés par des feeds qui comparent et des compagnons IA qui
bouchent le vide sans le combler. Cairn propose le contre-poison : une
**solidarité d'inconnus**, douce et sûre.

> ⚠️ Assumé : ce lien est plus **sûr et scalable** que l'échange Pokémon
> (incarné, en face à face), mais plus **tiède**. On ne recrée pas la cour
> d'école ; on crée autre chose. Ne pas prétendre égaler l'intensité affective
> de l'échange Pokémon — viser la chaleur douce de Death Stranding.

---

## 2. Les invariants mécaniques (le squelette, ne pas y toucher)

### Empiler
On empile des gestes. Support **structurel** : ce qui est déjà posé porte ce
qui vient. Choisi contre « tisser » (harmonie) et « cultiver » (croissance)
car l'empilement donne le « ça doit tenir » viscéral ET fait naître les métiers
tout seuls.

### Gravé
Le geste est **irréversible**. La matière ôtée ne revient jamais. C'est la
nature *physique* du métier de tailleur (on n'ajoute jamais de pierre), pas une
règle plaquée.
- Choisi contre « retravaillable ». Gravé rend chaque geste **lourd** (on
  s'engage) et surtout rend le **vandalisme structurellement impossible** :
  personne ne peut défaire ton geste, donc personne ne peut te nuire. C'est ce
  qui rend le jeu paisible ET sûr.
- Prix à payer : le blocage devient mortel → voir garde-fou « non-bloquant ».

### Contrainte sans blocage (INVARIANT VITAL avec « gravé »)
Le geste précédent — même mauvais — peut **compliquer** la suite, jamais
**l'interdire**. Le système doit garantir qu'on peut TOUJOURS continuer, même
moins bien. Une pierre trop taillée n'est jamais « ratée / poubelle » : elle
est **reclassée** (bonne pour un emplacement plus petit, un mur au lieu d'une
voûte). Rien ne se gâche, tout se recase.

### Plusieurs métiers en chaîne
A → B → C. Le travail fini de l'un est la matière première du suivant.
Tailleur → Maçon → (Charpentier…). Chaîne réelle du bâtiment, pas inventée.
Le geste de A **contraint** le travail de B (souple, pas dur — B garde du choix).

### S'adapter au geste du prédécesseur = LE cœur du plaisir
« Bordel, elle est pas nickel, mais on va faire avec. » ← **réplique-étoile**.
C'est le sel du jeu, pas un pis-aller. Le moment « il l'a commencée comme ça,
bon, je compose » EST le gameplay. Ce qu'on maudit (il a fait un truc bizarre)
et ce qu'on adore (ma construction porte la trace d'un inconnu) sont la MÊME
chose. On ne peut pas garder l'un sans l'autre.

### Archive-mémoire
Chaque pierre gravée est une strate, datée, signée d'une main (marque de
tâcheron). Le château fini est **l'archive de tous les gestes** qui l'ont fait,
dans l'ordre. On le lit comme les anneaux d'un arbre. C'est le « Pokédex de
générosités » incarné.

### Inachevé supportable
Un château en chantier est beau. L'absence ne punit JAMAIS : quand un joueur
part, rien ne se dégrade, rien ne fane. Revenir est un pur gain, jamais une
dette. (Ligne rouge anti-Farmville / anti-FOMO.)

---

## 3. Le geste concret (taille de pierre, fidèle au métier réel)

Le tailleur travaille **du gros vers le fin, sans retour** :
`chasser (dégager les arêtes) → dégrossir (pointe/poinçon) → approcher (ciseau)
→ finir`. La précision réelle se mesure **au trait** : « le ciseau doit couper
le trait en deux ». Trop tailler = irréversible.

### Les 3 outils (ordre de progression naturel)
- **Chasse** — enlève beaucoup, imprécis, dégrossit loin du trait. Ne peut PAS
  finir proprement (force à changer d'outil → progression forcée).
- **Pointe** — enlève moyennement, approche le trait. Cœur du dosage.
- **Ciseau** — enlève peu, précis, finit au trait. Si on force → dépasse →
  trop taillé.

### Le dosage (la source du skill)
Chaque coup a une **force** (durée d'appui ou curseur) et une **zone**.
Le pressé tape fort partout et dépasse le trait. Le bon adapte sa force à sa
distance au trait. La maîtrise = l'**économie du geste** (atteindre les
contraintes avec le moins de matière dépensée).

### Les 4 jauges (couplées — un clic bouge plusieurs jauges, parfois en sens
### contraire — c'est ce qui crée l'arbitrage)
1. **Matière restante** — ne fait QUE descendre. C'est le budget. Chaque coup
   la grignote.
2. **Proximité du trait** — monte en approchant la cible, **CHUTE brutalement
   si on dépasse** (trop taillé). Cœur du skill.
3. **Planéité** — monte avec les outils fins, plafonne avec les grossiers.
4. **Emboîtement avec la voisine** — SEULE jauge qui dépend d'une autre pierre
   déjà posée (donc du travail d'un inconnu). Canal social : transmet
   l'imperfection d'une main à l'autre. (Réservée au jeu complet, pas au
   Proto 1.)

> Pour les protos : on garde **3 jauges** (matière / trait / planéité).
> L'emboîtement arrive avec le multijoueur.

### Le score mesure la FONCTION, pas la ressemblance (décision capitale)
Le % ne dit pas « ressemblance à un modèle » mais « satisfaction des
contraintes » (planéité, emboîtement, hauteur, matière portante). Comme un test
unitaire : mille implémentations passent, il n'y a pas UN cube parfait mais une
*famille* de pierres valables.
- Une pierre à 84 % n'a pas *raté un modèle* de 16 % → elle *tient sa fonction*
  à 84 % avec un point faible identifiable.
- Choisi contre le « schéma à copier » : un modèle unique réintroduit le
  juste/faux, transforme le geste du prédécesseur en **faute** (→ colère) au
  lieu de **contrainte** (→ créativité). « Il a mal joué » rend furieux ;
  « il a joué autrement » rend créatif.
- Une **silhouette-guide** facultative peut aider à viser, MAIS n'est pas la
  réponse notée. L'expert peut s'en écarter intelligemment et *battre* le
  fantôme en s'adaptant à ce qu'il a reçu.

### La propagation de l'imperfection (émergent, précieux)
Le maçon reçoit une pierre un peu de travers → il la pose quand même → son
assise est légèrement biaisée → le suivant en hérite. L'imperfection **voyage**
le long de la chaîne. Le château fini est la trace cumulée de centaines de
petits « on fait avec » — légèrement de guingois, et beau *pour ça*.
> Garde-fou : chaque étape doit pouvoir **absorber** un peu du défaut reçu
> (le maçon rattrape au mortier), sinon l'accumulation devient fatale. Le
> défaut voyage en **s'atténuant**, jamais en s'amplifiant.

---

## 4. Le château (pas la cathédrale)

Changé de cathédrale → château fort pour **éviter la friction culturelle
(religion)** et parce que la fonction (« ça sert à tenir ») est plus lisible.

**PAS de gameplay d'attaque** (ni bélier ni catapulte). Décision tranchée :
un mur imparfait n'est *pas grave*, « on a construit un château fort ! ».
- Conséquence majeure : en retirant le siège, on retire la **seule chose qui
  pouvait rendre l'imperfection punitive**. Plus de test létal → l'imperfection
  ne peut plus jamais être une faute, seulement une trace. **Aucune condition
  d'échec dans le jeu.**
- Garde-fou récupéré : la qualité reste un **gradient** (jamais debout/effondré
  binaire), et on garde une dimension **non-militaire** (l'allure, la gueule du
  château) pour préserver la « beauté imparfaite » qu'offrait la cathédrale.

---

## 5. Les trois moteurs (pourquoi on joue, sans peur ni score)

Pas de défaite → la motivation vient d'ailleurs que la menace. Trois moteurs,
qui ne s'excluent pas mais **se nourrissent en chaîne** :

1. **Fierté du geste** — la belle pierre, signée. *Immédiate* (dès la 1re
   pierre, en solo).
2. **Gratitude en aval** — le maçon qui reçoit ma pierre est aidé ou emmerdé.
   *Différée de peu.*
3. **Contemplation** — le château qui monte, portant les mains de tous.
   *Différée de beaucoup.*

> Ordre imposé par la **structure temporelle**, pas par préférence : le premier
> battement de cœur ne PEUT être que la **fierté** (seul plaisir disponible à
> la seconde zéro, ne dépend de personne). Les deux autres sont les récompenses
> qui donnent envie de **revenir** — moteur de rétention d'un jeu asynchrone.
>
> Conséquence pour le build : d'abord rendre le geste solo agréable
> (fondation), puis greffer gratitude et contemplation (étages).

---

## 6. Sécurité (par conception, pas par surveillance)

Hérité de la réflexion sur les rencontres physiques, transposé :
- **Aucune rencontre physique** entre inconnus. Tout est asynchrone, à distance.
- **Pas de messagerie libre entre inconnus** — seulement des objets définis par
  le jeu (une pierre, une marque). Tue le canal de manipulation à la racine.
- **On ne parcourt jamais les gens**, on ne les choisit pas — matching
  aléatoire. Supprime la surface de repérage d'une cible + empêche de
  « collectionner des personnes ».
- **Gravé = pas d'annulation = pas de vandalisme** possible. L'autre n'est
  jamais une menace : il ne peut RIEN t'enlever.
- Séparation stricte des âges si mineurs (bassins séparés).
> ⚠️ Un vrai produit touchant des mineurs exige une revue Trust & Safety +
> juridique (RGPD enfants, etc.). Ces principes rendent le jeu *sûr par
> conception*, ce n'est pas un blanc-seing de conformité.

---

## 7. Risques connus (à garder en tête, pas à traiter maintenant)

- **Déficit d'anticipation** (angle mort révélé par l'analyse Pokémon) :
  Pokémon a de l'aléatoire partout (chaque herbe haute) ; Cairn n'en a qu'à UN
  endroit — l'état de la pierre héritée. Le geste de taille, une fois maîtrisé,
  devient prévisible. Si le jeu s'essouffle un jour, ce sera probablement par
  là. Question à rouvrir PLUS TARD (pas au proto) : où mettre de la variabilité
  au-delà de l'héritage ?
- **Le déséquilibre offre/demande** (si on réintroduit un jour un système de
  demande d'aide) : tout le monde veut être aidé, peu veulent aider. Levier
  connu : aider doit être le meilleur farm, mais coûter une ressource rare
  d'une AUTRE nature (les deux gains ne sont pas dans la même monnaie).
- **Le geste fade** : tout repose sur le fait que tailler + « faire avec »
  procure une étincelle. Si c'est un clic creux, aucune récompense ne le sauve.
  C'est ce que les protos vérifient.

---

## 8. Ce que les protos doivent trancher

- **Proto 1** → « Tailler une pierre de A à Z est-il agréable en solo ? »
  Juge : ai-je envie de tailler une 2e pierre sans me forcer ? Le frisson du
  dernier coup près du trait est-il là ?
  > Note : Michelangelo: Stonemason Simulator (2025) a déjà *prouvé
  > commercialement* que oui. Le Proto 1 est donc surtout un socle pour le 2.
- **Proto 2** → « Hériter de la pierre d'un inconnu et "faire avec" a-t-il un
  sel que tailler de zéro n'a pas ? »
  **JUGE DE PAIX DE TOUT L'ÉDIFICE** : « Le tordu » doit être **PLUS prenant**
  que « Le propre ». Si l'inverse, l'hypothèse centrale est fausse — le savoir
  sur un proto moche plutôt que sur un jeu fini.

---

## 9. Ce qui distingue Cairn de l'existant

- **Michelangelo: Stonemason Simulator** — même geste de taille solo, mais
  SOLO, reproduction d'un MODÈLE (le David), pas de château commun. A la brique
  du bas, aucun des étages sociaux.
- **Death Stranding** — la main anonyme qui réchauffe, mais les dons ne font
  pas *progresser* et il n'y a pas de chaîne de métiers.
- **Dark Souls (messages)** — un inconnu influence ton parcours, mais l'aideur
  ne progresse pas vraiment.
- **FarmVille** — MÊME squelette (progression débloquée par autrui) mais geste
  d'aide VIDE → détesté. Cairn = le même squelette avec l'âme (le geste a du
  goût et laisse une trace). C'est la carte au trésor à l'envers.

**Place vide occupée par Cairn** : hériter en asynchrone du chantier imparfait
d'un inconnu, et *briller en le rattrapant*, dans une chaîne de métiers, sur un
objet commun gravé et invandalisable. Jamais assemblé ainsi.

---

## 10. Ce que les protos ont tranché (29/08/2026)

### Les deux verdicts

- **Proto 1 — OUI sur ses deux questions.** Le geste de taille est satisfaisant,
  et le frisson du dernier coup est là. Mais pas du premier coup : la première
  version a été jugée *redondante*, et la cause était qu'il n'existait qu'une
  seule bonne méthode. Corrigé en donnant à chaque pierre une **commande** qui
  dit ce qui compte pour elle — d'où un arbitrage réel, mesuré.
- **Proto 2 — OUI. LE PARI EST GAGNÉ.** « Les coups tordus et bâclés sont plus
  fun. » Hériter du travail imparfait d'un inconnu et composer avec est *plus*
  prenant que partir d'un bloc neuf. L'imperfection héritée crée de la valeur.

> Restriction : l'inconnu est simulé par un bot. Le lien avec une **vraie**
> personne n'est pas testé et ne peut pas l'être en solo (§1). C'est l'étage
> suivant, et c'est maintenant la principale inconnue du projet.

### Deux enseignements de méthode, chèrement acquis

1. **Le score ne doit pas avoir une seule bonne réponse.** La première version
   du Proto 1 donnait 94 à une ligne de jeu et 42 à toutes les autres : ce n'est
   pas du skill, c'est de l'exécution — et l'exécution ne donne pas envie de
   recommencer. Une table de stratégies saine montre plusieurs lignes viables.
2. **« À la cote » n'est pas « au cube près ».** Tant que l'exactitude était
   exigée partout, l'outil fin restait obligatoire partout, donc une seule
   méthode. La **tolérance**, variable selon que la face se verra ou non, est ce
   qui a débloqué tout le reste. C'est le §3 appliqué : une *famille* de pierres
   valables, pas un modèle unique.

### La réserve qui reste : ça ne ressemble pas encore à un tailleur de pierre

Retour de partie répété, jusqu'au bout : *« ça ressemble au jeu Sixty Four
plutôt qu'à un tailleur de pierre »*. C'est juste, et il faut distinguer deux
problèmes que j'ai longtemps confondus :

- **Le verbe** — réglé. On ne fait plus disparaître des paquets de cubes ; on
  maintient, on balaie, l'outil frappe en cadence et rabote des copeaux. Ça
  *joue* comme de la taille.
- **La matière** — pas réglé. Ça *ressemble* encore à du minage abstrait.

Causes, par ordre d'importance :

1. **La maille de cubes.** La surface est un treillis de cubes séparés, aux
   facettes nettes. C'est *la* signature du jeu de voxels. Or elle n'a jamais
   été un choix de design : c'était une **facilité de prototype**, imposée par
   le brief (iso 2D, cubes empilés) pour pouvoir coder vite en canvas 2D.
   → **Décision : la maille de cubes ne survit pas au prototype.** Le vrai jeu
   doit présenter une surface **continue et irrégulière**. Le moteur porte déjà
   une usure fractionnaire (un cube à demi rongé est reculé d'autant) : la suite
   naturelle est un relief continu, pas des cubes discrets.
2. **Le vide autour.** Le bloc flotte sur une ombre dans le noir. Pas d'établi,
   pas d'atelier, aucune échelle, rien d'humain. Une scène de tailleur se
   définit autant par ce qui entoure la pierre que par la pierre.
   → Poser la pierre sur un **chevalet**, dans un lieu.
3. **Les éclats s'évaporent.** Ils volent et disparaissent. Dans un atelier, la
   poussière et les éclats **s'accumulent au pied du bloc** : c'est la trace
   visible du travail fourni. Bon marché, et parfaitement dans la thèse du jeu
   (§2, archive-mémoire).
4. **Aucun corps.** Pas de main, pas de maillet, pas de fer dans le cadre :
   l'outil est un cercle. Un jeu de taille montre le ciseau contre la pierre.
5. **Le son.** Toujours interdit au proto, et c'est le canal le plus important
   qui manque pour qu'on sente *quelqu'un* frapper de la pierre. À rouvrir en
   premier hors proto.

> Les points 1 et 2 sont structurels — ils décident de ce à quoi le jeu
> ressemble. Les 3, 4 et 5 sont additifs et peu coûteux.
