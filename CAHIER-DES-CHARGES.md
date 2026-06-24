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

### Couleurs par univers
- **La Voie** → dominante **bleu + vert** (`indigo-dye` + `asparagus`)
- **L'École** → dominante **noir + vert** (`black` + `asparagus`)

---

## 4. Acteurs & rôles

| Rôle | Description |
|------|-------------|
| **Membre** | Suit les cours, participe aux visios/présentiels de sa promo |
| **Coach / Animateur** | Anime les visios, suit une ou plusieurs promotions |
| **Admin** | Gère contenu, membres, accès, promotions, intégrations |

Accès **éditable** : un admin peut définir qui a accès à quel programme /
promotion (accès par membre).

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
- Menu : **« Les cours »** (catalogue complet) placé **au-dessus du Tableau de bord**
  (le tableau de bord garde « reprendre mes cours » → on évite le doublon « Mes cours »).
- **Tous les cours sont visibles** par tout participant connecté (pas de verrou sur les cartes).
- **Freemium** : sur un cours où le participant **n'est pas inscrit**, une partie du
  contenu est **gratuite** (pour donner envie) ; le **reste est réservé aux inscrits**.
- Sur un cours où il **est inscrit** : accès complet + progression.
- Dans le catalogue : cours inscrits = barre de progression ; cours non inscrits =
  mention « 🎁 Contenu gratuit · reste réservé aux inscrits ».

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

## 7. Structure d'un cours

Page cours = **6 onglets** :
1. **Sessions** — liste des sessions (contenu asynchrone)
2. **Présentiels** — dates + lieux + horaires (par promotion), **affichés les uns sous les autres**,
   avec un volet **« Détails pratiques & documents »** par présentiel : accès/transport, hébergement
   (se loger), règlement intérieur, fiche de l'établissement (liens/documents)
3. **Lexique** — termes clés (niveau cours), avec **index alphabétique** (A–Z) pour filtrer/aller à un terme
4. **Références** — livres, vidéos, articles recommandés (niveau cours), **avec liens cliquables**
5. **Participants** — annuaire des membres de la promotion (photo, nom, ville)
6. **WhatsApp** — accès au groupe WhatsApp de la promotion

+ Badge / sélecteur de la promotion du membre (dérouleur par année).

---

## 8. Structure d'une session

Une session **n'est pas un bloc unique** : elle est organisée en **séquences**,
et peut **s'étaler sur plusieurs semaines** (adaptation pédagogique).

### Une session = une suite de séquences
- Une session est une suite de **séquences** (≈ une semaine / un thème chacune).
- Une séquence peut contenir **plusieurs vidéos** et **plusieurs ressources**.
- Chaque séquence est une **suite de blocs séparés et color-codés** (cartes distinctes) :
  1. 📝 **Exercice(s)** préparatoire(s)
  2. 🎬 **Vidéo(s)** (Vimeo — playlist possible)
  3. 🎧 **Ressource(s)** synchronisées (audios / PDF, plusieurs possibles)
  4. ✅ **Quiz** de la séquence
  5. 🎥 **Replay** de la visio de la semaine (BBB + transcription/chapitrage/résumé Notta, par promo)
- Affichage : **accordéon** de séquences, avec libellé de semaine et déverrouillage progressif.
- Le **replay est rangé à la fin du bloc de la semaine** (pas dans une liste séparée).

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
- Actions par replay : ▶ Revoir (BBB) · 📝 Transcription (Notta) · 📄 Résumé (Notta) · 🧙 Questions à YoDalf · langues dispo
- **Résumé = fait par Notta** (transcription + chapitrage + résumé proposé)

---

## 12. Assistant IA — « YoDalf »

- Mentor IA basé sur **Claude (API Anthropic)**
- Formé sur le contenu des cours (résumés, révisions, Q&R)
- Accessible depuis le menu + depuis une session (« Demander à YoDalf »)
- Illustration dédiée : `assets/yodalf.png` (fournie par ODBI ; repli emoji 🧙)

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

Onglets :
- **Membres & accès** — comptes, rôles, accès aux cours (éditable), activation
- **Cours** — création/édition des cours, sessions, contenus, WhatsApp par cours
- **Promotions** — cohortes par programme, membres, dates, classes, présentiels
- **Suivi pédagogique (animateur)** — accès aux **restitutions des participants** :
  exercices rendus (fichiers téléchargeables), **résultats de quiz** (score, tentatives),
  vidéos vues, statut. Filtrable par cours / promo / séquence. Export CSV + Notion (BPF).
- **Intégrations** — config BBB, Notta, YoDalf, WhatsApp, Notion, Stripe

### Éditeur de cours (back-office)
Doit être **ergonomique** (rechargement manuel des contenus depuis Moodle).

**Éditeur de cours** :
- Paramètres : nom, univers, animateur, statut (publié/brouillon), durée, description, image de couverture
- Onglets d'édition : **Sessions** (ajout / réordonnancement par glisser-déposer / duplication / suppression), **Lexique**, **Références**, **Présentiels** (par promotion)

**Éditeur de session** :
- Titre, règle d'accès (verrouillage), **toggle « pédagogie inversée »** (gating exercices, activable par session — non bloquant)
- Onglets d'édition : **Exercices** (titre, consigne, fiche PDF), **Vidéos** (ID/URL Vimeo, titre, durée), **Ressources** (upload audios/PDF par glisser-déposer), **Quiz** (constructeur : type par question — QCM / Vrai-Faux / Association / Réponse libre — options et bonne réponse)

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
- Lien dans le **menu de gauche** vers le **Livret d'Accueil Client ODBI** (page Notion) :
  https://www.notion.so/odbi/Livret-d-Accueil-Client-ODBI-13d94934418a80cb843dfaebb32328ef

### Agenda
- Présenté comme un **vrai calendrier** (vue mensuelle), filtré sur les promos du membre.
- Événements color-codés et **cliquables** (renvoient au contenu concerné) :
  **Visios (BBB), Présentiels, Exercices, Ressources à consulter, Quiz**.

---

## 19. Points en suspens / à décider

- [ ] Groupe WhatsApp : **par promotion** (hypothèse retenue) ou par programme ?
- [x] Gating exercices : **configurable par session** (toggle « pédagogie inversée » dans l'éditeur)
- [ ] Lexique : uniquement par cours, ou aussi un lexique global plateforme ?
- [x] Sens de la synchro Notion : **bidirectionnel** — Notion→LMS (inscriptions + envoi des accès), LMS→Notion (CRM + BPF)
- [ ] Service d'**e-mailing transactionnel** à choisir (envoi des accès, notifications)
- [ ] **Plan Vimeo** adapté (Pro/Business+) requis pour Player SDK + confidentialité par domaine
- [ ] Contenu exact de l'**e-mail d'onboarding** (identifiants vs lien magique d'activation ?)
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
- `odbi-design-tokens.css` / `.json` — design system
