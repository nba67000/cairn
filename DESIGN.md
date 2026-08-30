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
2. **Respect des contraintes** — monte quand la pierre satisfait ses exigences
   (planéité des faces porteuses, hauteur dans la fourchette, emboîtement avec
   la voisine…), **CHUTE brutalement si on dépasse** (trop taillé = on crève une
   contrainte, ex. plus assez de matière, hauteur sous la fourchette). Cœur du
   skill. ⚠️ NE PAS coder ça comme « proximité d'une cible unique » (voir
   correction majeure §3bis) : c'est la satisfaction d'un cahier des charges à
   solutions multiples, pas la ressemblance à un bloc prédéfini.
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

## 3bis. CORRECTION MAJEURE — cahier des charges, pas cible unique

> Écart repéré APRÈS les protos 1-2-3 : ils avaient une **silhouette-cible** à
> atteindre. Or « atteindre un bloc prédéfini » = UNE seule bonne réponse =
> le schéma-à-copier déjà rejeté, revenu par la porte de l'implémentation.
> Les protos testaient donc une version DÉGRADÉE du jeu (bon geste, mais jugé à
> la ressemblance). Ce qui a marché en version dégradée est prometteur, mais le
> vrai jeu n'a pas encore été testé.

### Le piège du Proto 2 (à savoir)
Avec cible unique, le tordu battait le propre peut-être juste parce qu'il était
**plus dur à ramener vers la cible** → on aurait validé « un puzzle de
correction plus dur est plus fun », PAS la thèse « composer avec le choix d'un
autre est fun ». Corriger l'écart d'un autre (l'autre = fauteur) ≠ composer avec
sa proposition (l'autre = partenaire). Il faudra re-tester en version contraintes.

### Le principe : on définit ce que la pierre doit SATISFAIRE, pas sa forme
Exemple pierre de mur, contraintes mesurables :
- face du dessous **plane** (repose) — peu importe où est le plan, il en faut un
- face du dessus **plane et parallèle** (porte la suivante)
- **hauteur dans une fourchette** [X..Y] — pas une valeur exacte → plusieurs
  hauteurs valables
- **assez de matière restante** (porte la charge)
- **emboîtement avec la voisine** — dépend de ce qu'un AUTRE a posé avant

Aucune de ces règles ne dit « enlève ce cube-là ». Des milliers de blocs
différents les satisfont → la « multitude de solutions ». Le score = combien de
contraintes remplies et à quel point, PAS distance à une cible.

### La liberté qui se referme (propriété reine — décision prise)
La 1re pierre d'un mur a ~mille solutions. En la posant, le joueur **restreint**
le champ de la suivante. Chaque geste gravé **consomme une part de la liberté**
du suivant. Le dernier joueur du mur n'a presque plus de choix — sa pierre est
quasi déterminée par tous les gestes d'avant.
- Ce n'est PAS un problème d'équité, c'est le **cœur poétique** : la liberté est
  une ressource commune qui se consomme le long de la chaîne. Premiers = ivresse
  du champ ouvert ; derniers = virtuosité de la contrainte serrée. Deux plaisirs.
- **Comble l'angle mort d'anticipation** (§7) : chaque pierre reçue pose un
  problème différent, non par hasard mais par l'**histoire des gestes
  précédents**. Variabilité infinie et gratuite, générée par les joueurs. Plus
  besoin d'un générateur aléatoire.
- **Courbe de difficulté naturelle sans niveaux** : tôt = libre/facile, tard =
  contraint/dur. S'aligne avec les slots par niveau (pierres très contraintes →
  hauts niveaux ; pierres libres → débutants).
- **Vrai sens de « faire avec »** : l'inconnu d'avant a dépensé la liberté
  commune d'une certaine façon ; je compose avec le champ qu'il m'a laissé. Il
  n'a pas commis de faute, il a fait un choix légitime qui a fermé des portes et
  ouvert d'autres. J'hérite de son *chemin*, pas de sa *faute*.

### Garde-fou (le même que toujours, ici vital)
La liberté qui se referme ne doit JAMAIS tomber à zéro avant la dernière pierre.
Si l'avant-dernier peut rendre la dernière **impossible** (aucune solution
valable restante), on retombe dans le blocage mortel du « gravé ». La règle des
contraintes doit garantir qu'il **reste toujours ≥1 solution jouable**, même
étroite, jusqu'au bout. Le champ se referme, ne se ferme jamais complètement.

### Conséquence pour les protos
Le geste ne change pas (on taille en enlevant des cubes), l'affichage ne change
pas (iso, silhouette devient simple repère facultatif), seul le **calcul du
score** change : de « compare à la cible » → « évalue N contraintes ». Modif
circonscrite, pas une réécriture. Prochaine version des protos à faire là-dessus.

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

- **Déficit d'anticipation** (angle mort révélé par l'analyse Pokémon) —
  **EN GRANDE PARTIE COMBLÉ** par la « liberté qui se referme » (§3bis) : chaque
  pierre reçue pose un problème différent généré par l'histoire des gestes
  précédents, pas par un générateur aléatoire. Reste à vérifier en jeu que cette
  variabilité suffit à garder le geste frais dans la durée.
- **Le déséquilibre offre/demande** (si on réintroduit un jour un système de
  demande d'aide) : tout le monde veut être aidé, peu veulent aider. Levier
  connu : aider doit être le meilleur farm, mais coûter une ressource rare
  d'une AUTRE nature (les deux gains ne sont pas dans la même monnaie).
- **Le geste fade** : tout repose sur le fait que tailler + « faire avec »
  procure une étincelle. Si c'est un clic creux, aucune récompense ne le sauve.
  C'est ce que les protos vérifient.

- **Le contenu fini** (leçon Chivalry) : Chivalry 2 — pic ~16 900 joueurs au
  lancement, retombé à ~2 000-2 500 (−66 %). Une cause : une fois toutes les
  cartes/modes vus, on tourne en rond. Cairn a une parade PARTIELLE (contenu
  généré par les joueurs : chaque pierre héritée est neuve, cf. « liberté qui se
  referme »). MAIS question non résolue : **quand le château est fini, on fait
  quoi ?** Si la réponse est « rien », c'est le même mur que Chivalry, décalé.
  Pistes (pour plus tard) : châteaux infinis / nouveaux chantiers qui
  recommencent / une raison de revenir sur un château fini (l'entretenir, le
  visiter, en admirer l'archive). À trancher une fois le cœur validé.

- **Le try-hard toxique, version Cairn** (leçon Chivalry, la plus subtile) :
  Chez Chivalry, la mort vient de l'**écart de compétence qui s'auto-renforce** —
  les mordus deviennent excellents, les nouveaux se font démonter en 3 s et
  partent, la population vieillit et rétrécit jusqu'à un club fermé d'experts.
  Le jeu se rend HOSTILE à mesure qu'il mûrit : chaque joueur est une menace,
  la population forte joue CONTRE l'accessibilité.
  → Cairn est structurellement l'antidote (voir §8) MAIS un risque analogue
  subsiste sous une autre forme : un vétéran pourrait juger que les pierres des
  débutants « salissent » le château commun. Le grief ne serait pas « tu me
  tues » mais « tu bâcles NOTRE ouvrage ». Le **score visible** pourrait créer
  une hiérarchie de mépris entre bons et mauvais tailleurs. C'est le « try-hard »
  de Cairn.
  Garde-fous déjà en place : l'**anonymat** (on ne peut pas mépriser qui on ne
  voit pas) et le fait qu'une pierre « moyenne » sert quand même (reclassée,
  jamais jetée). Vigilance à garder : ne pas transformer le score en outil de
  jugement social ; envisager que le score reste privé / ressenti plutôt
  qu'affiché publiquement et comparé.

---

## 7bis. Décisions différées (notées, PAS à traiter maintenant)

> Rangées ici exprès pour ne pas polluer les protos. Chacune est un vrai sujet,
> mais aucune ne débloque une question ouverte actuelle — donc on attend.

### Rendu lissé de la pierre (cosmétique — pour quand le jeu passe de
### « tester » à « séduire »)
Aujourd'hui la pierre est un tas de cubes visibles (voxels iso). Une fois les
mécaniques validées, on pourra afficher une **surface lisse** à la place, sans
toucher au modèle de jeu.
- Technique : la grille de cubes reste le modèle logique (jauges, matière
  enlevée) ; le lissage est une pure **couche d'affichage** par-dessus.
  Algorithmes : **marching cubes** (historique), ou **surface nets / dual
  contouring** (mieux pour les arêtes vives, ce qu'on veut pour une pierre
  taillée : angles nets + faces planes, pas du tout arrondi).
- Point clé : **les imperfections survivent au lissage** — un cube mangé en
  trop devient un léger creux / une facette de travers, pas une marche. Le
  lissage traduit les défauts en accidents de surface réalistes, il ne les
  efface pas. Exactement ce qu'on veut (garder la trace du geste, en pierre
  crédible).
- Plus tard encore : grain, traces d'outil, usure — habillage d'habillage.
- ⚠️ Pourquoi attendre : le lissage ne change AUCUNE question ouverte (maçon,
  chaîne de métiers, sel de l'inconnu marchent aussi bien en cubes). Pire, une
  belle surface **embellit les protos et fausse le jugement** (le thermomètre
  parfumé). Le cube moche est l'allié du test. Le lissage = récompense pour
  l'étape « rendre beau », gain énorme pour peu d'efforts (le modèle ne bouge
  pas), à faire quand la question deviendra « comment séduire ».

### Le geste du maçon (à concevoir avant la chaîne de métiers)
Le maçon place les pierres, jouable. L'architecte (non jouable en v1) = le jeu
lui-même, qui fournit le **plan sous forme de contraintes** (« un mur qui tient
entre ici et là, cette hauteur »), JAMAIS un patron pierre-par-pierre à
décalquer — sinon on réintroduit le schéma-à-copier déjà rejeté (le score
mesure la fonction, pas la ressemblance).
Choix NON tranché — deux maçons possibles, deux morales :
- **Maçon-placeur** : skill = jugement d'appariement (trouver la place où les
  défauts d'une pierre ne gênent pas). Casse-tête spatial type Tetris à pièces
  imparfaites. Le défaut est *neutralisé par un bon placement* (rejoint « rien
  ne se gâche, tout se recase »). Morale : « une juste place pour chaque chose,
  même imparfaite ».
- **Maçon-ajusteur** : skill = compenser au mortier, égaliser l'assise pour que
  la pierre suivante parte droite malgré le défaut hérité. Le défaut est
  *absorbé* (incarne le garde-fou « le défaut s'atténue au lieu de s'amplifier »
  de §3). Morale : « on soigne les blessures du travail des autres ».
Intuition actuelle : le placeur crée une décision plus riche à chaque pierre
(où la mettre ?) ; l'ajusteur boucle mieux la chaîne de propagation du DESIGN.
À trancher au moment de concevoir le maçon, pas avant.

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
- **Chivalry 2 (contre-modèle de DURABILITÉ)** — combat médiéval multi
  compétitif. Pic ~16 900 → ~2 000-2 500 (−66 %). Mort typique du compétitif :
  l'écart de compétence s'auto-renforce (mordus → experts, nouveaux démontés en
  3 s → partent), la population vieillit et se referme en club d'experts, et
  **chaque joueur est une MENACE** → le jeu devient hostile à mesure qu'il
  mûrit, la population forte joue CONTRE l'accessibilité.
  Cairn est le **négatif photographique** de ces causes de mort :
  - écart de compétence → chez Cairn, vétéran et débutant ne s'affrontent pas,
    ils contribuent au MÊME château (liberté qui se referme = courbe de
    difficulté où chacun a sa place : pierres contraintes aux experts, pierres
    libres aux débutants). L'expertise *complète* au lieu d'*exclure*.
  - autre = menace → chez Cairn, gravé/invandalisable : l'autre ne peut jamais
    nuire. Plus il y a de monde, plus c'est ACCUEILLANT. L'effet réseau réchauffe
    au lieu de menacer.
  - « mort en 3 s, recommence » → chez Cairn, pas de mort/défaite/perte. Le pire
    = une pierre à 70 % qui sert quand même.
  **Thèse de durabilité** : là où le modèle compétitif porte sa propre asphyxie
  (santé DÉCROÎT avec la population experte), Cairn est conçu pour que sa **santé
  CROISSE avec la population**. Structurellement plus durable qu'un Chivalry.
  (Épines conservées en §7 : contenu fini + try-hard toxique version Cairn.)
- **Minecraft (LE concurrent conceptuel — le plus proche, à connaître par
  cœur)** — sandbox de construction, ~15 ans, revenus toujours en hausse
  (220 M$ en 2024). Prouve à l'échelle planétaire que **bâtir ensemble sur un
  monde persistant est puissant et durable**. Donc le pari de fond de Cairn
  n'en est pas un — il a un précédent géant. Ce que Minecraft VALIDE pour Cairn :
  le voxel comme langage iconique (le blocky n'est pas un défaut → la beauté
  n'est pas notre risque) ; la co-création sociale réchauffe vraiment
  (Poudlard brique par brique = l'étage 2 de Cairn a un précédent) ; pas
  d'objectif imposé = rétention longue (comme le château sans siège/défaite).

  ⚠️ MENACE FRONTALE : Minecraft fait déjà « construire ensemble », en mieux,
  avec 15 ans d'avance. Réponse à « en quoi c'est pas un Minecraft en moins
  bien ? » = TROIS oppositions, chacune une force de Cairn LÀ OÙ Minecraft est
  faible (= pitch défensif à connaître) :
  1. **Contrainte vs liberté infinie.** Minecraft creative = ressources
     infinies, aucune règle, pose n'importe quel bloc n'importe où. Faiblesse
     cachée : la page blanche lasse (témoignages « je me suis ennuyé et j'ai
     arrêté »). Poser un bloc est trivial. Cairn = la pierre RÉSISTE (cahier des
     charges, liberté qui se referme), tailler est un GESTE qu'on maîtrise.
     Cairn offre la *fierté de la compétence* ; Minecraft, la *liberté de
     l'expression*. Deux plaisirs, deux publics (Cairn vise ceux pour qui la
     page blanche angoisse et qui ont besoin d'un cadre pour ressentir la
     maîtrise).
  2. **Inconnu anonyme vs entre-soi.** Minecraft = ton terrain / ton serveur de
     potes choisis. PAS d'« héritage à moitié fait d'un inconnu jamais
     rencontré, avec qui je compose ». Le cœur de Cairn (validé Proto 2) n'existe
     nulle part dans Minecraft. Sandbox personnel/entre-soi vs chaîne de mains
     anonymes = nature sociale complètement différente.
  3. **Gravé vs réversible.** Minecraft = casse un bloc, recommence, rien ne
     pèse, bloc jetable. Cairn = irréversible → le geste ENGAGE, pèse, devient
     trace pour toujours → pierre PRÉCIEUSE. Minecraft ne peut pas avoir cette
     gravité (contraire à sa philosophie de liberté).

  Leçon sur le CONTENU FINI (§7) : la solution de Minecraft = l'absence de fin
  par design (monde infini, buts inventés par le joueur). Cairn a un château qui
  SE TERMINE — double tranchant : satisfaction de complétion que Minecraft
  n'offre JAMAIS (+), mais risque d'essoufflement (−). Ne PAS copier l'infini
  (ça renierait la complétion). Viser : **fin locale** (ce château-ci s'achève,
  et c'est beau = force sur Minecraft) + **continuité globale** (un nouveau
  chantier attend toujours). On récupère les DEUX : complétion ET durée infinie,
  là où Minecraft n'a que la durée.

**Place vide occupée par Cairn** : hériter en asynchrone du chantier imparfait
d'un inconnu, et *briller en le rattrapant*, dans une chaîne de métiers, sur un
objet commun gravé et invandalisable. Jamais assemblé ainsi.
