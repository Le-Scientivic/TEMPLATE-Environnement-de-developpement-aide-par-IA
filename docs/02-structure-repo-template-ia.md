# Structure d'un repo template GitHub IA-friendly

> **En résumé** : un repo bien structuré est le fondement de tout. Il guide l'IA, contraint la qualité, facilite l'onboarding humain, et rend le projet transmissible. Cette note décrit l'architecture recommandée pour un repo template GitHub conçu pour travailler avec Copilot et d'autres outils IA.

---

## Pourquoi la structure du repo est cruciale quand on travaille avec l'IA

L'IA (Copilot, Coding Agent) lit le contexte du repo pour générer du code. Un repo sans structure claire, c'est comme donner une mission à quelqu'un sans lui expliquer les règles du jeu : les résultats seront aléatoires.

Un repo bien structuré, c'est un **prompt durable** :
- Il instruit l'IA sur tes conventions, ton architecture, ton style.
- Il empêche Copilot de mélanger plusieurs architectures ou d'introduire des dépendances non voulues.
- Il guide les humains qui arrivent sur le projet pour la première fois.

**Source** : [GitHub Docs — Adding repository instructions for Copilot](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions)

---

## Architecture recommandée

Voici la structure cible d'un repo template IA-friendly :

```
.
├── .github/
│   ├── copilot-instructions.md          # Instructions globales pour Copilot
│   ├── instructions/                    # Instructions Copilot par contexte/dossier
│   │   ├── tests.instructions.md
│   │   └── docs.instructions.md
│   ├── ISSUE_TEMPLATE/                  # Formulaires d'issues structurés
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   ├── PULL_REQUEST_TEMPLATE.md         # Template de Pull Request
│   ├── workflows/                       # GitHub Actions (CI/CD)
│   │   ├── ci.yml
│   │   └── release.yml
│   └── CODEOWNERS                       # Propriétaires des parties du code
├── docs/
│   ├── README.md                        # Index de la documentation
│   ├── tutorials/
│   ├── how-to/
│   ├── reference/
│   ├── explanation/
│   └── adr/                             # Architecture Decision Records
├── src/                                 # Code source principal
├── tests/                               # Tests automatisés
├── .editorconfig                        # Cohérence de style entre éditeurs
├── .gitignore
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CODEOWNERS
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── SECURITY.md
```

---

## Détail de chaque fichier clé

### `.github/copilot-instructions.md`

C'est **le fichier le plus important** pour guider l'IA.  
Il est automatiquement ajouté au contexte de Copilot Chat et du Coding Agent dans VS Code et sur GitHub.com.

**Source** : [GitHub Docs — Copilot custom instructions](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions)

Ce fichier doit contenir :

```markdown
# Instructions pour Copilot

## Stack et architecture
- Langage principal : [à compléter]
- Framework : [à compléter]
- Structure des dossiers : src/ pour le code, tests/ pour les tests, docs/ pour la documentation

## Style de code
- [Conventions de nommage]
- [Règles d'indentation]
- [Longueur maximale des lignes]

## Tests
- Tous les changements doivent être accompagnés de tests
- Couverture minimale : [X]%
- Framework de test : [à compléter]

## Sécurité
- Ne jamais écrire de secrets ou clés dans le code
- Valider toutes les entrées utilisateur
- [Autres règles de sécurité]

## Documentation
- Documenter toutes les fonctions publiques
- Mettre à jour docs/ si le comportement change
- Respecter la structure Diátaxis (tutorials, how-to, reference, explanation)

## Conventions de commits
- Utiliser Conventional Commits : type(scope): description
- Types : feat, fix, docs, style, refactor, test, ci, chore

## Principes
- Faire le changement minimal correct
- Ne pas inventer si ambigu : poser la question
- Préférer la lisibilité à la "magie"
```

---

### `.github/instructions/`

Pour des instructions plus spécifiques par contexte ou par dossier.  
Ces fichiers `.instructions.md` permettent de donner à Copilot des règles précises selon le type de fichier ou la zone du repo.

Exemples :

**`tests.instructions.md`** :
```markdown
---
applyTo: "tests/**"
---
# Instructions pour les tests

- Utiliser [framework de test]
- Un test = un comportement vérifié
- Nommer les tests : should_[comportement]_when_[condition]
- Pas de logique métier dans les tests
```

**`docs.instructions.md`** :
```markdown
---
applyTo: "docs/**"
---
# Instructions pour la documentation

- Respecter la structure Diátaxis
- Écrire pour un humain normal, pas pour un expert
- Inclure des exemples concrets
- Vérifier que les liens sont fonctionnels
```

---

### `.github/ISSUE_TEMPLATE/`

Les formulaires d'issues structurent les demandes dès la création. Ça pousse vers des issues plus claires, plus actionnables, et mieux comprises par l'IA.

**`bug_report.yml`** (exemple minimal) :
```yaml
name: 🐛 Rapport de bug
description: Signaler un comportement inattendu
title: "[BUG] "
labels: ["bug"]
body:
  - type: textarea
    id: description
    attributes:
      label: Description
      description: Que s'est-il passé ?
    validations:
      required: true
  - type: textarea
    id: reproduction
    attributes:
      label: Étapes pour reproduire
    validations:
      required: true
  - type: textarea
    id: expected
    attributes:
      label: Comportement attendu
  - type: input
    id: version
    attributes:
      label: Version
```

**`feature_request.yml`** (exemple minimal) :
```yaml
name: ✨ Demande de fonctionnalité
description: Proposer une nouvelle fonctionnalité
title: "[FEAT] "
labels: ["enhancement"]
body:
  - type: textarea
    id: problem
    attributes:
      label: Quel problème résout cette fonctionnalité ?
    validations:
      required: true
  - type: textarea
    id: solution
    attributes:
      label: Solution proposée
  - type: textarea
    id: alternatives
    attributes:
      label: Alternatives considérées
  - type: textarea
    id: acceptance
    attributes:
      label: Critères d'acceptation
    validations:
      required: true
```

---

### `.github/PULL_REQUEST_TEMPLATE.md`

Ce fichier force l'auteur (humain ou IA) à documenter chaque PR.

```markdown
## Pourquoi ce changement ?

<!-- Problème résolu ou fonctionnalité ajoutée. Lien vers l'issue : Fixes #XXX -->

## Qu'est-ce qui a changé ?

<!-- Description technique du changement -->

## Comment a-t-il été testé ?

- [ ] Tests unitaires ajoutés/mis à jour
- [ ] Tests d'intégration validés
- [ ] Testé manuellement (décrire)

## Checklist

- [ ] Le code respecte les conventions du projet
- [ ] La documentation a été mise à jour si nécessaire
- [ ] Le CHANGELOG a été mis à jour
- [ ] Pas de secrets dans le code
- [ ] Les tests passent en CI

## Captures d'écran (si applicable)

<!-- Pour les changements d'interface -->
```

---

### `CONTRIBUTING.md`

Guide de contribution pour les humains ET pour l'IA. Il doit expliquer :

- Comment mettre en place l'environnement de développement local
- Conventions de code et de commits
- Processus de soumission d'une PR
- Comment lancer les tests
- Qui contacter en cas de question

**Source** : [GitHub Docs — Setting guidelines for repository contributors](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors)

---

### `CODEOWNERS`

Définit qui est responsable de quelle partie du code. Les propriétaires sont automatiquement ajoutés comme relecteurs sur les PR concernant leurs fichiers.

```
# Exemple CODEOWNERS
* @ton-username                    # Propriétaire par défaut : tout le repo
/docs/ @ton-username               # Documentation
/src/auth/ @ton-username           # Module auth
/.github/ @ton-username            # Configuration GitHub
```

**Source** : [GitHub Docs — About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)

---

### `docs/adr/`

Les **ADR (Architecture Decision Records)** documentent les décisions importantes d'architecture. Format recommandé :

```markdown
# ADR-001 : Titre de la décision

**Date** : YYYY-MM-DD  
**Statut** : Proposé / Accepté / Remplacé par ADR-XXX

## Contexte

Pourquoi cette décision était nécessaire.

## Options considérées

1. Option A
2. Option B
3. Option C

## Décision

On choisit l'option X parce que...

## Conséquences

- Avantages...
- Inconvénients / Points de vigilance...
```

**Source** : [ADR GitHub](https://adr.github.io/) · Pratique recommandée par les développeurs seniors sur des projets durables.

---

### `SECURITY.md`

Politique de divulgation responsable des vulnérabilités. Obligatoire pour un projet sérieux.

```markdown
# Politique de sécurité

## Versions supportées

| Version | Support |
|---|---|
| x.x.x | ✅ |
| < x.x.x | ❌ |

## Signaler une vulnérabilité

Pour signaler une vulnérabilité, **ne pas créer une issue publique**.  
Contacter directement : [email ou GitHub Security Advisory]

Nous nous engageons à répondre sous 48 heures et à corriger les vulnérabilités critiques sous 7 jours.
```

**Source** : [OpenSSF OSPS Baseline](https://openssf.org/projects/osps-baseline/) · [GitHub Docs — Adding a security policy](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository)

---

## Protections de branches recommandées

Dans les paramètres du repo (Settings > Branches), pour la branche `main` :

| Protection | Recommandation |
|---|---|
| Require a pull request before merging | ✅ Activé |
| Require approvals | ✅ Au moins 1 (ou 0 si solo) |
| Require status checks to pass | ✅ CI doit passer |
| Require branches to be up to date | ✅ Recommandé |
| Do not allow bypassing the above settings | ✅ Recommandé |
| Restrict who can push | ✅ Recommandé en équipe |

**Source** : [OpenSSF OSPS Baseline](https://openssf.org/projects/osps-baseline/) · [GitHub Docs — Branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)

---

## Paramètres GitHub recommandés

Dans les paramètres du repo :

- ✅ **Activer Issues** (pour le suivi du travail)
- ✅ **Activer Discussions** (si projet communautaire)
- ✅ **Activer Dependabot Alerts** (sécurité des dépendances)
- ✅ **Activer Secret Scanning** (détection de secrets accidentels)
- ✅ **Activer Code Scanning** (CodeQL) si applicable
- ✅ **Configurer le merge strategy** : Squash and merge recommandé pour un historique propre
- ✅ **Activer "Automatically delete head branches"** après merge

---

## Checklist — Repo template prêt à l'usage

- [ ] `.github/copilot-instructions.md` créé et documenté
- [ ] `.github/ISSUE_TEMPLATE/` avec bug_report et feature_request
- [ ] `.github/PULL_REQUEST_TEMPLATE.md` créé
- [ ] `.github/workflows/ci.yml` avec lint et tests
- [ ] `CODEOWNERS` créé
- [ ] `CONTRIBUTING.md` créé
- [ ] `SECURITY.md` créé
- [ ] `CODE_OF_CONDUCT.md` créé
- [ ] `CHANGELOG.md` initialisé
- [ ] `docs/` structuré selon Diátaxis
- [ ] `docs/adr/` créé pour les décisions d'architecture
- [ ] Branche `main` protégée
- [ ] Dependabot activé
- [ ] Secret scanning activé

---

*Sources : [docs.github.com](https://docs.github.com) · [openssf.org](https://openssf.org/projects/osps-baseline/) · [adr.github.io](https://adr.github.io/) · [diataxis.fr](https://diataxis.fr/)*
