# 📚 ESPACE ENSEIGNANT — En standby

> **Status** : Code initial créé puis mis en standby (15 mai 2026)
> **Raison** : Demande une refonte complète de l'UX/UI auth + navigation profil
> **Fichier en attente** : `_STANDBY_flowin_enseignant.html` (renommé pour ne pas être servi par Vercel)

---

## ❓ Pourquoi en standby

La V0 livrée (commit `b188ce4`) fonctionne techniquement mais reste un patch :
- Login par mot de passe partagé par école = pas un vrai compte
- Pas de gestion individuelle des profs (renommage, plusieurs profs par école…)
- Pas de navigation entre vues (classes / matières / élève détail)
- Pas d'historique des actions enseignant
- Pas d'UX d'onboarding enseignant
- Pas de récupération mot de passe

C'est bricolé pour démarrer mais ça ne tient pas à 50+ enseignants.

---

## 🎯 Quand on reprendra — Spécifications à concevoir

### A. Authentification & Comptes

**Modèle de données**
- Nouvelle table `enseignants` : `id (uuid)`, `email`, `prenom`, `nom`, `ecole`, `classes (array)`, `matiere (array)`, `role ('teacher'|'admin'|'principal')`, `actif`, `created_at`
- 1 prof peut suivre plusieurs classes, plusieurs matières
- 1 école peut avoir plusieurs profs

**Inscription enseignant**
- [ ] Comment un prof s'inscrit ? (Magic Link comme élève ? Invitation admin obligatoire ?)
- [ ] Validation admin avant activation ? (anti-spam)
- [ ] Vérification d'établissement (mail @lycee.fr ?)

**Connexion**
- [ ] Magic Link uniquement (cohérent avec élève)
- [ ] Ou Magic Link + Google ? (Google Edu est souvent utilisé)
- [ ] RGPD enseignant : consentement données élèves

### B. UX/UI Profil Enseignant

**Profil enseignant**
- [ ] Photo / Avatar (initiales si pas de photo)
- [ ] Prénom + Nom (ex: "M. Dubois")
- [ ] Email pro
- [ ] Établissement (sélection autocomplete)
- [ ] Matières enseignées (multi-select : Histoire, Géo, EMC, PSE, autre)
- [ ] Classes suivies (text libre : "Terminale Bac Pro 1", "CAP 2 Coiffure"…)
- [ ] Préférences notifications (alertes décrocheurs, hebdo, etc.)

**Édition profil**
- [ ] Modifier infos
- [ ] Changer email
- [ ] Désactiver compte
- [ ] Demander suppression RGPD

### C. Navigation & Vues

**Architecture proposée**
```
/enseignant
├── /tableau-de-bord       (vue actuelle — KPIs + perf matière)
├── /mes-classes           (liste classes + onglet par classe)
│   └── /classe/[id]       (élèves de cette classe)
├── /eleve/[pseudo]        (fiche détaillée d'UN élève)
├── /matieres              (vue 4 matières avec drill-down)
│   └── /matiere/[code]    (questions ratées, thèmes faibles)
├── /actions               (envois en masse : encouragement, défis groupe)
├── /profil                (mes infos, préférences, déconnexion)
└── /aide                  (FAQ, contact support)
```

**Composants à créer**
- [ ] Sidebar de navigation (style admin actuel mais simplifié)
- [ ] Header avec photo + nom prof
- [ ] Breadcrumb pour navigation profonde
- [ ] Filtres par classe (sélecteur global persistant)
- [ ] Filtres par période (7j, 30j, trimestre, année)

### D. Features pédagogiques avancées (à prioriser)

**Fiche élève détaillée** (vue critique)
- [ ] Profil + stats globales
- [ ] Heatmap d'activité (GitHub-like 90 jours)
- [ ] Historique des quiz par matière
- [ ] Points forts / Points faibles (par thème)
- [ ] Évolution dans le temps (graphique points)
- [ ] Bouton "Encourager" (envoie notif personnalisée)
- [ ] Bouton "Lancer un défi à cet élève"
- [ ] Notes privées prof (pas vues par l'élève)

**Vue par matière (drill-down)**
- [ ] Score moyen de la classe par thème (Seconde Guerre Mondiale, Mondialisation, Laïcité, etc.)
- [ ] Questions les plus ratées (focus enseignement)
- [ ] Top 5 / Bottom 5 élèves dans cette matière
- [ ] Export PDF rapport classe

**Communication**
- [ ] Envoyer un message in-app à un élève
- [ ] Envoyer un défi groupe à toute une classe
- [ ] Notifier les décrocheurs en 1 clic
- [ ] Annoncer un événement (révisions, contrôle, etc.)

**Insights pédagogiques**
- [ ] Comparaison classe vs moyenne établissement
- [ ] Comparaison établissement vs moyenne nationale Flowin
- [ ] Détection précoce de difficulté (ML : élève qui décroche)

### E. Permissions & Sécurité

- [ ] RLS Supabase : prof ne voit que SES classes
- [ ] Logs d'audit : qui a vu quoi, quand
- [ ] Limite quotidienne d'envois (anti-spam élèves)
- [ ] Modération par admin (admin peut révoquer prof)

### F. Onboarding

- [ ] Tutoriel guidé au premier login (5 étapes max)
- [ ] Email de bienvenue
- [ ] Tour rapide des features
- [ ] Aide contextuelle (?  popups)

---

## 🚀 Plan d'action (quand on reprendra)

**Phase 1 — Design & UX (2-3 jours)**
- [ ] Wireframes des 6 vues principales (sidebar + dashboard + classe + élève + matière + profil)
- [ ] Maquettes haute fidélité (au moins dashboard, classe et fiche élève)
- [ ] Spec des composants réutilisables
- [ ] Validation avec 1-2 profs testeurs (Olivia ?)

**Phase 2 — Schéma de données**
- [ ] SQL création table `enseignants`
- [ ] SQL liens classes / matières / élèves
- [ ] Modification table `joueurs` si nécessaire (FK enseignant_id ?)
- [ ] Policies RLS adaptées

**Phase 3 — Auth & Onboarding (1 semaine)**
- [ ] Page de login Magic Link
- [ ] Page d'inscription avec validation admin
- [ ] Email de bienvenue
- [ ] Tutoriel guidé

**Phase 4 — Vues principales (2 semaines)**
- [ ] Dashboard (reprendre la V0 + l'enrichir)
- [ ] Mes classes
- [ ] Fiche élève détaillée
- [ ] Vue matière drill-down

**Phase 5 — Actions & Communication (1 semaine)**
- [ ] Envoi messages
- [ ] Défis groupe
- [ ] Notifications décrocheurs

**Phase 6 — Migration Next.js**
- [ ] Idéalement on fait l'espace enseignant directement en Next.js
- [ ] Ça évite de redévelopper en vanilla puis migrer

**Total estimé : 4-5 semaines**

---

## 💾 Code existant à reprendre

`_STANDBY_flowin_enseignant.html` contient :
- Design system Flowin (variables CSS, typo, palette)
- Login simple par password école (à remplacer)
- Calculs KPIs (à conserver)
- Render performance matières (à conserver)
- Render top performers / décrocheurs (à conserver)
- Table élèves avec recherche (à conserver)

**Ce qu'on garde** : 70% du code (logique métier, rendering)
**Ce qu'on refait** : 30% (auth, navigation, profil, structure)

---

## ⏸️ Décisions à figer avant de reprendre

1. **Stack** : vanilla ou direct en Next.js ?
2. **Tarification** : espace prof gratuit ou freemium ?
3. **Onboarding prof** : ouvert ou sur invitation admin ?
4. **Périmètre V1** : juste suivi (read-only) ou avec actions (messages, défis) ?
5. **Permissions** : tout prof voit toute son école ou strictement ses classes ?

---

*Doc créé le 15 mai 2026 pour reprise future.*
*Mots de passe enseignant SQL **non lancés** (FLOWIN_PASSWORDS_ENSEIGNANT.sql reste en attente).*
