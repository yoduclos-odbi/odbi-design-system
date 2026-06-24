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

---

## 7. Structure d'un cours

Page cours = **4 onglets** :
1. **Sessions** — liste des sessions (contenu asynchrone)
2. **Présentiels** — dates + lieux + horaires (par promotion)
3. **Lexique** — termes clés (niveau cours)
4. **Références** — livres, vidéos, articles recommandés (niveau cours)

+ Badge de la promotion du membre + accès au groupe WhatsApp de la promo.

---

## 8. Structure d'une session

Page session = **4 onglets** (ordre = pédagogie inversée) :

1. **📝 Exercices** *(pédagogie inversée — important)*
   - Un ou plusieurs exercices à réaliser AVANT le contenu
   - Le membre **coche « j'ai réalisé l'exercice »** → **débloque** vidéos / audios / PDF
   - **NON obligatoire** : option « Voir quand même » pour ne jamais bloquer durement
   - Fiche d'exercice téléchargeable
2. **🎬 Vidéos** — plusieurs vidéos par session (playlist) — hébergement type Mux/Vimeo
3. **🎧 Ressources** — un ou plusieurs audios + PDF (écoute / téléchargement)
4. **✅ Quiz** — voir §9

---

## 9. Quiz

- **Un quiz par session**
- **Plusieurs formes de questions** : QCM, Vrai/Faux, Association (matching), Réponse libre
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
- Menu « Visios live » = agenda **filtré** par les promos du membre
- Chaque visio porte l'étiquette de son cours + promo

### Replays
Flux : `Visio BBB → enregistrement BBB → Notta (transcription + chapitrage + résumé, multi-langues) → objet Replay rattaché à la promo`

- **Accès réservé aux membres de la promotion** concernée 🔒
- Le membre retrouve ses replays dans « Visios live → Replays », **filtrés par promo**
- Actions par replay : ▶ Revoir (BBB) · 📝 Transcription (Notta) · 📄 Résumé (Notta) · langues dispo
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
| **Notion** | **CRM ODBI** (membres & prospects) — synchro |
| **Stripe** | Paiements & abonnements |

---

## 14. Back-office d'administration

Onglets :
- **Membres & accès** — comptes, rôles, accès aux cours (éditable), activation
- **Cours** — création/édition des cours, sessions, contenus, WhatsApp par cours
- **Promotions** — cohortes par programme, membres, dates, classes, présentiels
- **Intégrations** — config BBB, Notta, YoDalf, WhatsApp, Notion, Stripe

---

## 15. Stack technique envisagée

- **Front + back** : Next.js (React)
- **Base de données + Auth + Stockage** : Supabase (PostgreSQL)
- **Vidéo** : Mux ou Vimeo (hébergement/streaming)
- **Visio** : BigBlueButton (API)
- **IA** : API Claude (Anthropic)
- **Paiement** : Stripe
- À confirmer au démarrage du projet réel.

---

## 16. Points en suspens / à décider

- [ ] Groupe WhatsApp : **par promotion** (hypothèse retenue) ou par programme ?
- [ ] Niveau de gating exercices : configurable par session côté admin ?
- [ ] Lexique : uniquement par cours, ou aussi un lexique global plateforme ?
- [ ] Sens de la synchro Notion (CRM) : inscriptions → fiches ? suivi prospects ?
- [ ] Modèle d'abonnement / tarification (Stripe) : à définir
- [ ] Langues de l'interface (FR seul, ou multilingue ?)

---

## Annexes — livrables de conception

- `maquette-odbi-lms.html` — maquette interactive (écrans)
- `architecture-odbi.html` — schéma d'organisation (plan d'ensemble)
- `odbi-design-tokens.css` / `.json` — design system
