# Odbi campus — Feuille de route & chiffrage

> Document compagnon du **Cahier des charges** (`CAHIER-DES-CHARGES.md`) et de la **maquette**
> (`maquette-odbi-lms.html`). Il traduit la spec en **plan, estimation (en jours-homme) et décisions**.
> Chiffrages en **jours-homme du dev** (≈ jours calendaires ouvrés si le dev est à temps plein, ~21 j/mois).
> Modèle retenu : **Claude (moteur de code) + 1 dev senior (intégrations, ops, contrôle/test)**.

---

## 1. Synthèse décision (TL;DR)

- **Ne pas partir d'un SaaS fermé (Skool, etc.)** : il donne les ~60 % génériques mais **bloque** sur les
  ~40 % qui font le projet (certification RNCP, Qualiopi/BPF, BBB existant, Notion). Jardin clos = impasse.
- **Moodle est déjà l'existant** (BBB y est branché aujourd'hui). Motif du départ = **le design / l'UX**,
  **pas** un manque de fonctions. → Le moteur Moodle fait déjà les tranches lourdes et risquées.
- **Scénario recommandé : B (« Moodle headless »)** — on garde **le moteur Moodle** (cours, quiz, progression,
  certif, BBB, Qualiopi) et on développe **uniquement le front de la maquette** par-dessus, branché à l'API.
- **Trancher B vs C via un spike de 2–4 jours** (voir §6) avant tout engagement lourd.

---

## 2. Les 3 scénarios (jours & mois)

| Scénario | MVP | Produit complet |
|---|---|---|
| **B2** — Moodle moteur **+ admin Moodle** (pas de BO custom) ⭐ | ~18–28 j (~1 mois) | **~60–90 j (~3–4,5 mois)** |
| **B1** — Moodle moteur **+ BO custom** (admin joli aussi) | ~22–32 j (~1,5 mois) | ~85–115 j (~4–5,5 mois) |
| **C** — Full custom (front + backend maison) | ~26–35 j (~1,5 mois) | ~110–160 j (~5,5–7,5 mois) |

*(mois = temps plein ; marge d'imprévus ~15 % incluse dans le « complet ». Dev à mi-temps → double le calendrier, pas les jours-homme.)*

**Économie B2 vs B1** (ne pas développer le back-office, utiliser l'admin Moodle) : **~20–30 jours-homme**
(~1 à 1,5 mois), + pas de BO à maintenir dans le temps. Compromis : l'admin travaille dans l'interface
Moodle (fonctionnelle mais austère) ; les **membres ne voient que la maquette**.

---

## 3. MVP vs Produit complet

**MVP** = la **boucle d'apprentissage** de bout en bout pour **une promo pilote** : s'inscrire, suivre le
contenu, faire les quiz, rejoindre les visios, être suivi. Lancer petit et sûr, récolter du feedback réel.

| Domaine | MVP | Complet |
|---|---|---|
| Connexion, rôles, permissions | ✅ Participant/Animateur/Admin | ➕ vue Jury, périmètres fins |
| Catalogue & programmes | ✅ | ✅ |
| Contenu async (sessions, quiz, progression, pédagogie inversée) | ✅ | ➕ exercices en groupe, vidéo d'exercice |
| Promotions / cohortes | ✅ basique | ➕ déverrouillage fin, Alumni |
| Inscriptions Notion | ✅ lecture | ➕ **synchro bidirectionnelle** (CRM, BPF) |
| Visios BBB (live) | ✅ rejoindre la salle | ✅ |
| Replays / « Classes » | ⛔ (ou lien brut) | ✅ enrichi (Notta + YoDalf) |
| Back-office | ✅ minimal (ou Moodle) | ➕ suivi décrocheurs, exports |
| Gamification | ⛔ | ✅ |
| Notifications | ✅ minimal | ✅ moteur complet |
| Séances Coaching/Training + éval coach | ⛔ | ✅ |
| **Certification** (jury, PV, RNCP) | ⛔ | ✅ |
| Qualiopi / BPF | ⛔ (données collectées) | ✅ |
| YoDalf (IA) | ⛔ | ✅ |
| WhatsApp / Loom | ✅ lien WhatsApp | ➕ Loom, automatisations |

Le MVP est un **LMS d'apprentissage** utilisable ; le complet ajoute **① certification légale**,
**② conformité/automatisation** et **③ engagement (gamification, IA, replays enrichis, éval coach)**.

---

## 4. Détail par tranche (jours-homme, base full custom C)

**MVP (~26–35 j)**

| Tranche | Jours |
|---|---|
| Socle (auth, rôles, permissions, DB, design system, nav) | 8–10 |
| Catalogue & programmes | 2–3 |
| Contenu async (sessions, séquences, étapes, quiz, progression) | 6–8 |
| Promotions / cohortes (basique) | 3–4 |
| Inscriptions Notion (lecture) + création manuelle | 2–3 |
| Visios BBB en live (redirection, API) | 2–3 |
| Back-office basique + dashboard/profil + onboarding minimal | 3–4 |

**Post-MVP (~70–105 j)**

| Tranche | Jours |
|---|---|
| Replays enrichis (Notta + YoDalf) | 6–9 |
| Gamification | 4–6 |
| Notifications (moteur complet) | 5–7 |
| Séances Coaching/Training + éval coach (résultats Notion + compte rendu) | 8–12 |
| **Certification** (jury, grilles, PV, RNCP/RS, niveaux) | 12–18 |
| **Synchro Notion bidirectionnelle** (CRM écriture + BPF) | 10–15 |
| Qualiopi / BPF (traçabilité, exports) | 6–9 |
| YoDalf (assistant IA) | 6–9 |
| WhatsApp / Loom + automatisations | 3–5 |
| Back-office avancé (vues, décrocheurs, notif center, exports) | 6–9 |
| Annuaire, agenda iCal, profil complet, exercices en groupe | 4–6 |

En **scénario B**, le moteur Moodle **retire ou allège** : Socle (partiel), Contenu async, Certification,
Qualiopi/BPF, Back-office → d'où les ~60–90 j du complet en B2.

---

## 5. Modèle d'organisation

| Claude (moteur de code) | Le dev (intégrations · ops · contrôle) |
|---|---|
| Le gros du **code du front** (maquette → appli React/Next) | **Fait tourner** l'appli : héberge, déploie, environnements |
| La **couche d'appels API** (code qui parle à Moodle) | **Teste/valide** ce code contre le vrai Moodle / Notion |
| Tests, refactor, doc, cohérence | Construit la **glue Moodle** (plugins PHP, Make/n8n, Notion) |
| Itère vite sur les écrans | **Corrige le réel**, détient les accès, possède le repo |

**Points clés** : front et branchement **ne se séparent pas** proprement (le dev est présent tout du long,
pas en bout de chaîne). Mon code ne « vit » que lorsqu'il tourne et est branché au réel — d'où le dev.

**Compétences du dev** : Moodle / PHP (plugins, Web Services), un front JS (React/Next), Make/n8n, ops
(hébergement, CI, monitoring). Séniorité = variable clé (senior → fourchette basse).

---

## 6. Le spike Moodle (2–4 jours) — à faire AVANT de s'engager

Objectif : trancher **B (headless Moodle)** vs **C (full custom)** pour ~3 jours au lieu de le découvrir
après 3 mois. Le dev branche **une page du front** sur Moodle et valide :

1. **Lecture / pilotage via l'API** : cours, complétion, users, cohortes, lancement BBB, certificats.
2. **Crochets d'intégration** : capter un **événement Moodle** et le pousser vers **Notion via Make/n8n**
   (et l'inverse).

- API **couvre assez** → **B** (économie ~40–50 %, risques certif/Qualiopi effacés).
- API **bloque** sur des flux critiques → **C** justifié, en connaissance de cause.

À construire **quoi qu'il arrive** (Moodle ne les fait pas) : synchro Notion, éval coach (Notion), YoDalf.

---

## 7. Risques

- 🔴 **Restants** : synchro Notion bidirectionnelle · certification/PV (**validité juridique**) ·
  sécurité des permissions · conformité Qualiopi.
- 🟡 : intégration visios BBB · éval coach Notion · WhatsApp/Loom · infra app.
- 🟢 : tout le cœur produit (front, catalogue, contenu, quiz, dashboard).

**Mitigations** : le dev qui **teste tout** neutralise la dérive silencieuse ; **Make/n8n** peut absorber la
synchro Notion (sans PHP) ; le **spike** dé-risque le choix d'archi ; la **maquette validée** supprime les
allers-retours UX. **Le légal/Qualiopi reste hors du duo** (expert externe, ponctuel).

---

## 8. Décisions actées

- ✅ **Paiement : hors périmètre LMS** — encaissement via le process existant (inscriptions Notion / manuel).
  **Pas de Stripe.**
- ✅ **BBB déjà auto-hébergé (OVH)** avec **enregistrements sur le serveur** → intégration par API,
  **aucun stockage vidéo de visio à prévoir**, pas de coût serveur neuf.
- ✅ **Départ de Moodle motivé par le design/UX** (pas un manque de fonctions) → moteur Moodle réutilisable.
- ✅ **Marque : Odbi campus** (≠ ODBI Academy).

## 9. Hypothèses & exclusions

- Jours-homme = **1 dev senior à temps plein** avec Claude ; **mi-temps → calendrier ×2**.
- **Exclus des jours** : validation Qualiopi/juridique (expert externe) ; recette côté ODBI ; coûts runtime
  (Notta à la minute, API Claude/YoDalf, hébergement app+DB).
- Chiffrages = **ordres de grandeur** avec marges ; le spike (§6) les précisera.
