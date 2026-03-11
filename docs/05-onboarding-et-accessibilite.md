# Onboarding humain et accessibilité

> **En résumé** : un bon projet n'est pas seulement bien codé — il est aussi **accessible**. Accessible pour un nouveau développeur qui arrive, pour toi dans 6 mois, pour une IA qui lit le contexte, et pour toute personne qui cherche à comprendre rapidement. Cette note couvre l'onboarding, l'accessibilité du projet, et les principes pour rendre un projet "humain-friendly".

---

## Pourquoi l'onboarding est crucial

Un repo sans onboarding clair, c'est comme une maison sans plan : tu peux finir par t'y retrouver, mais ça prend du temps et c'est frustrant. Pour un projet de qualité, l'onboarding doit permettre à quelqu'un de nouveau (humain ou IA) de comprendre et contribuer en **moins de 30 minutes**.

C'est aussi un révélateur de qualité : si tu ne peux pas expliquer rapidement ton projet, c'est souvent qu'il manque de structure.

---

## 1. Le README — Première porte d'entrée

Le README est le document le plus lu de tout projet. Il doit être **excellent**.

### Structure recommandée d'un bon README

```markdown
# Nom du projet

> Une ligne : ce que fait le projet et pour qui.

## Démarrage rapide

Les 3 commandes pour faire tourner le projet en local.

## Ce que fait ce projet

Description plus détaillée. Contexte, problème résolu, public visé.

## Prérequis

Ce qu'il faut avoir installé avant de commencer.

## Installation

Étapes numérotées, claires, copy-pastables.

## Usage

Exemple(s) d'utilisation concret(s).

## Documentation

Lien vers docs/ pour aller plus loin.

## Contribuer

Lien vers CONTRIBUTING.md.

## Licence

Mention courte + lien vers LICENSE.
```

**À éviter** :
- Un README trop long qui essaie de tout dire
- Un README vide ou qui dit juste "TODO"
- Des instructions d'installation qui ne fonctionnent pas
- Du jargon sans explication

**Source** : [Write the Docs — How to write a README](https://www.writethedocs.org/guide/writing/beginners-guide-to-docs/)

---

## 2. CONTRIBUTING.md — Guide pour contribuer

Ce fichier explique **comment contribuer** au projet. Il est lu par les humains qui veulent contribuer ET par l'IA pour comprendre les conventions.

### Contenu recommandé

```markdown
# Guide de contribution

## Prérequis

- [Liste des outils nécessaires avec versions]
- [Comment installer l'environnement de développement]

## Démarrage local

```bash
# Cloner le repo
git clone https://github.com/[org]/[repo].git
cd [repo]

# Installer les dépendances
[commande d'installation]

# Lancer les tests
[commande de test]

# Lancer en mode développement
[commande de dev]
```

## Workflow de contribution

1. Créer une issue pour décrire le problème ou la fonctionnalité
2. Créer une branche depuis `main` : `git checkout -b feat/ma-fonctionnalite`
3. Coder en petits commits avec Conventional Commits
4. Ouvrir une Pull Request en utilisant le template
5. Attendre la revue et corriger si nécessaire

## Conventions

- Commits : [Conventional Commits](https://www.conventionalcommits.org/)
- Style de code : [lien vers la config ou les règles]
- Tests : [couverture minimale, framework utilisé]
- Documentation : [comment documenter]

## Où trouver de l'aide

- Issues GitHub pour les questions liées au projet
- [Autres canaux si applicable]
```

**Source** : [GitHub Docs — Setting up CONTRIBUTING.md](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors)

---

## 3. Guide pour débutant — "Je commence ici"

Pour les projets qui accueillent des débutants, ajouter un guide dédié dans `docs/tutorials/` :

**`docs/tutorials/00-demarrage-rapide.md`** :

```markdown
# Démarrage rapide

Bienvenue ! Ce guide te mènera de zéro à un environnement fonctionnel
en moins de 30 minutes.

## Ce dont tu as besoin

- [ ] [Outil 1] installé (lien vers installation)
- [ ] [Outil 2] installé (lien vers installation)
- [ ] Un compte GitHub

## Étape 1 — Cloner le projet

```bash
git clone https://github.com/[org]/[repo].git
cd [repo]
```

## Étape 2 — Installer les dépendances

```bash
[commande]
```

## Étape 3 — Vérifier que tout fonctionne

```bash
[commande de test ou de démarrage]
```

Tu devrais voir : [ce qu'on voit quand ça marche]

## Et maintenant ?

Si tu veux contribuer, lis [CONTRIBUTING.md](../../CONTRIBUTING.md).
Si tu veux comprendre l'architecture, lis [explanation/architecture.md](../explanation/architecture.md).
```

---

## 4. Accessibilité de la documentation

### Écrire pour un humain normal

Que ton lecteur soit un développeur junior, un collègue qui revient après 3 mois, ou toi dans 6 mois :

- **Pas de jargon non expliqué** : si tu utilises un terme technique, explique-le la première fois.
- **Exemples concrets** : toujours montrer avec un exemple plutôt que de rester abstrait.
- **Listes > paragraphes denses** : les listes sont plus faciles à parcourir.
- **Titres clairs** : quelqu'un qui parcourt la doc doit comprendre de quoi parle chaque section juste avec les titres.
- **Langage actif** : "Lance cette commande" plutôt que "Cette commande doit être lancée".

### Accessibilité pour les outils IA

La documentation aide aussi l'IA à comprendre le projet. Pour qu'elle soit bien utilisable par Copilot :

- Les fichiers Markdown bien structurés avec des titres (`#`, `##`) sont mieux analysés.
- Les blocs de code avec le langage spécifié (` ```python `, ` ```bash `) sont mieux compris.
- Les liens internes entre fichiers de doc aident à la navigation.
- Les exemples dans les docs peuvent être utilisés par Copilot comme contexte.

---

## 5. Architecture Decision Records (ADR) — Onboarding pour les décisions passées

Les ADR permettent à un nouvel arrivant de comprendre **pourquoi** le projet est comme il est.

Sans ADR, on se retrouve à dire "on a toujours fait comme ça" sans pouvoir expliquer pourquoi — et parfois la raison initiale n'est plus valable.

**Créer un ADR pour chaque décision importante** :
- Choix de framework ou langage
- Choix d'architecture (monolithe vs. microservices, SQL vs. NoSQL, etc.)
- Abandon d'une approche en faveur d'une autre
- Adoption d'une convention de code ou de doc

**Exemple d'ADR minimal** (`docs/adr/001-choix-du-framework.md`) :

```markdown
# ADR-001 : Choix du framework principal

**Date** : 2024-01-15  
**Statut** : Accepté

## Contexte

Nous démarrons un nouveau projet web et devons choisir un framework frontend.

## Options considérées

1. **React** : large écosystème, beaucoup de ressources
2. **Vue.js** : courbe d'apprentissage plus douce
3. **Svelte** : performances, syntaxe plus simple

## Décision

Nous utilisons **Vue.js** parce que l'équipe a déjà de l'expérience avec,
et que la courbe d'apprentissage est adaptée au projet.

## Conséquences

- L'équipe peut commencer rapidement sans formation
- L'écosystème est légèrement plus petit que React
- À réévaluer si l'équipe évolue significativement
```

**Source** : [adr.github.io](https://adr.github.io/)

---

## 6. Onboarding avec l'IA — Instructions Copilot

Les fichiers d'instructions Copilot font partie de l'onboarding : ils permettent à l'IA de comprendre le projet sans avoir à lire tout le code.

**`.github/copilot-instructions.md`** doit aussi servir d'onboarding express pour l'IA :

```markdown
# Ce projet en bref

[Une phrase sur ce que fait le projet]

## Architecture
[Description courte de l'architecture, 3-5 lignes]

## Stack
- Langage : ...
- Framework : ...
- Base de données : ...
- Tests : ...

## Conventions importantes
[Les 3-5 règles les plus importantes du projet]
```

---

## 7. Accessibilité des interfaces (si applicable)

Si ton projet comporte une interface utilisateur, les standards d'accessibilité web sont importants :

### WCAG (Web Content Accessibility Guidelines)

**Source officielle** : <https://www.w3.org/WAI/WCAG22/quickref/>

Les 4 principes POUR (Perceivable, Operable, Understandable, Robust) :

| Principe | Exemples pratiques |
|---|---|
| **Perceptible** | Images avec `alt`, contrastes suffisants, sous-titres vidéo |
| **Utilisable** | Navigation clavier possible, pas de temps limité obligatoire |
| **Compréhensible** | Messages d'erreur clairs, langue indiquée dans le HTML |
| **Robuste** | HTML valide, compatible avec les lecteurs d'écran |

**Niveaux de conformité** : A (minimal), AA (recommandé), AAA (idéal)

**Conseil pratique** : intégrer une vérification d'accessibilité en CI avec `axe-core` ou `pa11y`.

---

## 8. CODE_OF_CONDUCT.md

Un code de conduite définit les règles de comportement pour la communauté du projet. Même pour un projet solo, c'est une bonne pratique — et GitHub le recommande.

Le **Contributor Covenant** est la référence la plus utilisée :

**Source** : <https://www.contributor-covenant.org/>

```markdown
# Code de conduite

Ce projet adhère au [Contributor Covenant](https://www.contributor-covenant.org/)
version 2.1. En participant, tu t'engages à respecter ce code.

## Notre engagement

Nous nous engageons à faire de ce projet une expérience accueillante
pour tout le monde, sans discrimination d'aucune sorte.

## Comportements attendus

- Utiliser un langage accueillant et inclusif
- Respecter les différents points de vue et expériences
- Accepter constructivement les critiques
- Se concentrer sur ce qui est meilleur pour la communauté

## Comportements inacceptables

- Harcèlement de toute nature
- Commentaires offensants ou dégradants
- Comportement perturbateur

## Application

En cas de comportement inacceptable, contacter : [email ou autre]
```

---

## 9. Checklist — Onboarding et accessibilité

- [ ] `README.md` avec démarrage rapide en moins de 5 étapes
- [ ] `CONTRIBUTING.md` avec le workflow de contribution complet
- [ ] `docs/tutorials/00-demarrage-rapide.md` pour les débutants
- [ ] `CODE_OF_CONDUCT.md` présent
- [ ] `docs/adr/` avec les premières décisions documentées
- [ ] `.github/copilot-instructions.md` contient un résumé du projet pour l'IA
- [ ] Documentation sans jargon non expliqué
- [ ] Exemples de code fonctionnels et copy-pastables
- [ ] Liens entre les documents de documentation
- [ ] Accessibilité WCAG AA si interface utilisateur

---

## 10. Test d'onboarding — "Le test du débutant"

Avant de considérer ton onboarding comme bon, fais ce test :

1. Ouvre une fenêtre de navigateur privée (ou utilise un autre compte).
2. Essaie de suivre le README et CONTRIBUTING.md à la lettre, sans aucun contexte préalable.
3. Note tout ce qui n'est pas clair, qui manque, ou qui ne fonctionne pas.
4. Corrige avant de considérer le projet comme "prêt à accueillir des contributeurs".

Variante IA : demande à Copilot de résumer ce que fait le projet et comment y contribuer, en lisant uniquement les fichiers de surface (README, CONTRIBUTING). Si la réponse est bonne, l'onboarding est bon pour l'IA aussi.

---

*Sources : [writethedocs.org](https://www.writethedocs.org/guide/) · [docs.github.com](https://docs.github.com) · [adr.github.io](https://adr.github.io/) · [www.w3.org/WAI/WCAG22](https://www.w3.org/WAI/WCAG22/quickref/) · [contributor-covenant.org](https://www.contributor-covenant.org/)*
