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
| **Membre** | Suit les cours, participe aux visios/présentiels de sa promo |
| **Coach / Animateur** | Anime les visios, suit les promotions des programmes qu'il anime |
| **Admin** | Gère contenu, membres, accès, promotions, intégrations |

### Modèle de droits (rôle × périmètre)
- **Participant** : accès **uniquement à sa promo** (programmes où il est inscrit).
- **Coach / Animateur** : **affecté à un ou plusieurs programmes** (table d'association
  *animateur ↔ programmes animés*). Il a accès à **toutes les promos de ces programmes**
  (suivi pédagogique, visios, présentiels) et **à rien** sur les programmes qu'il n'anime pas.
- **Admin** : accès à **tout**.
- Le **périmètre d'animation** d'un coach se gère dans le back-office (Membres → Périmètre :
  cases à cocher des programmes animés). Les vues du coach (Suivi, Promotions, Planning visios)
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
- Le **Livret d'accueil** est placé **au-dessus des programmes** dans le menu.
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
  Livret · Les programmes · Tableau de bord · Visios live · YoDalf · Progression · Agenda · Back-office.
- **Barre contextuelle à gauche** = **un seul panneau persistant** (carte), affiché aussi bien sur l'accueil
  du programme que dans une leçon. Il remplace l'ancien double-état (sections programme / accordéon de session)
  qui faisait « perdre » l'utilisateur. **Sections repliables** (chevron) pour rester lisible. De haut en bas :
  1. **« Accueil du programme »** (renvoie à la page d'accueil : présentation + carte « Reprendre »).
  2. **« Préceptes »** (section repliable, badge **« Bonus »**) : vidéos fondamentales du programme,
     visibles par tous (inscrits ou non). Items par défaut : **« Présentation du programme »** (en 1er),
     **« Bienvenue dans La Voie »**. (Remplace l'ancienne grille « vidéos d'accueil » de la page d'accueil.)
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
  4. **« Ma promotion »** (section repliable, synchrone) : **sélecteur de promotion** (dérouleur par année, en tête
     de la section — pas de libellé « Promotion » redondant), puis **Présentiels, Participants, WhatsApp**.
     Sélecteur masqué en accès découverte (non-inscrit = sans promotion).
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

## 7. Structure d'un cours

Côté membre, la barre contextuelle gauche regroupe les sections en **deux familles** :

**Programme** (asynchrone, partagé entre promos) :
1. **Sessions** — liste des sessions (contenu asynchrone)
2. **Lexique** — termes clés (niveau cours), avec **index alphabétique** (A–Z) pour filtrer/aller à un terme
3. **Références** — livres, vidéos, articles recommandés (niveau cours), **avec liens cliquables**

**Ma promotion** (synchrone, par promo) :
4. **Présentiels** — dates + lieux + horaires (par promotion), **affichés les uns sous les autres**,
   avec un volet **« Détails pratiques & documents »** par présentiel : accès/transport, hébergement
   (se loger), règlement intérieur, fiche de l'établissement (liens/documents)
5. **Participants** — annuaire des membres de la promotion (photo, nom, ville)
6. **WhatsApp** — accès au groupe WhatsApp de la promotion

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

---

## 11. Visios live & replays (BigBlueButton + Notta)

### Visios
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
  **dans la section « Ma promotion »**, avec un **libellé contextuel** :
  - **« Training »** dans les programmes de **l'École** → le membre **programme** ses séances.
  - **« Coaching »** dans les programmes de **La Voie** → le membre **participe** comme coaché (sur invitation) :
    **lecture seule** — il voit « Mes séances » mais **ni « Programmer une séance » ni « À évaluer »** (réservés à l'École/coach).
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

### Flux Notion (bidirectionnel)

**① Notion → LMS (inscriptions)**
- Sur Notion, une **base de données « participants »** : on inscrit un participant
  à un cours via un **simple bouton**.
- Cela déclenche côté LMS : **création/provisionning du compte**, **rattachement
  au cours + à la promotion**, puis **envoi automatique des infos et des accès de
  connexion** au participant (e-mail d'onboarding).

**② LMS → Notion (CRM + reporting)**
- Mise à jour des fiches membres (CRM)
- Export des données **assiduité / heures / BPF** (cf. §17)

> Implique un **service d'e-mailing transactionnel** pour l'envoi des accès et
> des notifications (bienvenue, identifiants/lien d'activation, rappels visio…).

---

## 14. Back-office d'administration

### Accès au back-office par rôle
| Section BO | Admin | Coach / Animateur |
|---|---|---|
| **Suivi pédagogique** | tout | ✅ ses programmes uniquement |
| **Promotions** (membres promo, planning visios) | tout | ✅ promos de ses programmes |
| **Programmes** (contenu, sessions, visios) | tout | ✅ ses programmes (édition) |
| **Membres & accès** (global, rôles, comptes) | ✅ | ❌ |
| **Intégrations** (BBB, Notta, Notion, Stripe…) | ✅ | ❌ |
| Créer / supprimer un programme | ✅ | ❌ |

- Le coach ne voit dans le BO **que** : Programmes (les siens), Promotions (les siennes),
  Suivi pédagogique (les siens). Les onglets Membres & Intégrations lui sont masqués.
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
- **Intégrations** — config BBB, Notta, YoDalf, WhatsApp, Notion, Stripe

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
  - **Préceptes** (vidéos en accès libre/gratuit, listées dans le menu de gauche du programme) — chaque
    vidéo d'une séquence peut aussi être marquée « publique/Gratuit » et remonte alors automatiquement ici.
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
- Le déverrouillage (ignorer la pédagogie inversée) se fait **par programme**
  (toggle dans l'éditeur du programme), **pas globalement**.

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
- **Intégré dans la plateforme** (page interne avec **iframe Notion**), pas un lien externe.
- Accessible depuis le **menu principal (barre du haut)**, **en première position, avant « Les programmes »**.
- Source : Livret d'Accueil Client ODBI (Notion) — **lien d'intégration officiel** :
  `https://odbi.notion.site/ebd//13d94934418a80cb843dfaebb32328ef` (le `/ebd/` est l'endpoint
  d'embed Notion). Page publiée sur le web ; bouton « Ouvrir dans un onglet » en repli.
- **Limite de la maquette locale** : l'iframe Notion **ne s'affiche pas** quand le fichier est ouvert
  en `file://` (Notion exige un hôte `https`). L'intégration **fonctionnera une fois la plateforme hébergée**
  (Next.js sur un domaine ODBI). En attendant, la maquette affiche une note explicative + le bouton de repli.

### Agenda
- Présenté comme un **vrai calendrier** (vue mensuelle), filtré sur les promos du membre.
- Événements color-codés et **cliquables** (renvoient au contenu concerné) :
  **Visios (BBB), Présentiels, Exercices, Ressources à consulter, Quiz, Séances d'entraînement (Coaching/Training)**.

---

## 19. Points en suspens / à décider

- [ ] Groupe WhatsApp : **par promotion** (hypothèse retenue) ou par programme ?
- [x] Gating exercices : **configurable par session** (toggle « pédagogie inversée » dans l'éditeur)
- [ ] Lexique : uniquement par cours, ou aussi un lexique global plateforme ?
- [x] Sens de la synchro Notion : **bidirectionnel** — Notion→LMS (inscriptions + envoi des accès), LMS→Notion (CRM + BPF)
- [ ] Service d'**e-mailing transactionnel** à choisir (envoi des accès, notifications)
- [ ] **Plan Vimeo** adapté (Pro/Business+) requis pour Player SDK + confidentialité par domaine
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
