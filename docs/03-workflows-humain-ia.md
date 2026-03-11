# Workflows Humain + IA

> **En résumé** : la collaboration entre un humain et une IA est une discipline à part entière. Ce n'est pas "demander à l'IA de coder à ma place". C'est un cycle structuré où l'humain pilote, l'IA produit des suggestions, et la qualité est vérifiée à chaque étape.

---

## Principe fondamental

> L'IA est un partenaire, pas un pilote automatique.

L'IA est très bonne pour :
- générer du code boilerplate rapidement,
- suggérer des implémentations,
- écrire de la documentation à partir de code existant,
- détecter des patterns connus,
- reformuler et résumer.

L'IA est mauvaise (ou risquée) pour :
- comprendre tes contraintes métier implicites,
- respecter une architecture qu'elle n'a pas vue,
- valider que le code fait vraiment ce qu'il faut faire,
- évaluer la sécurité dans ton contexte précis.

**Conclusion** : l'humain doit rester propriétaire de la décision. L'IA gère l'exécution dans un cadre défini par l'humain.

---

## Workflow complet : du besoin à la PR mergée

### Étape 1 — Cadrer la demande (humain)

Avant d'écrire une ligne de code, créer une **issue structurée** dans GitHub.

Une bonne issue contient :

```markdown
## Contexte
Pourquoi ce changement est nécessaire. Ce qui existe aujourd'hui.

## Problème
Ce qui ne fonctionne pas ou manque.

## Résultat attendu
Ce que l'utilisateur doit pouvoir faire après ce changement.

## Contraintes
- Pas de dépendances externes supplémentaires
- Doit fonctionner sur [plateforme X]
- Compatible avec [version Y]

## Critères d'acceptation
- [ ] L'utilisateur peut [action A]
- [ ] Le comportement B est préservé
- [ ] Les tests passent

## Risques identifiés
- Impact sur le module X
- Migration de données si applicable

## Documentation à mettre à jour
- README section "Installation"
- docs/reference/api.md
```

**Pourquoi c'est important** : une issue bien structurée donne à l'IA (et à toi) un cadre précis. Copilot peut s'appuyer dessus pour proposer une implémentation cohérente.

**Source** : [GitHub Docs — Using Copilot to create issues](https://docs.github.com/en/copilot/how-tos/github-flow/using-github-copilot-to-create-issues)

---

### Étape 2 — Demander une analyse avant de coder (humain + IA)

Avant de lancer le code, utiliser Copilot Chat pour :

**Prompt type — Analyse d'impact** :
```
En te basant sur le code existant dans ce dépôt et l'issue #XXX,
identifie :
1. Les fichiers qui seront probablement modifiés
2. Les risques de régression
3. Les tests existants qui pourraient être affectés
4. Les questions à clarifier avant de commencer
```

**Prompt type — Découpage en tâches** :
```
Décompose l'issue #XXX en sous-tâches techniques de 1 à 2 heures chacune.
Pour chaque sous-tâche, indique les fichiers concernés et les dépendances entre tâches.
```

---

### Étape 3 — Coder en petits incréments (humain + IA)

Ne jamais demander à l'IA de tout faire d'un coup. Travailler par petits morceaux :

1. **Squelette d'abord** : demander la structure, les interfaces, les signatures de fonctions.
2. **Implémentation ensuite** : compléter une fonction à la fois.
3. **Tests en parallèle** : écrire le test avant ou juste après chaque fonction.
4. **Documentation au fil de l'eau** : docstrings, commentaires importants.

**Prompt type — Implémentation ciblée** :
```
Implémente la fonction [nom] dans [fichier].
Elle doit :
- Prendre en entrée : [paramètres]
- Retourner : [résultat]
- Gérer les cas : [cas limites]
Respecte les conventions du projet définies dans .github/copilot-instructions.md.
```

**Prompt type — Génération de tests** :
```
Écris les tests unitaires pour la fonction [nom] dans [fichier].
Couvre les cas : nominal, erreur, cas limites.
Utilise le framework [X] déjà en place dans le projet.
```

**Prompt type — Documentation** :
```
Génère la documentation pour la fonction [nom].
Format : [JSDoc / docstring Python / etc.]
Inclure : description, paramètres, valeur de retour, exemples d'usage, exceptions.
```

---

### Étape 4 — Vérification de qualité (automatique + humain)

Avant toute PR, s'assurer que :

**Automatique (CI)** :
- [ ] Formatage respecté (Prettier, Black, gofmt, etc.)
- [ ] Lint passe sans erreur (ESLint, Pylint, etc.)
- [ ] Tests passent
- [ ] Couverture de tests minimale atteinte
- [ ] Scan de sécurité (dépendances, secrets)

**Humain** :
- [ ] Le code fait bien ce que l'issue demande
- [ ] L'implémentation est lisible et maintenable
- [ ] Pas de "magie" inexpliquée
- [ ] Les cas limites sont gérés
- [ ] La sécurité est correcte dans ce contexte

---

### Étape 5 — Pull Request documentée (humain + IA)

Utiliser le template de PR. Pour chaque PR :

- **Pourquoi** : lier l'issue (`Fixes #XXX`)
- **Quoi** : décrire les changements techniques
- **Comment testé** : décrire les tests manuels et automatiques
- **Impacts** : modules affectés, changements de comportement
- **Documentation** : confirmer que la doc est à jour

**Prompt type — Description de PR** :
```
À partir de ces changements de code [ou: dans cette branche],
génère une description de Pull Request qui explique :
- Le problème résolu (référence l'issue #XXX)
- Les changements techniques effectués
- Les tests ajoutés
- Les points d'attention pour le relecteur
```

---

### Étape 6 — Revue humaine obligatoire (humain)

La revue humaine est **non-négociable**, même pour du code généré par l'IA.

Le relecteur vérifie :
- **Exactitude métier** : est-ce que ça répond vraiment au besoin ?
- **Simplicité** : y a-t-il une solution plus simple ?
- **Sécurité** : y a-t-il des risques de sécurité ?
- **Lisibilité** : un humain peut-il comprendre ce code dans 6 mois ?
- **Cohérence architecturale** : respecte-t-on les conventions du projet ?

**Prompt type — Aide à la revue** :
```
Joue le rôle d'un relecteur senior. Examine cette PR et identifie :
1. Les problèmes potentiels de sécurité
2. Les problèmes de maintenabilité
3. Les cas limites non gérés
4. Les incohérences avec l'architecture du projet
Sois précis et actionnable dans tes retours.
```

---

## Prompts standards réutilisables

### Prompts de conception

```
# Conception d'architecture
Avant de coder, propose une architecture pour [fonctionnalité].
Décris les composants, leurs responsabilités, et les interfaces entre eux.
Identifie les risques et les alternatives que tu as écartées.
```

```
# Revue de conception
Examine la conception suivante : [description].
Identifie les problèmes potentiels, les couplages forts, et les opportunités d'amélioration.
```

---

### Prompts de refactoring

```
# Refactoring ciblé
Refactore [fonction/classe/module] pour améliorer [lisibilité / performance / testabilité].
Ne change pas le comportement observable.
Explique chaque changement.
```

```
# Détection de code problématique
Analyse ce fichier et identifie : code mort, duplications, fonctions trop longues,
couplages problématiques, et manques de tests.
```

---

### Prompts de débogage

```
# Diagnostic de bug
Voici le bug : [description].
Voici le code concerné : [code].
Voici les logs d'erreur : [logs].
Identifie la cause probable et propose une correction avec explication.
```

```
# Analyse de logs
Analyse ces logs d'erreur et identifie :
1. La cause racine du problème
2. L'impact potentiel
3. Les étapes pour corriger
```

---

### Prompts de documentation

```
# Mise à jour de documentation
Suite à ce changement de code [description], identifie quelle documentation
doit être mise à jour et propose les modifications correspondantes.
```

```
# Génération de how-to
À partir de cette fonctionnalité [description], écris un guide "how-to" 
pour un développeur qui veut l'utiliser dans son projet.
Format : Diátaxis how-to guide. Public : développeur junior.
```

---

### Prompts de préparation de release

```
# Résumé de release
À partir de ces commits [liste] ou de cette comparaison de branches,
génère :
1. Un résumé de release en langage humain
2. Les entrées de CHANGELOG à ajouter
3. La version SemVer recommandée (patch/minor/major)
```

---

## Utiliser le Copilot Coding Agent

Le **Coding Agent** de GitHub Copilot peut travailler de manière autonome sur une tâche définie dans une issue et ouvrir une PR.

**Quand l'utiliser** :
- Tâches bien définies avec critères d'acceptation clairs
- Changements localisés (pas de refactoring global)
- Tâches répétitives (migration de format, ajout de tests sur du code existant)
- Documentation à partir de code existant

**Quand ne PAS l'utiliser** :
- Tâches ambiguës ou sous-spécifiées
- Changements d'architecture
- Code critique (authentification, sécurité, paiements)
- Tâches qui nécessitent une compréhension du contexte métier profond

**Source** : [GitHub Docs — About Copilot Coding Agent](https://docs.github.com/en/copilot/concepts/coding-agent/about-copilot-coding-agent)

**Bonnes pratiques** :
1. Toujours écrire l'issue avec des critères d'acceptation clairs avant d'assigner à l'agent.
2. Lire la PR ouverte par l'agent avec la même attention qu'une PR humaine.
3. Ne jamais merger sans relecture humaine.
4. Utiliser les fichiers `.github/copilot-instructions.md` pour guider l'agent.

---

## Éviter les pièges courants

### Piège 1 — Trop faire confiance au code généré

**Symptôme** : merger du code généré par l'IA sans le comprendre.  
**Conséquence** : bugs invisibles, failles de sécurité, dette technique accumulée.  
**Solution** : toujours comprendre ce que le code fait avant de l'accepter.

---

### Piège 2 — Issues trop larges

**Symptôme** : "Refactore tout le module auth et ajoute OAuth"  
**Conséquence** : l'IA génère une grosse PR ingérable, difficilement relisible.  
**Solution** : découper en issues de 1-2 heures de travail.

---

### Piège 3 — Documentation générée jamais relue

**Symptôme** : docs qui décrivent un comportement qui n'existe plus, ou qui mélangent des détails d'implémentation.  
**Conséquence** : documentation trompeuse, pire que pas de doc.  
**Solution** : relire toute documentation générée par l'IA avant de commiter.

---

### Piège 4 — Dépendances introduites à la légère

**Symptôme** : Copilot propose d'utiliser une nouvelle bibliothèque pour un problème simple.  
**Conséquence** : dépendances inutiles, surface d'attaque élargie.  
**Solution** : toujours questionner chaque nouvelle dépendance. Préférer la solution la plus simple.

---

### Piège 5 — Hallucination d'API ou de comportement

**Symptôme** : l'IA génère du code qui appelle une fonction/API qui n'existe pas ou a changé.  
**Conséquence** : erreurs runtime, pertes de temps.  
**Solution** : toujours vérifier les appels à des APIs externes dans la documentation officielle. Lancer les tests.

---

## Rituels quotidiens recommandés

| Moment | Action |
|---|---|
| Début de session | Rappeler à Copilot le contexte en cours (issue, fichier, objectif) |
| Avant chaque commit | Relire le diff, s'assurer qu'on comprend chaque ligne |
| Avant chaque PR | Vérifier la checklist de qualité |
| Fin de semaine | Mettre à jour CHANGELOG, ADR si décision prise |
| Avant release | Revue de sécurité, tests complets, mise à jour docs |

---

*Sources : [docs.github.com/copilot](https://docs.github.com/en/copilot) · [google.github.io/eng-practices](https://google.github.io/eng-practices/) · [conventionalcommits.org](https://www.conventionalcommits.org/)*
