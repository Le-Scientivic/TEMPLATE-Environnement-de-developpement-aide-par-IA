# Instructions pour les IA — Environnement de développement augmenté par l'IA

Ce fichier fournit des instructions claires aux assistants IA (GitHub Copilot, Copilot Coding Agent, etc.)
qui travaillent sur ce repository.

---

## 🤖 CI/CD — Ce que tu dois savoir

Ce repository dispose d'une **pipeline CI/CD GitHub Actions** qui s'exécute automatiquement sur chaque
`push` vers `main` et sur chaque `pull_request` vers `main`.

### Fichier de workflow principal

`.github/workflows/ci-linux.yml` — s'exécute sur `ubuntu-latest`

### Checks automatiques effectués

| Check | Outil | Condition d'échec |
|---|---|---|
| 📝 Lint Markdown | `markdownlint-cli` | Erreur de syntaxe ou style Markdown |
| 🔍 Syntaxe YAML | `js-yaml` | Fichier YAML malformé |
| 🔍 Syntaxe JSON | `node` natif | Fichier JSON malformé |
| 🔗 Liens Markdown | `markdown-link-check` | Lien cassé dans la documentation |

---

## 📋 Règles à respecter impérativement

### 1. Tests et qualité

- **Écrire des tests pour tout nouveau code** ajouté au projet.
- **Toujours vérifier que la CI passe** avant de proposer un merge ou de soumettre une PR.
- Si tu ajoutes un fichier de code (Python, JS, shell, etc.), ajoute également les vérifications
  correspondantes dans `.github/workflows/ci-linux.yml`.

### 2. Linting Markdown

- Tous les fichiers `.md` doivent respecter les règles définies dans `.markdownlint.json`.
- Règles désactivées (voir `.markdownlint.json`) :
  - `MD013` : longueur de ligne (pas de limite imposée)
  - `MD031` : lignes vides autour des blocs de code (assouplissement pour les docs existantes)
  - `MD032` : lignes vides autour des listes (assouplissement pour les docs existantes)
  - `MD033` : HTML inline autorisé
  - `MD040` : langage sur les blocs de code (assouplissement pour les docs existantes)
  - `MD041` : premier heading non obligatoire en H1
  - `MD060` : style des colonnes de tableau (assouplissement pour les docs existantes)
- **Ne jamais désactiver une règle de lint sans justification** dans un commentaire.

### 3. Fichiers YAML

- Tous les fichiers `.yml` et `.yaml` doivent être syntaxiquement valides.
- Utiliser l'indentation à **2 espaces** (standard YAML).
- Ne jamais utiliser de tabulations dans les fichiers YAML.

### 4. Fichiers JSON

- Tous les fichiers `.json` doivent être syntaxiquement valides.
- Utiliser l'indentation à **2 espaces**.

### 5. Liens dans la documentation

- Tous les **liens relatifs** (internes au repo) dans les fichiers Markdown doivent être valides.
- Les liens externes (HTTP/HTTPS) ne sont pas vérifiés en CI (pour éviter les faux positifs dus au réseau).
- Préférer les **liens relatifs** pour les fichiers internes au repo.

---

## 🔄 Workflow recommandé pour les contributions IA

1. **Avant de commencer** : lire ce fichier et le fichier `README.md`.
2. **Pendant le développement** :
   - Suivre les conventions de code du projet.
   - Écrire des commentaires clairs en **français** (langue du projet).
   - Tester les changements localement si possible.
3. **Avant de soumettre une PR** :
   - S'assurer que tous les fichiers Markdown, YAML, JSON sont valides.
   - Vérifier que les liens dans la documentation sont valides.
   - Vérifier que le workflow CI passerait (simuler mentalement les checks).
4. **Sur la PR** :
   - La CI se lancera automatiquement.
   - Un commentaire automatique sera posté par le bot CI.
   - ❌ Si la CI échoue → corriger avant de demander un review.
   - ✅ Si la CI passe → la PR peut être revue et mergée.

---

## 📁 Structure du projet

```
.
├── README.md                          ← README principal avec badge CI
├── .markdownlint.json                 ← Config markdownlint
├── .github/
│   ├── workflows/
│   │   └── ci-linux.yml               ← Pipeline CI principale
│   ├── mlc_config.json                ← Config markdown-link-check
│   └── copilot-instructions.md        ← Ce fichier
└── docs/
    ├── README.md                      ← Index de la documentation
    ├── 01-standards-et-qualite.md
    ├── 02-structure-repo-template-ia.md
    ├── 03-workflows-humain-ia.md
    ├── 04-automatisation-ci-qualite.md
    └── 05-onboarding-et-accessibilite.md
```

---

## ⚠️ Points d'attention spécifiques

- Ce repo est un **template** : il sera utilisé comme base pour d'autres projets.
  Toute modification doit donc être générique et réutilisable.
- La langue principale du projet est le **français**.
- Les commentaires dans les fichiers de configuration (YAML, JSON) doivent être en français.
- Toujours utiliser les **actions officielles GitHub** (`actions/*`) dans les workflows.
- Ne jamais stocker de **secrets ou credentials** dans les fichiers du repo.

---

*Ces instructions sont à destination des agents IA et assistants comme GitHub Copilot.
Elles doivent être respectées pour garantir la qualité et la maintenabilité du projet.*
