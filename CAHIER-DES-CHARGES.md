# Cahier des charges — ODBI Academy (plateforme LMS)

> Document de travail vivant — mis à jour au fil des décisions.
> Sert de référence pour le développement technique du projet réel.
> Dernière mise à jour : 24/06/2026

---

## 1. Contexte & objectif

ODBI souhaite une plateforme de formation (LMS) inspirée de Skool, mais
adaptée à sa pédagogie et à son identité de marque (« Stratège du facteur
humain en haute intensité »).

**Objectif** : diffuser les programmes ODBI en ligne (contenu asynchrone) +
animer des promotions en direct (visios, présentiels), avec un assistant IA
et une gamification.

---

## 2. Périmètre

### Inclus
- Inscriptions + comptes membres + rôles
- Catalogue de programmes / formations
- Cours structurés en sessions (vidéos, audios, PDF, exercices, quiz)
- Promotions (cohortes) avec contenu synchrone
- Visios live (BigBlueButton) + replays
- Assistant IA « YoDalf »
- Gamification (points, niveaux, classement, badges)
- Back-office d'administration (contenu, membres, accès, promotions)

### Exclu / externalisé
- **Module communauté** : NON développé → géré sur **WhatsApp** (groupe par promotion)

---

## 3. Identité visuelle

S'appuie sur le **design system ODBI** (`odbi-design-tokens.css` / `.json`).

- **Polices** : Roboto (titres), Roboto Mono (corps), Raleway (légendes)
- **Couleurs clés** : Asparagus `#86A43F`, Apple Green `#B1BC4B`,
  Indigo Dye `#114C6A`, Jet `#4D4D4D`, Black `#1A1A1A`, Alabaster `#F0EEE6`

### Logo & marque
- Marque : **« Odbi campus »** (et non « ODBI Academy »). Logo en haut à gauche (barre + page de connexion).
  **Logo officiel intégré** : versions **foncée** (barre du haut, fond clair) et **blanche** (page de connexion,
  fond foncé), extraites des PDF fournis (PNG transparent, recadrés, embarqués en base64).

### Couleurs par univers (codes repris des flyers ODBI)
- **Programme « La Voie »** → **bleu uniquement** (bleu profond `#1F465B`).
- **« La Voie des… » (managers, artisans)** → **bleu + vert mixés** (`#1F465B` + `#86A43F`).
- **Born To Coach** → dégradé **noir → vert olive** (noir d'abord, `#435B20`).
- **Intelligence Collective** → dégradé **noir → vert forêt** (noir d'abord, `#3F571D`).
- **Série Vous En… (VEM/VEP/VAT/VEL)** → **photo (placeholder) + aplat NEUTRE gris** (gris du design system),
  volontairement sans couleur d'univers. Photos définitives à fournir.
- L'**étiquette texte « bleu + vert »** n'est plus affichée à l'écran (identité conservée dans les tokens).

### Présentation des programmes (vignettes)
- Chaque programme = **une image + un aplat couleur** selon son code couleur.
- *Maquette : photos temporaires extraites des flyers PDF (La Voie / BTC / IC) avec aplat par univers ;
  la série Vous En… utilise une photo placeholder + **aplat gris neutre**.* À remplacer par les visuels
  définitifs (export Claude Design ou photothèque ODBI).

### Icônes
- **Pas d'emojis dans l'interface** : types de contenu (exercice, vidéo, audio, PDF,
  quiz, visio) et navigation utilisent des **icônes vectorielles (SVG)** cohérentes.

### Page de connexion
- Dégradé latéral gauche **vert → noir → bleu** (noir inséré au milieu).
- Accroche : « **Rejoindre le mouvement pour éclairer l'humain et transformer les interactions.** »
  Sous-titre : « Le campus ODBI : cours structurés, visioconférences en direct et YoDalf… ».

---

## 4. Acteurs & rôles

| Rôle | Description |
|------|-------------|
| **Membre / Participant** | Suit les cours, participe aux visios/présentiels de sa promo |
| **Coach** (hors animation) | Coache aux présentiels / trainings ; **est évalué** par les participants ; **n'anime pas** de programme ; peut être **alumni** de l'École |
| **Animateur** (périmètre) | **Anime** les visios/promotions d'un ou plusieurs programmes ; peut **aussi être coach** (cumul des deux rôles) |
| **Admin** | Gère contenu, membres, accès, promotions, intégrations |

> Le rôle historique « Coach / Animateur » est **scindé** en deux rôles distincts et **cumulables** :
> **Coach** (posture d'accompagnement, sans périmètre d'animation, soumis à l'évaluation interne §12ter) et
> **Animateur** (périmètre = programmes animés). Un même membre peut porter les deux badges
> (ex. *Marie Lambert* = Animateur + Coach).

### Modèle de droits (rôle × périmètre)
- **Participant** : accès **uniquement à sa promo** (programmes où il est inscrit).
- **Coach (hors animation)** : **pas de périmètre d'animation** ; intervient aux **présentiels / trainings**,
  **est évalué** (§12ter), et peut être rattaché à la promotion permanente **Alumni / Coachs École**
  une fois son parcours terminé. **Aucun accès au back-office** — *sauf* s'il est **aussi Animateur** (cumul).
- **Animateur** : **affecté à un ou plusieurs programmes** (table d'association *animateur ↔ programmes animés*).
  Il a accès à **toutes les promos de ces programmes** (suivi pédagogique, visios, présentiels) et
  **à rien** sur les programmes qu'il n'anime pas. Son back-office est en **« vue animateur »** (restreinte).
- **Admin** : accès à **tout**.
- Le **périmètre d'animation** se gère dans le back-office (Membres → Périmètre :
  cases à cocher des programmes animés). Les vues de l'animateur (Suivi, Promotions, Planning visios)
  sont **filtrées** sur ses programmes.

Accès **éditable** : un admin peut définir qui a accès à quel programme / promotion.

---

## 4bis. Connexion / authentification

- Page de connexion : **e-mail + mot de passe** + **« Mot de passe oublié »**
  (lien de réinitialisation par e-mail).
- **Pas de création de compte en self-service** sur la plateforme (les comptes
  viennent de Notion ou sont créés par un admin — cf. §14).

---

## 5. Arborescence du catalogue

```
ODBI Academy
├── Univers LA VOIE (bleu + vert)
│   ├── La Voie                    (programme)
│   ├── La Voie des managers       (programme long)
│   ├── La Voie des artisans       (programme long)
│   └── Série « Vous En… »
│       ├── Vous En Mieux (VEM)
│       ├── Vous En Paix  (VEP)
│       ├── Vous Au Top   (VAT)
│       └── Vous En Lien  (VEL)
│
└── Univers L'ÉCOLE (noir + vert)
    ├── Born To Coach (BTC)
    │   ├── BTC : 1ère génération
    │   ├── BTC : 2ème génération
    │   └── BTC : 3ème génération
    └── Intelligence Collective (IC)
        ├── IC : Cohésion de groupe
        ├── IC : Coaching d'équipe
        └── IC : Transformation des organisations
```

Chaque **formation/programme** est un cours autonome (modules/sessions,
progression, promotions, groupe WhatsApp, visios propres).

### Catalogue & modèle d'accès (freemium)
- Menu : **« Les programmes »** (catalogue complet) placé **au-dessus du Tableau de bord**
  (le tableau de bord garde « reprendre mes programmes » → on évite le doublon).
- Le **Livret d'accueil** n'est **plus dans le menu** : c'est une **carte/lien sur le tableau de bord** qui ouvre la page Notion (cf. §18).
- **Tous les cours sont visibles** par tout participant connecté (pas de verrou sur les cartes).
- **Freemium** : sur un cours où le participant **n'est pas inscrit**, une partie du
  contenu est **gratuite** (pour donner envie) ; le **reste est réservé aux inscrits**.
- **Point d'entrée selon l'inscription** :
  - **Inscrit** → arrive sur la **page d'accueil du programme**, **dynamique** (carte « Reprendre
    où vous en étiez » pointant sur sa position courante).
  - **Non‑inscrit** → arrive **directement sur les Préceptes** ; il **n'a pas accès à l'« Accueil »**
    (l'entrée Accueil est masquée).
- **Accès découverte (non inscrit)** : le participant n'a accès **qu'aux Préceptes** (badge **« Bonus »**,
  et non plus « Gratuit »). Il **peut déplier les sessions** dans le menu pour voir la structure, **mais
  le contenu reste verrouillé au centre** (panneau « Réservé aux inscrits »). De même, **Présentiels,
  Participants, sélecteur de promotion et WhatsApp** sont visibles mais **verrouillés**. CTA
  « Rejoindre le programme » dans l'en-tête et sur les sections réservées. (Les Préceptes remplacent
  l'ancienne logique « 1ère session ouverte en découverte ».)
  - **Aucune promotion** : un non‑inscrit n'appartient à **aucune promo**. Le contenu gratuit
    est **asynchrone** (partagé, indépendant des promotions) ; le **synchrone** (visios/replays,
    présentiels, WhatsApp) dépend d'une promotion et nécessite l'inscription. Le sélecteur de
    promotion **n'apparaît pas** en accès découverte. L'admin assigne une promotion lors de
    l'inscription.
- **Présentation du catalogue** : chaque **univers** (La Voie / L'École) est présenté dans un **bloc/panneau distinct**
  (fond blanc, **accent couleur en haut** : bleu pour La Voie, foncé pour L'École, en-tête avec filet, sous-sections
  à liseré coloré) → séparation visuelle marquée entre univers et groupes de programmes.
- Dans le catalogue : cours inscrits = barre de progression ; cours non inscrits =
  mention « 🔒 Réservé aux inscrits · 🔓 Bonus disponible à tous » (icônes cadenas fermé / ouvert).

---

## 6. Promotions (point structurant : async vs synchrone)

Un programme se décline en **plusieurs promotions** (cohortes).

| Type | Contenu | Portée |
|------|---------|--------|
| 📦 **Asynchrone** | Sessions (vidéos, audios, PDF, exercices, quiz), lexique, références | **Partagé** par toutes les promotions (ne bouge pas) |
| 👥 **Synchrone** | Classes / visios, replays, présentiels, groupe WhatsApp | **Propre à chaque promotion** |

### Identité d'une promotion
- Nom lisible : `Programme · Promo millésime — saison` (ex. *La Voie des managers · Promo 2026 — Automne*)
- Code unique (slug, ex. `lvm-2026-aut`)
- Dates début/fin, animateur, liste des membres (cohorte)

Un membre est rattaché à **une promotion par programme** (peut être dans
plusieurs promos s'il suit plusieurs programmes). La plateforme filtre
automatiquement le contenu synchrone selon sa/ses promo(s).

**Rattachement (source d'inscription)** : l'inscription d'un participant à un
cours se fait depuis la **base Notion « participants »** (via un bouton). Le LMS
provisionne alors le compte, le rattache au cours + à la promotion, et **envoie
les accès de connexion** par e-mail (cf. §13, flux Notion → LMS).

### Volume & navigation des promotions
- **Beaucoup de promotions** : ex. « La Voie » → **~5 promotions par an**
- Côté UI : sélection des promotions via un **dérouleur groupé par année**
  (pas une liste de chips qui déborderait)
- À prévoir : archivage des promotions passées

---

## 6bis. Navigation (architecture de l'information)

Inspirée des LMS type Skool :
- **Menu principal en haut** (barre horizontale, texte sans emojis, onglet actif souligné) :
  Les programmes · Tableau de bord · Visios live · YoDalf · Progression · Agenda · Back-office.
  (Le **Livret d'accueil** n'est plus dans le menu : lien Notion depuis le tableau de bord, cf. §18.)
- **Barre contextuelle à gauche** = **un seul panneau persistant** (carte), affiché aussi bien sur l'accueil
  du programme que dans une leçon. Il remplace l'ancien double-état (sections programme / accordéon de session)
  qui faisait « perdre » l'utilisateur. **Sections repliables** (chevron) pour rester lisible. De haut en bas :
  1. **« Accueil du programme »** (page d'accueil). Elle présente **trois cartes d'action** (pas de bouton
     « Reprendre » redondant en en-tête) : **① Reprendre où vous en étiez** (dernière leçon),
     **② Prochaine visio live** (si une visio est programmée → bouton *Rejoindre la visio*),
     **③ Prochaine séance Training/Coaching** (si programmée → bouton *Voir la séance*, libellé contextuel
     selon l'univers). Ces cartes synchrones ne s'affichent **que pour les inscrits**.
  2. **« Introduction »** (section repliable, badge **« Bonus »**, accès libre — inscrits ou non),
     composée de **deux pages** :
     - **« Présentation du programme »** = **une seule vidéo** (page lecteur 16/9).
     - **« Préceptes »** = **plusieurs vidéos** bonus (grille). Chaque vidéo ouvre **son propre lecteur**
       (16/9, titre, auteur/durée, description) avec un lien **« Retour aux Préceptes »**.
     (Remplace l'ancienne grille « vidéos d'accueil » de la page d'accueil.)
  3. **« Session »** (section repliable) — **regroupe tout le périmètre programme** :
     - **Arbre du curriculum** (façon Skool) : **toutes les sessions** en **arbre dépliant sur 3 niveaux** —
       Session (S1, S2…) → Séquence (« 2.1 — [thématique] »…) → Étape (Exercice, Vidéos, Ressources, Quiz, Replay).
       Lien **« Tout réduire »** dans l'en-tête de section.
     - puis **Lexique** et **Références** (au même endroit que le curriculum).
     - **Une seule session dépliée à la fois** ET **une seule séquence dépliée à la fois** (ouvrir 2.2 referme 2.1)
       pour éviter un menu qui s'allonge à l'infini.
     - **Lecteur vidéo à gauche** + **panneau d'infos à droite** (mise en page côte à côte, pour remplir la largeur
       de façon harmonieuse) : titre de la vidéo, **courte description**, puis actions (« Reprendre à … »,
       « Marquer comme vu »). Les chips de sélection des vidéos restent au-dessus ; Précédent/Suivant en bas.
     - **Bouton « Chapitrage »** (toggle) : au clic, affiche/masque le **chapitrage de la vidéo** (timestamps cliquables, **importé de Vimeo**), **en pleine largeur sous la vidéo**. Le lecteur reste en **16/9**.
     - **Affichage allégé** : chaque étape = **case à cocher + libellé + durée** (façon Skool), **sans icône de type**
       dans l'arbre (les icônes SVG de type restent dans la zone de contenu).
     - **Cases vertes auto-cochées** une fois l'étape terminée ; pastille verte (partielle/complète) sur les
       parents séquence/session ; **cadenas** sur le contenu verrouillé.
  4. **« Promotion »** (section repliable, synchrone) : **sélecteur de promotion** (dérouleur par année, en tête
     de la section — pas de libellé « Promotion » redondant), puis **Coaching/Training, Présentiels, Participants, WhatsApp**.
     Sélecteur masqué en accès découverte (non-inscrit = sans promotion).
  5. **« Certification »** (section repliable **en bas du menu**, badge **RNCP / RS / interne** ; **masquée si le
     programme n'est pas certifiant**) : **« Mon parcours de certification »** → déroulement, **niveaux (Niveau 1 / Niveau 2)**,
     **ateliers d'évaluation** (cliquables → sa grille en lecture), **référentiel**, **mon résultat + mon diplôme**.
     Le **PV (par promotion)** et la **délivrance** restent côté **jury/BO**, pas dans l'espace participant. Réservé aux inscrits (cf. §11ter).
- **Pas de recherche dans le menu** : une **seule** zone de recherche, **globale, dans la barre du haut**.
- **Scrolls indépendants** : la colonne de gauche (menu) et la zone de contenu (centre) **défilent séparément** ;
  cliquer un élément du menu après l'avoir fait défiler affiche le contenu **en haut** de la zone centrale.
- **Règle de complétion d'une étape** (ce qui coche la case) : vidéo = vue ≥ 90 % · exercice = déposé ·
  ressource = ouverte · quiz = au moins 1 tentative · replay = ouvert. (Sert aussi à la progression Qualiopi/BPF.)
- **Aucune emoji** dans l'interface : les types de contenu (exercice, vidéo, audio, PDF, quiz, visio, verrou, cloche)
  utilisent des **icônes SVG**. Seules restent les emojis d'identité explicitement validées (YoDalf, badges).
- **Contenu à droite** : fil d'Ariane + progression ; navigation étape par étape (Précédent/Suivant) possible.
- Objectif : une **seule colonne de navigation** persistante (menu global en haut, arbre + infos à gauche),
  pas de double menu, pas de bascule déroutante.

---

## 6ter. Formats pédagogiques & types de programme

Un programme porte un **type** (BO → éditeur → **« Type de programme »** : Parcours long · Format court ·
Individuel — Tronc commun / Bilan / VAE) + une **« Modalité »** (distanciel/présentiel/mixte) + une
**« Sanction / clôture »** (*Certification titre RS/RNCP* · *Documents & livrables (bilan/VAE)* · *Attestation* · *Aucune*)
qui **pilote la section du bas de menu** côté membre, et un **référent** (individuel uniquement).
Le **catalogue « Les programmes »** est organisé en **3 onglets** (façon navigateur) :
**Programmes longs · Programmes courts · Programmes individuels**.

### 1) Parcours long
- Structure complète : **sessions → séquences → étapes**, **promotions**, pédagogie inversée, certification, etc.
- Ex. **La Voie des managers**, **Born To Coach**. (Onglet « Programmes longs ».)

### 2) Format court
- **1 à 2 jours**, en **présentiel** ou **distanciel** (ou mixte). (Onglet « Programmes courts ».)
- **Même architecture pédagogique que les programmes longs** (sessions → séquences → étapes) : **pas de vue
  dédiée**. Ce qui le rend « court », c'est simplement que **peu de choses sont renseignées** — et **tout ce qui
  est vide est masqué** (cf. principe « vide → masqué » ci-dessous). Ex. la **série « Vous En… » (VE)**.

### 3) Programmes individuels (onglet dédié)
Onglet **« Programmes individuels »**, dans l'ordre : **① Tronc commun d'accompagnement**, **② Bilan de
compétences**, **③ VAE** (validation des acquis de l'expérience).
- **Tronc commun = le programme d'accompagnement de La Voie, adapté individuellement** ; les bilans/VAE s'appuient dessus.
- **Même structure que les programmes longs** (sessions → séquences → étapes), **mais les « sessions » sont
  renommées en phases** (nommage libre au BO) :
  - **Tronc commun** : phases type La Voie (dont une phase **certification titre RS**).
  - **Bilan de compétences** : *Phase préliminaire → Investigation → Conclusion*.
  - **VAE** : *Recevabilité → Accompagnement → Dossier (livret 2) → Jury VAE*.
- Ces **3 programmes sont individuels** : **pas de cohorte/promotion** → **sessions individuelles (1:1)**,
  calendrier propre à chaque personne.
- **Menu de gauche adapté au 1:1** : « Accueil du programme » → **« Mon accompagnement »** ; « Session » →
  **« Mon parcours »** ; « Promotion » → **« Mon suivi »** ; **Participants supprimé**, **promo/cohorte masqués**,
  **Présentiels masqués s'il n'y en a pas**, **WhatsApp → « Mon formateur »** (lien direct vers le formateur qui suit la personne).
- **Certification vs Documents & livrables** (section en bas du menu, selon le programme) :
  - **Tronc commun** → **Certification** (titre **RS**) — même module que les longs.
  - **Bilan de compétences & VAE** → la section devient **« Documents & livrables »** (remplace « Certification ») :
    le **jury / la commission** (membres, date de passage), le **rapport & procès-verbal** (ex. PV du jury VAE,
    document de synthèse du bilan), et les **livrables & pièces** (convention, dossiers livret 1/2, consentement…).
    Documents **confidentiels** (bénéficiaire + formateur/référent + jury VAE le cas échéant).
- En 1:1, l'entrée **« Classes » s'appelle « Séances »** (séances individuelles, pas des classes de groupe).

### Structure flexible — principe « vide → masqué »
Le modèle ne force rien : **une session, une séquence ou une étape sans contenu n'apparaît pas côté membre**
(et une **section de menu vide non plus**). C'est ce principe qui rend un « format court » léger sans page dédiée.
- **Étapes optionnelles** : une séquence peut ne pas avoir d'**exercice**, de **ressources**, de **quiz**…
- **Séquences optionnelles** : si une session n'a **qu'un seul contenu**, inutile de créer des séquences.
- **Nommage libre (BO)** : sessions et séquences sont **nommées librement** (« Phase préliminaire », « Jour 1 »…) ;
  la numérotation S1 / 2.1 est **facultative** (champs éditables dans l'éditeur de programme/session).
- **Replays regroupés dans « Promotion », renommés « Classes »** (pour les programmes **longs et courts**) :
  pas forcément un replay par session → les **classes** (visios enregistrées) sont rassemblées dans l'entrée
  **« Classes » de la section Promotion** (placée **au-dessus de Coaching/Training**), plutôt que dans chaque
  session. Chaque replay donne accès à **Revoir · Transcription · Chapitrage · Résumé · YoDalf**.
- **Ressources = lien externe possible** : une ressource peut être un **fichier** (PDF, audio…) **ou un lien
  externe** (s'ouvre dans un nouvel onglet).
- **Exercices à plusieurs** : un exercice peut être **individuel ou en groupe** (notamment à l'École, en
  **Intelligence Collective**) — avec **dépôt individuel ou dépôt commun** au sous-groupe et **restitution commune**.
- **Exercice = fiche PDF et/ou vidéo support** : un exercice peut porter une **fiche (PDF)** et/ou un
  **lien vidéo** (Vimeo/YouTube) — une **vidéo qui peut faire l'objet de l'exercice** (front : bouton
  « 🎬 Vidéo de l'exercice » ; BO : champ lien dans l'éditeur d'exercice).

### Déverrouillage du contenu **par promotion** (point structurant)
Le contenu (asynchrone) est **partagé par toutes les promotions**, mais **quand/comment il se débloque**
est **propre à chaque promotion** → réglé au **BO → Promotions → gestion d'une promo → onglet « Déverrouillage »** :
- **Mode de déverrouillage** (par promo) : *séquentiel* (pédagogie inversée), *au calendrier* (ouverture datée
  par session), *tout ouvert*, ou *manuel*.
- **Déverrouillage total OU partiel, par session/séquence** : ex. ouvrir *les vidéos seules*, *sans l'exercice*,
  *théorie seule*, ou *tout le contenu* — avec une **date d'ouverture** par ligne.
- Ainsi le **même contenu** peut être **ouvert différemment** selon la promo (drip/planning par cohorte),
  sans dupliquer le contenu. (Le déverrouillage **global par programme** de §14 reste, comme réglage par défaut.)

---

## 7. Structure d'un cours

Côté membre, la barre contextuelle gauche regroupe les sections en **deux familles** :

**Programme** (asynchrone, partagé entre promos) :
1. **Sessions** — liste des sessions (contenu asynchrone)
2. **Lexique** — termes clés (niveau cours), avec **index alphabétique** (A–Z) pour filtrer/aller à un terme
3. **Références** — livres, vidéos, articles recommandés (niveau cours), **avec liens cliquables**

**Promotion** (synchrone, par promo) :
4. **Présentiels** — dates + lieux + horaires (par promotion), **affichés les uns sous les autres**,
   avec un volet **« Détails pratiques & documents »** par présentiel : accès/transport, hébergement
   (se loger), règlement intérieur, fiche de l'établissement (liens/documents)
5. **Replays** — classes/visios **enregistrées de la promotion** (BBB + Notta), **regroupées ici**
   (car pas forcément un replay par session), indépendamment des sessions
6. **Participants** — annuaire des membres de la promotion (photo, nom, ville)
7. **WhatsApp** — accès au groupe WhatsApp de la promotion

+ Badge / sélecteur de la promotion du membre (dérouleur par année).

---

## 8. Structure d'une session

Une session **n'est pas un bloc unique** : elle est organisée en **séquences**,
et peut **s'étaler sur plusieurs semaines** (adaptation pédagogique).

### Une session = une suite de séquences
- Une session est une suite de **séquences**, libellées **« 2.1 — [thématique] », « 2.2 — [thématique] »…**
  (numéro de session . numéro de séquence + nom de la thématique). On **n'emploie pas** le mot « Semaine ».
- Une séquence peut contenir **plusieurs vidéos** et **plusieurs ressources**.
- Chaque séquence est une **suite de blocs séparés** (étapes, icônes SVG) :
  1. **Exercice(s)** préparatoire(s)
  2. **Vidéo(s)** (Vimeo — plusieurs possibles)
  3. **Ressource(s)** synchronisées (audios / PDF, plusieurs possibles)
  4. **Quiz** de la séquence
  5. **Replay** de la visio (BBB + transcription/chapitrage/résumé Notta, par promo)
- Affichage : **accordéon** de séquences, avec déverrouillage progressif et **cases vertes de validation**.
- **Plusieurs vidéos** dans une étape Vidéos : affichées en **onglets/chips** au-dessus d'un **lecteur pleine largeur**
  (pas de liste à droite) ; le clic sur une chip change le lecteur, avec « Reprendre / Marquer / Précédent / Suivant ».
- Le **replay est rangé à la fin du bloc de la séquence** (pas dans une liste séparée).

### Mapping session / semaine / thème (flexible)
- **Cas standard : 1 session = 1 semaine.**
- Mais le modèle doit rester **souple** : on peut avoir p. ex. **3 sessions et 12 thèmes**.
  → Ne pas figer « 1 session = 1 semaine » dans le modèle de données ; prévoir un
  découpage configurable (session → séquences/semaines/thèmes).

### Pédagogie inversée (par séquence)
- Dans chaque séquence, l'**exercice se réalise AVANT** la vidéo.
- Le membre **coche « j'ai fait l'exercice »** → **débloque** la vidéo + ressources de la séquence.
- **NON obligatoire** : option « Voir quand même ».
- Activable/désactivable par session (toggle admin).

### Restitution des exercices (dépôt participant → animateur)
- Le participant peut **déposer (uploader) son exercice réalisé** (PDF, photo, doc…)
  directement dans le bloc exercice.
- Ces dépôts sont **visibles par l'animateur** (voir §14 — Suivi pédagogique).

### Vidéos
- **Hébergées sur Vimeo** ; **reprise de lecture** gérée par la plateforme
  (Vimeo Player SDK, position par membre × vidéo, multi-appareils) ; alimente
  progression + assiduité.

### Quiz & replays — multiples par session
- **Plusieurs quiz** par session (au moins un par séquence).
- **Plusieurs replays** par session (≈ 1 visio/semaine) — chacun avec son
  enregistrement BBB + transcription/chapitrage/résumé Notta.
- Voir §9 pour les formes de quiz.

### Replay de la classe (visio live)
- La visio live de la session est accessible **dans la session** (bloc « Classe en direct / Replay »),
  **propre à la promo** : enregistrement BBB + transcription/**chapitrage**/**résumé** Notta + « Questions à YoDalf ».
- Rythme : **~1 visio / semaine par cours** ; programmes de **3 à 12 mois** (volume important).

---

## 9. Quiz

- **Au moins un quiz par séquence** (donc plusieurs par session)
- Un quiz comporte **plusieurs questions** (typiquement **~5**), de **formes variées** :
  QCM, Vrai/Faux, Association (matching), Réponse libre
- **Score affiché**, **sans seuil minimal** de réussite
- **Refaisable** autant de fois que voulu
- **Mémoire des tentatives** (historique : score + date par tentative)

---

## 10. Gamification

- Points ODBI (barème : leçon/session terminée, module complété, visio, connexion quotidienne, cours fini…)
- Niveaux (avec libellé, ex. « Stratège confirmé ») + progression vers le niveau suivant
- Classement (leaderboard) de la communauté
- Badges / accomplissements (obtenus + verrouillés)
- Série de jours (streak)

### Tout se règle au BO (éditeur de programme → « Règles de progression » + « Points, badges & niveaux »)
**Toute la progression est administrable depuis le BO** :
- **Règles de complétion — par type de contenu** : ce qui marque une étape « faite » (vidéo ≥ X %, exercice
  déposé / validé, ressource ouverte / non requise, quiz : 1 tentative ou score ≥ X %, classe ouverte / non requise)
  + **si l'étape compte dans la progression** (toggle par type).
- **Calcul de la progression** (% d'étapes / pondéré) · **séquencement par défaut** (séquentiel/libre) ·
  **étapes prises en compte** (toutes / obligatoires) · **prérequis pour la sanction** (ex. ≥ 100 % + exercices déposés).
- **Gamification** : **points par action** (activables), **niveaux** (nom + seuil), **badges** (nom + condition,
  activables, ajout possible). Alimente la page « Progression » et le profil.
- Rappel : l'**ouverture datée / drip** reste **par promotion** (Déverrouillage) ; ici ce sont les **règles de complétion**.

---

## 10bis. Notifications (gérées au BO — onglet « Notifications »)

Chaque notification est **activable/désactivable par canal**, avec **destinataires** paramétrables, et on peut
**en ajouter** de nouvelles (événement + destinataires + canaux + modèle de message).
- **Canaux** : **cloche (in-app)** · **e-mail** (service transactionnel) · **WhatsApp** (connecteur, si activé).
  Une notification **désactivée n'est ni affichée ni envoyée**.
- **Destinataires** : participant concerné · toute la promotion · coach/animateur · admin.
- **Notifications définies par défaut** : Bienvenue/accès (onboarding) · Nouvelle session / contenu débloqué ·
  Rappel de visio/classe (1h avant) · Replay disponible · Exercice à rendre / relance · Invitation séance
  Coaching/Training · Feedback disponible · Rappel de présentiel · Diplôme/attestation délivré · Badge/niveau
  atteint (gamification) · Annonce de l'animateur.
- **Ajout** : bouton « + Ajouter une notification ». Le formulaire demande un **déclencheur (événement système
  choisi dans une liste** : inscription, contenu débloqué, visio programmée, replay, exercice non rendu, quiz
  complété, séance créée, feedback/document déposé, présentiel, diplôme, badge, annonce… ou **personnalisé**), un
  **moment d'envoi** (immédiat / 1 h avant / 24 h avant / le jour même / 1-3-7 jours après pour les relances),
  un **intitulé interne**, les **destinataires**, les **canaux** et un **message-modèle**.
- Réservé à l'**Admin** (onglet masqué en vue coach).

---

## 10ter. Évaluation interne des coachs (résultats internalisés depuis Notion)

Retour **interne et anonyme** : les **participants notent les coachs de l'École** (sur les **présentiels** et
les **trainings/coaching**). **≠ évaluation Qualiopi de la formation** (obligations légales) — c'est un outil
d'amélioration de la posture des coachs.
- **Le questionnaire reste sur Notion** (la saisie ne se fait **pas** dans le LMS ; les questions **évolueront**).
- **Ce qui est internalisé = les résultats** : le LMS **lit la base Notion des réponses** et affiche un
  **tableau de bord** (remplace la Google Sheet). BO → onglet **« Éval. coachs »** (admin).
- **Note /10** : calculée à partir des réponses (**règle de calcul paramétrable** : méthode — moyenne pondérée
  ramenée sur 10 / somme de points / NPS — et barème des réponses Oui-Non), affichée en **graphique** (barres,
  note /10 par coach) + **tableau détaillé** (nb de réponses, % écouté, % recommandé).
- **Anonyme** (aucune réponse reliée à un évaluateur).
- **Coachs « anciens »** : des coachs ayant **terminé le parcours de l'École** continuent de venir aux
  présentiels/trainings et **restent évalués** (statut « Ancien »).
- Filtres : **contexte** (présentiels / trainings-coaching) et **période**. Export CSV. Le mapping colonnes
  Notion → questions se règle au BO (les nouvelles questions Notion remontent automatiquement).

---

## 11. Visios live & replays (BigBlueButton + Notta)

### Visios
- **Où on les programme** : **BO → Promotions → gérer une promo → onglet « Visios & replays »** → bouton
  **« + Programmer une visio »** (formulaire : séquence rattachée, titre, date/heure, durée, récurrence).
  La **salle BBB** est créée automatiquement, l'**enregistrement** activé (replay + Notta) ; la visio remonte
  ensuite dans **Promotion → Replays** côté membre + dans son agenda.
- Classes **synchrones** d'une promotion, via **BigBlueButton**
- Chaque participant voit **uniquement les visios de son cours ET de sa promo** (croisement inscription × cours × promo)
- **Pas d'inscription** aux visios : accès **automatique** via l'appartenance à la promo
- **Visios du jour dans le menu de gauche** : accès rapide pour rejoindre BBB en un clic
- Menu « Visios live » = agenda **filtré** par les promos du membre + prochaines visios
- Rythme : **~1 visio / semaine par cours**

### Hébergement BBB (existant ODBI)
- BBB **auto-hébergé chez OVH** (serveur **TURN** inclus)
- Domaine associé : **https://www.classe-virtuelle.com/**
- Actuellement utilisé via **Moodle** → à ré-intégrer dans la nouvelle plateforme (voir §17)

### Replays
Flux : `Visio BBB → enregistrement BBB → Notta (transcription + chapitrage + résumé, multi-langues) → objet Replay rattaché à la promo`

- **Accès réservé aux membres de la promotion** concernée 🔒
- Les replays sont rangés **dans chaque session du cours** (onglet « 🎥 Replay »), pas dans une liste globale (volume important)
- Actions par replay (boutons qui **affichent le contenu dessous**) : ▶ Revoir (BBB) · 📝 Transcription (Notta) · 📑 **Chapitrage** (Vimeo/Notta) · 📄 Résumé (Notta) · 🧙 Questions à YoDalf
- **Résumé = fait par Notta** (transcription + chapitrage + résumé proposé)

---

## 11bis. Séances d'entraînement (pratique du coaching)

Module **distinct des visios de promo** : les membres de **l'École** s'entraînent à coacher, se font
enregistrer, et reçoivent des retours (pairs + coach + IA).

### Accès (dans le programme, pas dans la barre du haut)
- **Pas d'entrée dans le menu du haut** (allégé). La fonctionnalité vit **dans le menu de gauche du programme**,
  **dans la section « Promotion »**, avec un **libellé contextuel** :
  - **« Training »** dans les programmes de **l'École** → le membre **programme** ses séances.
  - **« Coaching »** dans les programmes de **La Voie** → le membre **participe** comme coaché (sur invitation) :
    **lecture seule** — il voit « Mes séances » mais **ni « Programmer une séance » ni « À évaluer »** (réservés à l'École/coach).
- **Libellé identique dans l'agenda** : l'événement de séance porte le **même libellé contextuel** —
  **« Training »** pour un membre de l'**École**, **« Coaching »** pour un membre de **La Voie** (pas de terme « Entraînement »).
- Les **invités** (qui n'ont pas forcément l'entrée dans leur menu) sont prévenus par **notification** (cloche
  en haut + e-mail) ; chaque notification ouvre directement la séance (**page séance autonome**).

### Création d'une séance
- **Réservée aux membres de l'École** (BTC / IC). Les membres **La Voie ne créent pas** de séances.
- Le créateur (= **Coach**, celui qui s'entraîne) choisit date/heure/durée ; **salle BBB créée automatiquement**,
  **enregistrement activé** (→ **transcription Notta** ; **pas de chapitrage ni de résumé** pour ces séances).
- **Rôles** à désigner : **Coach** (s'entraîne) · **Coaché** (invité via l'**annuaire**, École ou La Voie) ·
  **Coach / animateur évaluateur** (peut être d'**un autre programme** : chez ODBI, dès qu'un coach est nommé,
  il peut donner un feedback) · **Participant supplémentaire** · **Observateur(s)**.
- Chaque invité reçoit une **notification** (accepte / refuse).
- **Visibilité du replay** : **privée par défaut** (invités + coach/animateur), élargissable (ma promo / inter-promos). RGPD.

### Visionnage & feedback
- **Supervision = Coach/Animateur** (pas de rôle « Superviseur » dédié).
- **Feedback humain = vidéo (Loom) OU document** : déposé par le **coach/animateur**, les **observateurs** et le
  **participant supplémentaire** (vidéo écran+voix, ou fichier/grille). Chaque feedback **vidéo** est **transcrit par Notta**.
- **Feedback YoDalf = écrit** : généré **depuis la transcription** (points forts / axes / suggestions horodatés).
- **Confidentialité** : les **feedbacks sont visibles par tous les participants de l'École** de la séance
  (coach/animateur, participant invité, observateur, **et le coaché s'il est lui-même de l'École**).
  **Seul un participant de La Voie n'y a pas accès** (ni Loom, ni document, ni YoDalf).
- **Accès de l'évaluateur** (coach d'un autre programme) : via la **notification** + une liste **« À évaluer »**
  (la page séance étant autonome, l'appartenance au programme n'est pas requise).

### Intégrations mobilisées
- **BigBlueButton** (salle + enregistrement), **Notta** (transcription de la séance **et des feedbacks**),
  **Loom** (feedback vidéo — connecteur, compte ODBI existant), **YoDalf/Claude** (feedback écrit).

### Modèle de données
- `SeanceEntrainement` : créateur, date, salle BBB, participants[] (rôle : coach / coaché / coach‑animateur /
  participant / observateur), programmes concernés, visibilité, enregistrement, transcription.
- `Feedback` : séance, auteur, type (**vidéo Loom** + transcription Notta **ou document**), URL/fichier, date,
  **visible_participants_ecole = true** (exclut les participants La Voie) — + `FeedbackYoDalf` (écrit, auto, même restriction).
- **Suivi pédagogique (BO)** : alimenté automatiquement par les séances (séances réalisées comme coach / coaché,
  feedbacks reçus & donnés, dernier feedback) — pris en compte dans l'assiduité (Qualiopi).
- **Vue promo (BO)** : la gestion d'une promotion (BO → Promotions → gérer) comporte un onglet
  **« Coaching / Training »** listant les séances de la promo (date, coach ↔ coaché, coach/animateur, statut,
  replay/feedbacks) — les séances sont **créées par les membres** côté programme ; le BO en offre le **suivi**
  et l'**affectation d'un coach/animateur évaluateur**.

---

## 11ter. Certification (évaluation finale)

Un programme **peut** se terminer par une **évaluation finale** qui fait office de **certification**.

### Programme certifiant — ou non
- Un programme peut être **certifiant** ou **non certifiant** (paramétrable au BO).
- **Si non certifiant** : la section **« Certification » est masquée** côté membre ; le participant reçoit
  une **attestation de suivi** en fin de parcours (déposée dans son profil).

### Adossement (RS / RNCP / interne)
- Un programme certifiant peut être **adossé à un titre RNCP**, à une **certification RS** (Répertoire spécifique),
  **ou à aucun** (« certification interne ODBI » non adossée).
- Le **type d'adossement** et le **code + intitulé** du titre sont **paramétrables par programme** (BO).
- Le badge correspondant (**RNCP / RS / interne**) est affiché dans le menu de gauche du programme
  (section **« Certification »**, **en bas du menu**) et rappelé sur le diplôme et le procès-verbal.

### Ateliers d'évaluation (1 à 3 par programme)
- Un programme comporte **1 à 3 ateliers**, **de natures différentes** et configurables :
  **entretien, présentation, étude de cas, jeu de rôle, quiz**, etc.
- Chaque atelier porte : type, intitulé, durée, **coefficient**, et un **statut** côté candidat
  (à planifier / planifié / rendu / réussi…).

### Portée : programme (partagé) vs promotion (propre)
Même principe que l'async/synchrone (cf. §6) :
- **Au niveau du programme — commun à TOUTES les promotions** : le fait d'être certifiant ou non,
  l'adossement (RNCP/RS/interne), les **niveaux**, le **référentiel** (blocs + indicateurs) et les **ateliers**.
  On les **définit une seule fois** ; ils s'appliquent automatiquement à chaque promotion.
- **Au niveau de la promotion — propre à chaque session** : le **jury**, les **dates**, les **candidats**,
  les **grilles** renseignées, le **procès-verbal** et la **délivrance des diplômes**.
- Au BO, l'onglet sépare visuellement **① Paramétrage du programme** et **② Session de certification (promotion)**.

### Versionnement & instantané (snapshot) par promotion
Problème : le référentiel/ateliers/paramètres sont au niveau **programme** (partagés), mais s'ils évoluent un jour,
les **promotions déjà certifiées** ne doivent pas être impactées (le **PV doit citer le référentiel en vigueur le
jour du jury** — exigence Qualiopi). Solution retenue :
- **Modèle de programme versionné** : chaque enregistrement du paramétrage (référentiel/ateliers/niveaux) crée une
  **nouvelle version** (v1, v2, v3… + date). Une **« version courante »** s'applique aux **nouvelles** promotions.
- **Instantané (snapshot) figé par promotion** : au **lancement de la session de certification** d'une promotion,
  le modèle est **copié et figé** sur cette promotion. Les évolutions ultérieures du programme **ne la modifient pas**.
  C'est **cette version figée** qui est référencée dans le **procès-verbal**.
- **Édition par promotion (dérogation)** : on peut **éditer la copie figée** d'une promotion sans toucher au modèle
  programme (ajustement ponctuel).
- **Copier depuis une promo précédente** : initialiser la session d'une nouvelle promotion à partir de la version
  d'une **promotion antérieure** (plutôt que de la version courante).
- **Resynchroniser** : optionnellement, réaligner une promotion non encore figée sur la **dernière version** du modèle.
- Au BO (bloc ②), un encart **« Modèle d'évaluation de cette promotion »** affiche la **version figée** et propose
  *Éditer pour cette promotion* / *Copier depuis une promo précédente* / *Resynchroniser*.

### Référentiel de compétences & indicateurs
- Le programme expose son **référentiel** : **blocs de compétences** → **compétences** → **indicateurs**
  (critères observables évalués par le jury).
- **Éditable dans le BO** : on ajoute/supprime **autant de blocs et d'indicateurs que voulu** (aucune limite),
  ou on **importe** depuis un PDF France compétences. (Les **ateliers** restent limités à **1 à 3**.)
- **Défini au niveau du programme** → **identique pour toutes les promotions**.
- Côté candidat : consultable en accordéon dans la section **Certification** du programme.

### Grille de notation & niveaux
- Chaque atelier est noté via une **grille** : pour chaque **indicateur**, le jury positionne le **niveau atteint**
  (barème ex. **Non acquis 0 / En cours 1 / Acquis 2 / Expert 3**) + une **observation**.
- La grille calcule un **score d'atelier** et une **contribution au niveau visé**.
- Un programme peut prévoir **1 ou 2 niveaux de certification** ; **quand il y a 2 niveaux, on les nomme
  « Niveau 1 » et « Niveau 2 »** (seuils paramétrables, ex. Niveau 1 ≥ 60 % par atelier ; Niveau 2 ≥ 85 % de moyenne).
  Avec 1 seul niveau : « Niveau 1 ».
- **Vue jury** : saisie des grilles. **Vue candidat** : consultation de ses résultats (lecture seule).

### Jury de certification
- Le **coach/animateur du programme nomme le jury** mais **n'en fait pas partie** (il a accompagné la promotion).
- Le jury comprend **au moins 2 personnes**, dont **un·e président·e** (coachs labellisés).
- Constitution du jury **au BO** (depuis l'annuaire).

### Procès-verbal du jury (PV) — **par promotion**
- Le PV est **établi par promotion** (un **seul document** pour toute la session), **pas un PV par candidat**.
- Contenu (modèle ODBI) : **organisme de formation**, **objet** (promotion), **date & lieu**,
  **composition du jury** (président + membres, présence), **liste des candidats & décisions individuelles**
  (Niveau 1 / Niveau 2 / Ajourné), **déroulement des délibérations** (procédure, statistiques générales),
  **remarques & recommandations**, **signatures** (président + membres), **annexes**.
- Actions jury : **Valider & signer le PV**, **Télécharger (PDF)**, **Délivrer les diplômes (toute la promo)**.
- Le PV est un **document du jury** (espace jury / BO) — **il n'apparaît pas dans l'espace du participant**.

### Délivrance du diplôme — **par le jury**
- **La délivrance est une action du jury** (depuis le PV ou la liste des candidats au BO) —
  **jamais une action du participant**.
- Le diplôme est **déposé dans le profil** du candidat admis → onglet **« Mes certifications & diplômes »**
  (téléchargeable en PDF), avec **notification + e-mail**. Les candidats **ajournés** reçoivent une **attestation**.
- Le profil **affiche** (lecture seule) : diplômes **délivrés** (avec code RNCP/RS) et **attestations**.

### Vue candidat (section « Certification » du programme)
- Déroulement → **Niveaux** → **Ateliers d'évaluation** (cliquables → sa grille en lecture) → **Référentiel**
  → **Mon résultat** (décision du jury) + **Mon diplôme** (vers le profil).
- Le candidat **ne voit pas** le PV de promotion ni le bouton de délivrance.
- Réservé aux **inscrits** (en accès découverte, la section est verrouillée).

### Back-office — onglet « Certification »
Deux sélecteurs en tête : **Programme** (paramétrage commun) et **Promotion** (jury & candidats).
- **① Paramétrage du programme (commun à toutes les promos)** : **certifiant oui/non**, adossement
  (RNCP/RS/interne) + code, **nombre de niveaux** (1 / 2 → Niveau 1 / Niveau 2) + seuils,
  **référentiel** (éditeur de blocs/indicateurs, sans limite), **ateliers** (1 à 3, type/intitulé/durée/coefficient).
- **② Session de certification (propre à la promotion)** : **jury** (≥ 2, dont président, coach/animateur exclu),
  **candidats**, **saisie des grilles**, **PV par promotion**, **délivrance des diplômes**.
  **Synchronisé avec le suivi pédagogique** et exportable (PDF / registre Notion).
- **Réservé à l'Admin** dans le back-office (paramétrage du référentiel, ateliers, jury, grilles, PV, diplômes).
  L'**Animateur n'y a pas accès** ; le jury proprement dit reste **tiers** (président + ≥ 2 membres, coach/animateur exclu).

### Intégrations mobilisées
- **Notion** (registre des certifications, BPF), **génération PDF** (PV + diplôme + attestations), suivi pédagogique (assiduité Qualiopi).

### Modèle de données
- `Certification` : programme, **certifiant (bool)**, type_adossement (rncp/rs/interne), code_titre,
  **version_courante**, **versions[]** (n° + date + référentiel + ateliers + niveaux + seuils figés).
- `PromotionCertif` (instantané) : promotion, **version_figée** (copie du modèle au lancement), **jury[]**
  (membre + rôle président/membre), dates, statut (figé / éditable / resynchronisable).
- `Bloc` → `Competence` → `Indicateur`.
- `Atelier` : certification, type (entretien/présentation/étude_de_cas/jeu_de_role/quiz…), intitulé, durée, coefficient.
- `Grille` : atelier, candidat, jury[], notes[] (indicateur → niveau + observation), score, contribution_niveau.
- `ProcesVerbal` : **promotion**, **version_référentiel_figée**, jury[], candidats[] (décision individuelle
  Niveau 1/2/ajourné), stats, délibérations, remarques, date, lieu, signatures.
- `Diplome` / `Attestation` : candidat, certification, niveau_obtenu, date_délivrance, fichier_pdf → rattaché au **profil**.

---

## 11quater. Évaluation interne des coachs (anonyme) — à internaliser

Questionnaire par lequel **les participants évaluent les coachs de l'École**, sur les **présentiels** et les
**trainings/coaching**. **Strictement interne et anonyme** — **≠ l'évaluation Qualiopi de la formation**.
Aujourd'hui sur **Tally → Google Sheet anonyme** ; **à internaliser dans le LMS**.

### Côté participant (front)
- Bouton **« Évaluer le coach (anonyme) »** sur un **présentiel** et sur une **séance Training/Coaching**.
- Formulaire **anonyme** (notes /5, échelle, oui/non, texte libre) → écran de remerciement. Aucune réponse
  n'est reliée à un participant.

### Côté BO (onglet « Éval. coachs »)
- **Questionnaire éditable** (questions + type : Note /5, Échelle 1–10, Oui/Non, Texte libre ; réordonnables,
  ajout/suppression). **Source** : Interne LMS / Tally (actuel) / **Notion (à venir)** — les questions
  **évolueront** et seront **synchronisables depuis Notion** (« Importer depuis Notion »).
- **Périmètre** : s'applique aux **Présentiels** et/ou **Trainings/Coaching** (toggles).
- **Résultats anonymes agrégés** (remplace la Google Sheet) : moyennes par question **par coach**, nb de réponses,
  % recommandé, filtres (coach / source), **export CSV** + verbatims.
- **Coachs « anciens »** : ceux ayant **terminé le parcours de l'École** mais qui **continuent d'animer / d'être
  évalués aux présentiels** apparaissent avec un statut « Ancien ». Ils sont rattachés à la promotion
  permanente **Alumni / Coachs École** (cf. ci-dessous).

### Promotion « Alumni / Coachs École »
- Promotion **permanente** (sans date de fin) rattachée au programme **École**, servant de **point d'ancrage**
  pour les coachs sortis des promos en cours mais toujours actifs aux présentiels / trainings.
- Créable/administrable par l'admin comme toute promotion (onglet **Promotions** du BO) ; présente dans la
  maquette à titre d'exemple (18 membres, statut *Permanente*, présentiels *Continu*).
- Permet de continuer à **collecter et rattacher** leur évaluation coach une fois qu'ils ne sont plus dans
  une promotion active.

### Compte rendu personnel du coach (front)
- Chaque **coach évalué** dispose de son propre **compte rendu** de son évaluation, accessible :
  1. depuis une **carte dédiée du tableau de bord** (« Mon évaluation coach » — note actuelle),
  2. depuis un **lien dans le menu de gauche** de la section **Promotion** du cours (« Mon évaluation coach »).
- La page dédiée (`page-mycoacheval`) affiche **uniquement ses propres retours** (jamais ceux des autres
  coachs) : **note globale /10**, nb de réponses, % recommandé, **évolution de la note par promo** (graphe barres),
  **détail par critère** (graphe barres) et **verbatims anonymes** des participants (présentiel / training).
- Source : réponses **anonymes** du questionnaire **Notion**. Objectif : faire progresser la posture d'accompagnant.

### Modèle de données
- `QuestionnaireCoach` : questions[] (intitulé + type), source (interne/tally/notion), périmètre (présentiel/training).
- `EvaluationCoach` : coach, source (présentiel/training), **anonyme (pas de lien participant)**, réponses[], date.

---

## 12. Assistant IA — « YoDalf »

- Mentor IA basé sur **Claude (API Anthropic)**
- Formé sur le contenu des cours (résumés, révisions, Q&R)
- Accessible depuis le menu + depuis une session (« Demander à YoDalf »)
- Illustration dédiée : `assets/yodalf.png` (fournie par ODBI ; repli emoji 🧙)
- **Menu de gauche « Historique des conversations »** (même style/carte que le menu des programmes) :
  bouton **« + Nouvelle conversation »** puis la liste des chats mémorisés, **groupés par date**
  (Aujourd'hui / 7 derniers jours / Plus ancien) — **groupes repliables** (chevron, comme le menu des sessions), chat actif surligné. Permet de **garder en mémoire**
  et rouvrir les échanges précédents.

---

## 13. Intégrations externes

| Service | Usage |
|---------|-------|
| **BigBlueButton** | Visios live + enregistrement (replays) |
| **Notta** | Transcription + chapitrage + résumé des visios (multi-langues) |
| **YoDalf / Claude (Anthropic)** | Assistant IA sur le contenu |
| **WhatsApp** | Groupe communautaire **par promotion** (lien externe) |
| **Notion** | **CRM** + **source des inscriptions** + **extraction Qualiopi / BPF** |
| **E-mailing transactionnel** | Envoi automatique des accès LMS + notifications (service à choisir) |
| **Stripe** | Paiements & abonnements |

### Flux Notion (bidirectionnel — le LMS lit ET écrit dans Notion, et inversement)

**Bases Notion à connecter & mapping**
- **« Session de formation »** ⇄ **Promotion du LMS** : une session Notion **=** une promotion.
  Chaque promotion est **liée à une session Notion** (réglage dans la gestion de promo).
- **« Participants session »** ⇄ **participants d'une promotion** : on **envoie des participants
  dans une promotion** depuis Notion ; ils remontent dans la liste des participants (badge source « Notion »).
- (Plus les bases **CRM** et **BPF/assiduité**, et la page **Livret d'accueil**.)

**① Notion → LMS**
- Depuis « Participants session », **envoyer un participant dans une promotion** (bouton) → côté LMS :
  **création/provisionning du compte**, **rattachement au cours + à la promotion**, **envoi des accès** (e-mail d'onboarding).

**② LMS → Notion**
- **Ajout/retrait d'un participant** dans une promotion **répercuté vers Notion** ; mise à jour des fiches (CRM) ;
  export **assiduité / heures / BPF** (cf. §17). **Sens paramétrable** : bidirectionnel / Notion→LMS / LMS→Notion.

> Implique un **service d'e-mailing transactionnel** pour l'envoi des accès et
> des notifications (bienvenue, identifiants/lien d'activation, rappels visio…).

---

## 14. Back-office d'administration

### Accès au back-office par rôle
> Colonne « Animateur » = animateur d'un ou plusieurs programmes (**vue animateur** restreinte).
> Un **coach** (hors animation) **n'accède pas au back-office** — **sauf s'il est aussi Animateur**
> (rôles cumulables, cf. §4), auquel cas il entre en **vue animateur** avec le périmètre correspondant.
> Le sélecteur de vue BO ne propose donc que **Admin** et **Animateur** (aucune entrée « Coach »).

| Section BO | Admin | Animateur |
|---|---|---|
| **Suivi pédagogique** | tout | ✅ ses programmes uniquement |
| **Certification** (paramétrage, grilles, PV, diplômes — cf. §11ter) | tout | ❌ |
| **Promotions** (membres promo, planning visios) | tout | ✅ promos de ses programmes |
| **Programmes** (contenu, sessions, visios) | tout | ✅ ses programmes (édition) |
| **Évaluation des coachs** (résultats §12ter) | tout | ✅ coachs de son périmètre |
| **Membres & accès** (global, rôles, comptes) | ✅ | ❌ |
| **Notifications** (paramétrage §10bis) | ✅ | ❌ |
| **Intégrations** (BBB, Notta, Notion, Stripe…) | ✅ | ❌ |
| Créer / supprimer un programme | ✅ | ❌ |

- L'animateur voit dans le BO : Programmes (les siens), Promotions (les siennes),
  Suivi pédagogique (les siens), **Évaluation des coachs** (résultats des coachs intervenant
  sur son périmètre). Les onglets **Certification**, **Membres & accès**, **Notifications** et
  **Intégrations** lui sont masqués (la certification — jury, PV, diplômes — reste **réservée à l'admin**).
- L'accès à l'**évaluation des coachs** permet à l'animateur de suivre la qualité de l'accompagnement
  sur ses présentiels / trainings ; les résultats affichés sont **filtrés sur son périmètre** (anonymat conservé).
- Toutes les données sont **filtrées sur son périmètre d'animation** (cf. §4).
- Filtrage à appliquer **côté serveur** (sécurité), pas seulement masquage UI.
- *(À valider : le coach peut-il éditer le contenu de ses programmes, ou lecture seule ?)*

Onglets :
- **Membres & accès** — comptes, rôles, accès aux cours (éditable), activation
  - **Pas d'auto-inscription** : les comptes sont créés **via Notion** (inscription) **ou
    manuellement par un admin**.
  - **Création manuelle d'un compte gratuit** par l'admin, en donnant accès aux
    **programmes et sessions de son choix** (ex. 1ère session en découverte gratuite).
  - **Déverrouillage par programme** : option dans l'éditeur du programme pour **ignorer
    la pédagogie inversée** sur ce programme (accès direct vidéos/ressources) — pas globalement.
- **Cours** — création/édition des cours, sessions, contenus, WhatsApp par cours
- **Promotions** — cohortes par programme, membres, dates, classes, présentiels
- **Suivi pédagogique (animateur)** — accès aux **restitutions des participants** :
  exercices rendus (fichiers téléchargeables), **résultats de quiz** (score, tentatives),
  vidéos vues, statut. Filtrable par programme / promo / séquence — avec option
  **« Toutes les séquences »** donnant des **stats agrégées par participant au niveau
  du programme** (progression, exercices rendus, score quiz moyen, vidéos vues,
  assiduité visios), **actualisées en temps réel**. Export CSV + Notion (BPF).
  - **Détection des décrocheurs / apprenants « à risque »** : le suivi met en avant les participants
    **inactifs** (ex. 14 j sans connexion) ou **bloqués** dans leur progression, pour déclencher une
    **relance** (notification/e-mail). Vue « entonnoir de complétion » par session pour repérer où ça coince.
- **Certification** — paramétrage par programme (adossement RNCP/RS/interne, niveaux, référentiel,
  ateliers 1 à 3), suivi des candidats, **saisie des grilles**, **procès-verbal** et **délivrance des diplômes** (cf. §11ter).
- **Intégrations** — config BBB, Notta, YoDalf, **Loom**, WhatsApp, Notion, Stripe

### Éditeur de cours (back-office)
Doit être **ergonomique** (rechargement manuel des contenus depuis Moodle).

**Principe directeur — séparation programme / promotion** (décision structurante) :
- **Ce qui appartient au PROGRAMME** (asynchrone, partagé entre toutes les promos) :
  Sessions, **Lexique, Références**, et au sein des sessions **Exercices, Vidéos, Ressources, Quiz**.
- **Ce qui appartient à une PROMOTION** (synchrone, varie d'une promo à l'autre) :
  **Participants, Visios + replays, Présentiels, WhatsApp**.
- Conséquence : on **n'édite pas** les visios ni les présentiels dans l'éditeur de programme/session ;
  ils se gèrent dans **Back-office → Promotions** (carte de gestion de la promo sélectionnée).

**Éditeur de cours (programme)** :
- Paramètres : nom, univers, animateur, statut (publié/brouillon), durée, description, image de couverture
- **Déverrouillage du programme** (toggle pédagogie inversée, **par programme** — pas global)
- Onglets d'édition (**uniquement le périmètre programme**) :
  - **Sessions** (ajout / réordonnancement / duplication / suppression)
  - **Introduction** (accès libre/gratuit) : **Présentation du programme** (une vidéo) + **Préceptes**
    (plusieurs vidéos). Chaque vidéo d'une séquence peut aussi être marquée « publique/Gratuit » et remonte
    alors automatiquement dans les Préceptes.
  - **Lexique** (terme + définition)
  - **Références** (type + titre + **lien/URL**)
  - Note rappelant que **Présentiels, visios, participants et WhatsApp se gèrent par promotion**.

**Éditeur de session** :
- Titre, règle d'accès (verrouillage), **toggle « pédagogie inversée »** (gating exercices, activable par session — non bloquant)
- **Découpage en séquences** (« 2.1, 2.2… ») : ajout / réordonnancement / suppression.
- Édition du **contenu de la séquence sélectionnée** via onglets, **au même niveau** :
  **Exercices**, **Vidéos** (Vimeo, plusieurs), **Ressources** (audios/PDF, plusieurs),
  **Quiz** (constructeur ~5 questions). **Pas d'onglet Visio** (la visio est par promotion).

### Gestion d'une promotion (Back-office → Promotions)
Carte de gestion de la promo sélectionnée, avec sous-onglets :
- **Participants** — annuaire/membres de la promo (ajout, rôle, accès).
- **Visios + replays** — **c'est ici que se programment les visios** (et non dans la session) :
  titre, séquence rattachée, animateur, **date complète via calendrier (jour/mois/année)**, heure, durée ;
  **salle BBB créée automatiquement** ; une séquence peut avoir **une ou plusieurs visios** par promo
  (donc plusieurs replays) ; les replays (BBB + Notta) s'attachent à la séquence côté membre.
- **Présentiels** — dates, intitulé, lieu, horaires + **détails pratiques** (accès/transport,
  hébergement, règlement intérieur PDF, fiche établissement PDF).
- **WhatsApp** — lien du groupe de la promotion.

### Déverrouillage du contenu
- **Global par programme** : toggle dans l'éditeur du programme pour ignorer la pédagogie inversée
  (réglage **par défaut**, pas globalement à toute la plateforme).
- **Par promotion (fin)** : onglet **« Déverrouillage »** de la gestion d'une promo — mode (séquentiel /
  calendrier / tout ouvert / manuel) + **ouverture totale ou partielle par session** (vidéos seules, sans
  exercice, théorie seule, tout) avec **date d'ouverture**. Même contenu, ouverture propre à chaque cohorte.
  Voir §6ter.

---

## 15. Stack technique envisagée

- **Front + back** : Next.js (React)
- **Base de données + Auth + Stockage** : Supabase (PostgreSQL)
- **Vidéo** : **Vimeo** (hébergement/streaming) + **Player SDK** (reprise de lecture, suivi de progression)
- **Visio** : BigBlueButton (API)
- **IA** : API Claude (Anthropic)
- **Paiement** : Stripe
- **E-mailing transactionnel** : à choisir (ex. Resend, Postmark, Brevo…) — envoi des accès + notifications
- **CRM / inscriptions / reporting** : Notion (via API)
- À confirmer au démarrage du projet réel.

---

## 16. Intégration technique BigBlueButton

### Existant
- BBB **auto-hébergé chez OVH** (serveur **TURN** pour la traversée NAT/firewall)
- Domaine : **https://www.classe-virtuelle.com/**
- Intégré aujourd'hui à **Moodle** (plugin BBB Moodle)

### Approche d'intégration dans la nouvelle plateforme
La maquette montre une visio « intégrée » (tuiles dans la page). En réalité,
deux modes possibles côté BBB :

1. **Redirection / nouvel onglet (recommandé pour démarrer)**
   - La plateforme appelle l'**API BBB** (`create` + `join`) côté serveur,
     signe la requête avec le **secret BBB**, puis ouvre l'URL de session.
   - Le membre rejoint la salle BBB (interface BBB native, audio/vidéo via TURN).
   - Simple, robuste, réutilise l'existant OVH/classe-virtuelle.com tel quel.

2. **Intégration embarquée (iframe / BBB HTML5 / API frontend)**
   - La salle s'affiche **dans** la plateforme (comme suggéré visuellement).
   - Plus immersif mais plus complexe (CSP, cookies tiers, mises à jour BBB).

> **Reco** : démarrer en **mode 1** (l'app pilote la création/jointure via l'API
> BBB, rejointe en plein écran), puis évaluer l'embarqué si vraiment souhaité.
> Dans les deux cas, la logique « qui peut rejoindre quelle salle » est gérée
> par **notre** plateforme (membre × cours × promo), pas par BBB.

### À récupérer côté ODBI pour l'intégration
- URL du serveur BBB (API endpoint) + **secret partagé** (shared secret)
- Confirmation de la version BBB et de l'activation de l'API d'enregistrement
  (pour récupérer les replays)

### Migration depuis Moodle
- Moodle sert à **lancer les salles BBB** ET à **héberger des contenus de cours**.
- **Pas de migration automatique** : ODBI **rechargera manuellement** les contenus
  dans la nouvelle plateforme. → Aucun import Moodle à développer.
- Côté visio : on rebranche simplement l'API BBB sur la nouvelle plateforme.
- Implique un **back-office d'upload de contenus** ergonomique (vidéos, audios, PDF,
  exercices, quiz) pour faciliter ce rechargement manuel.

---

## 17. Conformité Qualiopi & BPF (traçabilité)

ODBI est un **organisme de formation (OF) certifié Qualiopi** → obligations de
**traçabilité** et de **reporting réglementaire**.

### Le système doit journaliser (« éléments de connexion » / assiduité)
- **Connexions** des membres à la plateforme (dates/heures, durée)
- **Présence aux visios** (entrée/sortie BBB) — émargement numérique
- **Progression** : sessions ouvertes/terminées, vidéos vues, exercices cochés
- **Quiz** : tentatives et scores
- Rattachement systématique à : **membre × cours/formation × promotion**

Ces traces servent de **preuves d'assiduité** (exigence Qualiopi) et alimentent
les statistiques du **BPF**.

### BPF — Bilan Pédagogique et Financier (annuel)
Données à pouvoir produire : nombre de stagiaires, **heures réalisées** par
formation/promotion, etc. (volet financier = à coupler avec Stripe).

### Extraction / export
- **Extraction vers une base de données Notion** avec les **champs souhaités**
  (à définir précisément avec ODBI).
- Réutilise l'intégration **Notion** existante (déjà CRM) — Notion devient aussi
  le réceptacle des données de reporting BPF/Qualiopi.

### Satisfaction (à chaud / à froid) — géré sur Notion
- Les **questionnaires de satisfaction** (à chaud en fin de session, à froid à J+3 mois) et leurs
  relances sont **gérés sur Notion via des automatisations** (comme l'évaluation coach, cf. §10ter),
  **pas** re-développés dans le LMS.
- Le **LMS extrait les données de connexion / d'assiduité** vers Notion (cf. ci-dessus) ; les
  **résultats agrégés** (taux de satisfaction, NPS) peuvent ensuite être **rapatriés/affichés** dans
  le BO au même titre que les résultats d'évaluation coach.

### Émargement & attestations — géré sur Notion
- **Émargement** (présence visios via entrée/sortie BBB, présentiels) et **génération des
  attestations / certificats de réalisation** : orchestrés par **automatisations Notion** à partir
  des **données de connexion/assiduité extraites du LMS** (extraction déjà prévue au BO).

---

## 18. Espace membre — Profil & annuaire

### Profil (accès via l'avatar en haut à droite)
Champs :
- **Photo** — éditable sur le LMS (upload)
- **Prénom**, **Nom**, **Email** — **synchronisés depuis Notion** (inscription), non modifiables sur le LMS
- **Téléphone**, **Ville** — éditables sur le LMS

> Répartition Notion (synchronisé) vs LMS (éditable) à confirmer champ par champ.
> Principe : l'identité d'inscription vient de Notion ; les compléments de profil
> sont éditables sur le LMS.

Le profil affiche aussi :
- **Points & badges acquis** (total points, niveau, badges, série) + lien vers la progression
- **« Mes certifications & diplômes »** — diplômes **délivrés** par le jury (intitulé, niveau/mention,
  programme, date, **code RNCP/RS**, **PDF téléchargeable**) et certifications **en cours / non encore délivrées** (cf. §11ter).
- **Les cours auxquels le membre est inscrit** + sa **progression** — **cartes cliquables** (ouvrent le cours)

### Annuaire des participants
- **Onglet « Participants »** de la page cours (au même niveau que Lexique/Références)
- Champs affichés : **prénom, nom, ville, rôle, e-mail, numéro WhatsApp**
- **E-mail cliquable** (mailto) et **numéro cliquable vers WhatsApp** (wa.me)
- Le **groupe WhatsApp** de la promo est lui aussi un **onglet du cours**
- **RGPD — opt-in** : l'e-mail et le numéro WhatsApp ne sont visibles par les autres
  participants **que si le membre y a consenti** (réglage dans son profil). Par
  défaut masqués ; prénom/nom/ville/rôle restent visibles.

### Livret d'accueil
- **Lien externe vers Notion** (pas d'embed/iframe) : décision d'**épurer le menu** et de **ne pas dépendre
  de l'intégration Notion**. Plus de page interne, plus de souci d'affichage `file://`/`https`.
- Présenté comme une **carte/lien sur le tableau de bord** (titre + courte description + « Ouvrir ↗ »)
  qui **ouvre la page Notion dans un nouvel onglet**.
- **Retiré du menu principal (barre du haut)** → menu allégé.
- Source : Livret d'Accueil Client ODBI (Notion), page publiée :
  `https://odbi.notion.site/Livret-d-Accueil-Client-ODBI-13d94934418a80cb843dfaebb32328ef`.

### Agenda
- Présenté comme un **vrai calendrier** (vue mensuelle), filtré sur les promos du membre.
- Événements color-codés et **cliquables** (renvoient au contenu concerné) :
  **Visios (BBB), Présentiels, Exercices, Ressources à consulter, Quiz, Séances d'entraînement (Coaching/Training)**.
- **Export / abonnement calendrier** (retenu) : lien **iCal (.ics)** + **« Ajouter à Google Calendar »** pour
  synchroniser visios et séances Training/Coaching dans l'agenda personnel du membre (abonnement mis à jour
  automatiquement), en plus du bouton « Mon agenda » ponctuel.

---

## 19. Points en suspens / à décider

- [ ] Groupe WhatsApp : **par promotion** (hypothèse retenue) ou par programme ?
- [x] Gating exercices : **configurable par session** (toggle « pédagogie inversée » dans l'éditeur)
- [ ] Lexique : uniquement par cours, ou aussi un lexique global plateforme ?
- [x] Sens de la synchro Notion : **bidirectionnel** — Notion→LMS (inscriptions + envoi des accès), LMS→Notion (CRM + BPF)
- [ ] Service d'**e-mailing transactionnel** à choisir (envoi des accès, notifications)
- [ ] **Plan Vimeo** adapté (Pro/Business+) requis pour Player SDK + confidentialité par domaine

---

## 20. Arbitrage des améliorations (juillet 2026)

Suite à la revue de propositions d'améliorations, décisions prises :

| # | Proposition | Décision | Où c'est traité |
|---|---|---|---|
| 1 | **Communauté intégrée** (feed, commentaires par leçon, entraide) | **v2 / plus tard** | §21 (Évolutions v2) |
| 2 | **Mobile-first / PWA** | **Retenu** — principe **décidé maintenant**, **implémenté au dev** | §15 + Backlog UX (chantier prioritaire) |
| 3 | **Reprise de lecture vidéo** | **Déjà prévu** | §8, §11, §15 (Vimeo Player SDK) |
| 4 | **Satisfaction à chaud / à froid** | **Géré sur Notion** (automatisations) ; résultats rapatriables au BO | §17 |
| 5 | **Émargement & attestations** | **Géré sur Notion** (automatisations) à partir des données de connexion extraites | §17 |
| 6 | **Détection des décrocheurs / à risque** | **Retenu** — dans le **BO → Suivi pédagogique** | §14 |
| 7 | **Recherche globale** | **Retenu** | Backlog UX |
| 8 | **Export / abonnement agenda (iCal, Google Cal)** | **Retenu** | §18 (Agenda) |
| 9 | **Onboarding 1ʳᵉ connexion** (visite guidée) | **Retenu (déjà listé)** | Backlog UX, §14 |
| 10 | **Consultation hors-ligne** | **Retenu** sous forme de **cache temporaire chiffré** (voir ci-dessous) ; **pas** de téléchargement de fichiers bruts | ci-dessous |
| 11 | **YoDalf proactif** (relances, révisions, répétition espacée) | **v2 / plus tard** | §21 (Évolutions v2) |
| 12 | **Architecture de production** | **Confié à un développeur** (qui pourra s'appuyer sur Claude) ; la maquette sert de référence UX | §15 |

### Point 10 — consulter sans connexion (réponse technique)
Visionner **sans téléchargement de fichier** que l'utilisateur conserverait n'est pas possible : pour lire hors-ligne,
le contenu doit **exister localement** d'une manière ou d'une autre. La bonne approche (type Netflix/Spotify) :
- **Cache temporaire géré par l'application** (via une **PWA / app mobile**) : le contenu récemment consulté (ou
  explicitement « mis de côté ») est **stocké de façon chiffrée**, **non exportable**, et **expire** automatiquement.
- L'utilisateur ne récupère **pas** un fichier vidéo réutilisable → la **protection du contenu (RGPD/Vimeo)** est préservée.
- Ce mécanisme **dépend de la PWA** (point 2) : à cadrer au développement, pas indispensable à la V1.

---

## 21. Évolutions v2 (à étudier plus tard)

Pistes conservées pour une **version ultérieure**, non incluses dans le périmètre courant :

- **Communauté intégrée (« esprit Skool »)** : fil de discussion par promotion, **commentaires sous chaque
  leçon / replay**, entraide entre pairs, posts épinglés des coachs — pour garder les échanges **dans** le LMS
  (aujourd'hui via WhatsApp externe) et enrichir la donnée pédagogique.
- **YoDalf proactif** : au-delà du chat, un mentor IA qui **relance** (contenus non terminés), génère des
  **fiches de révision** et applique la **répétition espacée** sur les concepts clés — dans la posture « coaching » d'ODBI.
- [ ] Contenu exact de l'**e-mail d'onboarding** (identifiants vs lien magique d'activation ?)
- [ ] Coach : peut-il **éditer le contenu** de ses programmes, ou **lecture seule** (suivi + visios) ?
- [ ] Profil : répartition exacte des champs **Notion (synchro)** vs **LMS (éditable)**
- [x] Annuaire RGPD : **opt-in** — e-mail & WhatsApp visibles seulement si le membre consent
- [ ] Modèle d'abonnement / tarification (Stripe) : à définir
- [ ] Langues de l'interface (FR seul, ou multilingue ?)
- [ ] BBB : mode **redirection** (reco) ou **embarqué (iframe)** dans la plateforme ?
- [x] Migration contenus Moodle : **non** — rechargement manuel par ODBI (back-office d'upload à soigner)
- [ ] BPF/Qualiopi : **champs exacts** à exporter vers Notion + durée de conservation des traces
- [ ] Émargement : signature électronique requise, ou trace de connexion suffisante ?

---

## Annexes — livrables de conception

- `maquette-odbi-lms.html` — maquette interactive (écrans)
- `architecture-odbi.html` — schéma d'organisation (plan d'ensemble)
- `modeles-session.html` / `modele-A-variantes.html` / `modele-A-integration.html` — explorations de navigation
- `odbi-design-tokens.css` / `.json` — design system

---

## Backlog UX (idées à conserver pour plus tard)

- **« Reprendre »** omniprésent (retour exact à la dernière position vidéo/étape)
- **Recherche globale** (cours, sessions, lexique, ressources)
- **« Ma prochaine action »** sur le dashboard (exercice/visio/replay à faire)
- **Rappels & notifications** (visio imminente, exercice à rendre, nouveau replay, badge)
- **Onboarding** 1ʳᵉ connexion (visite guidée + Livret d'accueil)
- **Responsive / mobile-first** (chantier prioritaire — usage smartphone)
- **Lecteur vidéo** : vitesse, « vu » auto à 90 %, chapitres cliquables
- **Téléchargement hors-ligne** des PDF/audios
- **YoDalf contextuel** (pré-rempli selon le contenu) + plan de semaine + relances
- **Dashboard animateur** : alertes retard, exercices reçus, assiduité
- **Gamification vivante** : animations points/badges, classement promo, défis
