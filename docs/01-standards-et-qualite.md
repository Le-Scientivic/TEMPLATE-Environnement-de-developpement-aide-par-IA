# Standards et qualité logicielle — Synthèse des grandes références

> **En résumé** : il existe plusieurs corpus de standards reconnus pour la qualité logicielle, la documentation, la sécurité et l'architecture. Cette note en fait une synthèse actionnable, orientée pratique, pour un développeur humain travaillant avec l'IA.

---

## Pourquoi s'appuyer sur des standards ?

Les standards ne sont pas des règles bureaucratiques. Ce sont des condensés d'expérience collective issus de décennies de pratiques sur des projets réels — parfois critiques (NASA, Linux kernel). Les ignorer, c'est réinventer la roue à chaque projet. Les appliquer intelligemment, c'est gagner du temps, réduire les erreurs, et produire un travail transmissible.

Utilisés avec l'IA, ils servent aussi de **langage commun** : tu peux dire à Copilot "respecte Twelve-Factor" ou "structure la doc avec Diátaxis" et il comprend.

---

## 1. Qualité du code — Style et maintenabilité

### Linux Kernel Coding Style

**Source officielle** : <https://www.kernel.org/doc/html/latest/process/coding-style.html>

Le guide de style du noyau Linux est l'un des plus lus au monde. Il est parfois taillé pour le C, mais ses principes sont universels :

- **La lisibilité passe avant tout.** Un code lisible est maintenable, auditable, transmissible.
- Indentation cohérente (les tabs dans Linux, des espaces dans beaucoup d'autres : l'essentiel est la cohérence dans le projet).
- Fonctions courtes, responsabilité unique.
- Nommage explicite : éviter les abréviations cryptiques.
- Commentaires qui expliquent **pourquoi**, pas **quoi** (le code dit déjà quoi).
- Éviter la "magie" : une expression complexe en une ligne est souvent un bug futur.

**À retenir pour ton projet** : adopte une feuille de style claire dès le départ, note-la dans `CONTRIBUTING.md` et dans les instructions Copilot.

---

### Google Engineering Practices

**Source officielle** : <https://google.github.io/eng-practices/>

Google publie ses guides internes sur la revue de code et les standards de contribution. Points clés :

- Les PR doivent être petites et ciblées.
- Le relecteur cherche avant tout la lisibilité, la maintenabilité et la correction — pas la perfection théorique.
- Le code doit améliorer la santé globale de la codebase, même si ce n'est pas parfait.

---

## 2. Sécurité — OpenSSF et l'hygiène de projet

### OpenSSF OSPS Baseline

**Source officielle** : <https://openssf.org/projects/osps-baseline/>  
**Scorecard** : <https://securityscorecards.dev/>

L'Open Source Security Foundation (OpenSSF) propose un ensemble de bonnes pratiques de sécurité pour les projets open source. L'**OSPS Baseline** est une liste minimale mais solide :

- Contrôle d'accès au dépôt (qui peut merger, pousser sur main ?)
- Politique de divulgation responsable (`SECURITY.md`)
- Automatisation de la détection de vulnérabilités dans les dépendances
- Signatures et provenance des releases
- Protections de branches (require reviews, require CI passing)
- Pas de secrets dans le code source

**Le Scorecard OpenSSF** est un outil automatique qui évalue la posture sécurité d'un repo GitHub. Tu peux l'intégrer en CI.

**À retenir** : crée un fichier `SECURITY.md` dès le départ avec les instructions pour signaler une vulnérabilité. Active les alertes Dependabot sur GitHub.

---

### Dependabot et GitHub Security Features

**Source officielle** : <https://docs.github.com/en/code-security>

GitHub propose nativement :

- **Dependabot Alerts** : alerte quand une dépendance a une CVE connue.
- **Dependabot Security Updates** : ouvre automatiquement une PR pour corriger.
- **Secret Scanning** : détecte si un secret (clé API, token) est accidentellement commité.
- **Code Scanning (CodeQL)** : analyse statique du code pour détecter des failles.

Tout cela s'active dans les paramètres du repo (Settings > Security).

---

## 3. Architecture — Twelve-Factor App

**Source officielle** : <https://12factor.net/>

La méthode **Twelve-Factor** est une référence pour construire des applications déployables comme services. Elle définit 12 principes pour une architecture robuste et portable :

| Facteur | Principe |
|---|---|
| I. Codebase | Un seul dépôt, déploiements multiples |
| II. Dependencies | Déclarer et isoler explicitement les dépendances |
| III. Config | La config dans l'environnement (pas dans le code) |
| IV. Backing services | Traiter les services comme des ressources attachées |
| V. Build, release, run | Séparer strictement les phases |
| VI. Processes | Exécuter l'app comme des processus stateless |
| VII. Port binding | Exposer les services via un port |
| VIII. Concurrency | Scaler par ajout de processus |
| IX. Disposability | Démarrage rapide, arrêt propre |
| X. Dev/prod parity | Garder dev, staging, prod aussi similaires que possible |
| XI. Logs | Traiter les logs comme des flux d'événements |
| XII. Admin processes | Lancer les tâches admin comme des processus ponctuels |

**À retenir** : même si tu ne fais pas une app "twelve-factor" complète, les facteurs III (config), V (séparation des phases) et X (parité dev/prod) sont des réflexes universels précieux.

---

## 4. Documentation — Framework Diátaxis

**Source officielle** : <https://diataxis.fr/>

Diátaxis est un framework de structuration de la documentation technique, créé par Daniele Procida. Il distingue 4 types de contenu documentaire selon deux axes :

- **axe d'apprentissage** (étude ↔ travail)
- **axe de besoin** (pratique ↔ théorique)

| Type | But | Exemple |
|---|---|---|
| **Tutoriels** | Apprendre en faisant | "Mon premier projet avec ce template" |
| **How-to guides** | Accomplir une tâche précise | "Comment ajouter un workflow CI" |
| **Référence** | Consulter des faits précis | "Options de configuration disponibles" |
| **Explication** | Comprendre les choix | "Pourquoi on utilise Conventional Commits" |

**À retenir** : ne mélange pas ces types ! Un tutoriel qui essaie aussi d'expliquer l'architecture est un mauvais tutoriel. Organise `docs/` en respectant cette structure.

Structure recommandée :

```
docs/
  tutorials/       # Apprendre pas à pas
  how-to/          # Résoudre une tâche précise
  reference/       # Faits, API, options, commandes
  explanation/     # Comprendre les choix et concepts
  adr/             # Architecture Decision Records
```

---

## 5. Assurance logicielle — Standards NASA

**Source officielle** : <https://www.nasa.gov/intelligent-systems-division/software-management-office/>

La NASA publie des standards pour l'ingénierie logicielle sur des systèmes critiques. Les deux documents les plus pertinents :

- **NPR 7150.2D** — NASA Software Engineering Requirements : cycle de vie, traçabilité, vérification, revues.
- **NASA-STD-8739.8** — Software Assurance and Safety Standard : assurance qualité, gestion des risques.

Tu n'as probablement pas besoin de tout ça pour un projet web. Mais les principes fondamentaux sont universels :

- Chaque exigence doit être **traçable** (de l'issue au code au test).
- Toute décision d'architecture importante doit être **documentée** (c'est l'ADR).
- Les tests ne sont pas optionnels, ils sont des **preuves de fonctionnement**.
- La documentation fait partie du livrable, pas d'un bonus optionnel.

---

## 6. Changelog et versioning sémantique

### Keep a Changelog

**Source officielle** : <https://keepachangelog.com/>

Un changelog lisible par les humains n'est pas un dump de commits. Il doit :

- Être chronologique (plus récent en premier).
- Distinguer les types de changements : `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.
- Lier chaque section à une version.

### Semantic Versioning (SemVer)

**Source officielle** : <https://semver.org/>

`MAJEUR.MINEUR.PATCH`

- **PATCH** : correction de bug rétrocompatible
- **MINEUR** : nouvelle fonctionnalité rétrocompatible
- **MAJEUR** : changement incompatible avec les versions précédentes

---

## 7. Conventions de commits

### Conventional Commits

**Source officielle** : <https://www.conventionalcommits.org/>

Format : `type(scope): description`

Exemples :
```
feat(auth): add password reset flow
fix(api): handle null response from provider
docs(readme): clarify local setup steps
refactor(core): simplify configuration loading
test(auth): add unit tests for login validation
ci(github-actions): add lint step to workflow
```

Types courants :
- `feat` : nouvelle fonctionnalité
- `fix` : correction de bug
- `docs` : documentation uniquement
- `style` : formatage, pas de changement logique
- `refactor` : restructuration sans changement de comportement
- `test` : ajout ou correction de tests
- `ci` : modifications des pipelines CI/CD
- `chore` : maintenance, mise à jour de dépendances

**À retenir** : Conventional Commits facilite la génération automatique de changelogs et la lecture de l'historique Git.

---

## 8. Write the Docs — Philosophie de documentation

**Source officielle** : <https://www.writethedocs.org/guide/>

La communauté Write the Docs rassemble des rédacteurs techniques et des développeurs autour d'une idée simple : **la documentation est une compétence, pas un accessoire**.

Quelques principes clés :

- La doc doit être pensée **dès le début**, pas après.
- La doc doit être **testée** : liens fonctionnels, exemples qui marchent.
- La doc est vivante : une doc obsolète est pire qu'une absence de doc.
- Le README est souvent le premier (et parfois seul) document qu'on lit : il doit être excellent.

---

## Résumé rapide — Ce que tu peux faire dès maintenant

| Action | Standard associé |
|---|---|
| Créer `SECURITY.md` | OpenSSF OSPS Baseline |
| Activer Dependabot | OpenSSF / GitHub Security |
| Structurer `docs/` en 4 types | Diátaxis |
| Adopter Conventional Commits | Conventional Commits |
| Créer un `CHANGELOG.md` | Keep a Changelog |
| Protéger la branche `main` | OpenSSF OSPS Baseline |
| Créer des ADR dans `docs/adr/` | NASA NPR 7150.2D (traçabilité) |
| Garder la config hors du code | Twelve-Factor (facteur III) |
| Documenter le style de code | Linux Kernel Style / Google |

---

*Sources : [kernel.org](https://www.kernel.org/doc/html/latest/process/coding-style.html) · [openssf.org](https://openssf.org/projects/osps-baseline/) · [12factor.net](https://12factor.net/) · [diataxis.fr](https://diataxis.fr/) · [nasa.gov](https://www.nasa.gov/intelligent-systems-division/software-management-office/) · [keepachangelog.com](https://keepachangelog.com/) · [semver.org](https://semver.org/) · [conventionalcommits.org](https://www.conventionalcommits.org/) · [writethedocs.org](https://www.writethedocs.org/guide/)*
