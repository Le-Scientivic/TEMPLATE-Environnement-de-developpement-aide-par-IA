# Automatisation CI/CD et qualité

> **En résumé** : tout ce qui est répétitif, objectif et vérifiable doit sortir du cerveau humain et aller dans l'automatisation. Cette note couvre les automatisations essentielles : CI, lint, tests, sécurité, conventions de commit, et protections de branches.

---

## Pourquoi automatiser ?

- **Libère le cerveau humain** pour les tâches qui nécessitent vraiment de la réflexion.
- **Garantit la cohérence** : les mêmes vérifications tournent à chaque commit, pour tout le monde.
- **Permet à l'IA de travailler dans un cadre** : Copilot Coding Agent peut s'appuyer sur des CI claires.
- **Réduit les erreurs** : un bug ou une faille détecté par une CI en 2 minutes vaut mieux qu'un bug en prod.

**Principe** : si tu te retrouves à faire la même vérification à la main 3 fois, automatise-la.

---

## 1. GitHub Actions — Les bases

GitHub Actions permet de définir des workflows déclenchés sur des événements (push, PR, schedule, etc.).

**Structure d'un workflow** :

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup [language]
        uses: actions/setup-[language]@v[X]
        with:
          [language]-version: '[version]'

      - name: Install dependencies
        run: [install command]

      - name: Lint
        run: [lint command]

      - name: Format check
        run: [format check command]

      - name: Tests
        run: [test command]

      - name: Coverage
        run: [coverage command]
```

**Source** : [GitHub Docs — GitHub Actions](https://docs.github.com/en/actions)

---

## 2. Ce qu'il faut automatiser — par priorité

### Priorité 1 — Qualité de base (indispensable)

| Automatisation | Outil(s) courants | Quand |
|---|---|---|
| **Formatage du code** | Prettier (JS/TS), Black (Python), gofmt (Go), rustfmt (Rust) | À chaque PR |
| **Lint** | ESLint (JS/TS), Pylint/Flake8/Ruff (Python), golangci-lint (Go) | À chaque PR |
| **Tests unitaires** | Jest (JS), pytest (Python), go test (Go), cargo test (Rust) | À chaque PR |
| **Couverture minimale** | Istanbul/nyc (JS), pytest-cov (Python) | À chaque PR |

### Priorité 2 — Sécurité (très recommandé)

| Automatisation | Outil(s) courants | Quand |
|---|---|---|
| **Dépendances vulnérables** | Dependabot (GitHub natif), npm audit, pip-audit | À chaque PR + schedule |
| **Détection de secrets** | GitHub Secret Scanning (natif), gitleaks, truffleHog | À chaque push |
| **Analyse statique sécurité** | CodeQL (GitHub natif), Semgrep, Bandit (Python) | À chaque PR |
| **Mise à jour automatique des dépendances** | Dependabot | Schedule hebdo |

### Priorité 3 — Documentation et conformité

| Automatisation | Outil(s) courants | Quand |
|---|---|---|
| **Liens cassés dans les docs** | markdown-link-check | À chaque PR sur docs/ |
| **Orthographe/grammaire docs** | Vale, cspell | À chaque PR sur docs/ |
| **Convention de commits** | commitlint, action-conventional-commits | À chaque PR |
| **Labels automatiques** | labeler (GitHub Action) | À chaque PR |
| **Génération de changelog** | Release Please, semantic-release | À chaque merge sur main |

### Priorité 4 — Release et déploiement

| Automatisation | Quand |
|---|---|
| **Versioning automatique** (SemVer) | Au merge sur main avec Conventional Commits |
| **Génération du CHANGELOG** | Avec Release Please ou semantic-release |
| **Création de la release GitHub** | Automatique depuis les tags |
| **Publication (npm, PyPI, Docker, etc.)** | Sur tag ou release |

---

## 3. Exemples de workflows GitHub Actions

### Workflow CI — JavaScript/TypeScript (Node.js)

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Format check
        run: npm run format:check

      - name: Tests
        run: npm test -- --coverage

      - name: Build
        run: npm run build
```

---

### Workflow CI — Python

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          pip install -e ".[dev]"

      - name: Lint
        run: ruff check .

      - name: Format check
        run: ruff format --check .

      - name: Type check
        run: mypy src/

      - name: Tests
        run: pytest --cov=src --cov-report=term-missing
```

---

### Workflow — Vérification des conventions de commit

```yaml
# .github/workflows/commitlint.yml
name: Commit Convention

on:
  pull_request:

jobs:
  commitlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Check commit convention
        uses: wagoid/commitlint-github-action@v6
```

Configuration `commitlint.config.js` :
```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```

---

### Workflow — Vérification des liens dans la documentation

```yaml
# .github/workflows/docs-links.yml
name: Check documentation links

on:
  pull_request:
    paths:
      - 'docs/**'
      - '**.md'

jobs:
  markdown-links:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Check Markdown links
        uses: gaurav-nelson/github-action-markdown-link-check@v1
        with:
          use-quiet-mode: 'yes'
          config-file: '.github/mlc_config.json'
```

---

### Workflow — Release automatique (Release Please)

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    runs-on: ubuntu-latest
    steps:
      - uses: googleapis/release-please-action@v4
        with:
          release-type: node  # ou python, go, rust, simple...
```

Release Please génère automatiquement les PR de release, le CHANGELOG et les tags Git en lisant les Conventional Commits.

**Source** : [Release Please GitHub](https://github.com/googleapis/release-please)

---

## 4. Configuration Dependabot

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"  # ou pip, gomod, cargo, github-actions...
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    labels:
      - "dependencies"
    commit-message:
      prefix: "chore"
      prefix-development: "chore"
      include: "scope"
    # Limiter le nombre de PR ouvertes simultanément
    open-pull-requests-limit: 5

  # Toujours mettre à jour les GitHub Actions aussi
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

**Source** : [GitHub Docs — Configuring Dependabot](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/configuring-dependabot-security-updates)

---

## 5. Convention de commits avec commitlint

### Installation locale (Node.js / husky)

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional husky
npx husky init
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg
```

Cela vérifie chaque message de commit **localement** avant qu'il soit créé.  
Le workflow GitHub Actions le vérifie ensuite sur les PR en CI.

---

## 6. `.editorconfig` — Cohérence entre éditeurs

Le fichier `.editorconfig` garantit que tous les éditeurs (VS Code, IntelliJ, Vim, etc.) utilisent les mêmes conventions de base :

```ini
# .editorconfig
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
insert_final_newline = true

[*.py]
indent_size = 4

[Makefile]
indent_style = tab

[*.md]
trim_trailing_whitespace = false
```

**Source** : [editorconfig.org](https://editorconfig.org/)

---

## 7. Labeler automatique — Triage des PR

```yaml
# .github/labeler.yml
# (utilisé avec actions/labeler)
documentation:
  - docs/**
  - "*.md"

ci:
  - .github/workflows/**
  - .github/dependabot.yml

tests:
  - tests/**
  - "**/*.test.*"
  - "**/*.spec.*"

dependencies:
  - package.json
  - package-lock.json
  - requirements*.txt
  - go.mod
  - Cargo.toml
```

Workflow associé :
```yaml
# .github/workflows/labeler.yml
name: Label PR

on:
  pull_request:

jobs:
  label:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/labeler@v5
```

---

## 8. Protections de branches — Rappel

Pour compléter les automatisations, activer dans GitHub Settings > Branches (pour `main`) :

- **Require status checks to pass** : sélectionner les jobs CI critiques
- **Require branches to be up to date** : la branche doit être à jour avec `main` avant de merger
- **Require a pull request** : pas de push direct sur `main`
- **Require signed commits** : optionnel mais recommandé pour les projets critiques

⚠️ Note : les "commits signés requis" peuvent créer des frictions avec le Copilot Coding Agent. Si tu utilises l'agent, adapte cette règle en conséquence.

**Source** : [OpenSSF OSPS Baseline](https://openssf.org/projects/osps-baseline/) · [GitHub Docs — Branch protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)

---

## 9. Récapitulatif — Checklist d'automatisation minimale

- [ ] `.github/workflows/ci.yml` avec lint + tests
- [ ] `.github/workflows/` — vérification de convention de commit
- [ ] `.github/dependabot.yml` configuré
- [ ] `.editorconfig` à la racine
- [ ] Secret Scanning activé dans GitHub Settings
- [ ] Dependabot Alerts activé dans GitHub Settings
- [ ] Branche `main` protégée avec statuts CI requis
- [ ] Labels automatiques configurés (optionnel mais pratique)
- [ ] Release Please ou équivalent pour le versioning automatique (optionnel)

---

*Sources : [docs.github.com/actions](https://docs.github.com/en/actions) · [openssf.org](https://openssf.org/projects/osps-baseline/) · [editorconfig.org](https://editorconfig.org/) · [github.com/googleapis/release-please](https://github.com/googleapis/release-please) · [commitlint.js.org](https://commitlint.js.org/)*
