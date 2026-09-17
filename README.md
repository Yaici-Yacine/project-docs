# project-docs — Skill pour gérer la documentation projet

> 📌 Skill créé par **[@Yaici-Yacine](https://github.com/Yaici-Yacine)** — installable via le CLI **[`skills`](https://skills.sh)** (`npx skills` / `bunx skills`).

📦 Dépôt : [Yaici-Yacine/project-docs](https://github.com/Yaici-Yacine/project-docs)

> 💡 Le CLI `skills` (paquet npm `skills`, maintenu par **Vercel Labs**) est le **mécanisme d'installation**. Le contenu de ce skill (`SKILL.md`, `templates/`) est quant à lui créé et maintenu par **Yaici-Yacine**.

---

**`project-docs`** est un skill qui crée et maintient une arborescence `docs/` structurée en deux parties : `docs/skill/` (règles, conventions, décisions) et `docs/wiki/` (documentation des features). Ce README explique comment **vos utilisateurs** peuvent l'installer via le CLI skill.sh, et comment l'utiliser au quotidien.

---

## Prérequis

- **Node.js ≥ 18** (pour `npx`) **OU Bun** (pour `bunx`)
- **Git** (le CLI `skills` clone les skills depuis GitHub)
- Un agent compatible : Claude Code, Codex, Cursor, GitHub Copilot, OpenCode, etc.

Vérifiez votre runtime :

```bash
node --version   # ≥ 18
# ou
bun --version
```

---

## Installation locale pour développement

Pour tester votre skill en local **avant** de le publier sur GitHub :

```bash
git clone https://github.com/Yaici-Yacine/project-docs.git
cd project-docs
bunx skills add . --skill project-docs
```

Ou en une ligne, sans cloner, depuis n'importe quel dossier de test :

```bash
bunx skills add /chemin/vers/project-docs --skill project-docs
```

> C'est la commande la plus rapide pour itérer : vous modifiez `SKILL.md` / `templates/`, puis vous relancez la même commande pour re-installer la nouvelle version.

---

## Installation pour vos utilisateurs (skill.sh)

> Une fois publié sur GitHub, vos utilisateurs peuvent installer le skill avec l'une des commandes ci-dessous.

La méthode recommandée pour installer **uniquement** ce skill :

```bash
bunx skills add Yaici-Yacine/project-docs --skill project-docs
```

Équivalent avec `npx` :

```bash
npx skills add Yaici-Yacine/project-docs --skill project-docs
```

Équivalent avec URL GitHub complète :

```bash
npx skills add https://github.com/Yaici-Yacine/project-docs --skill project-docs
```

Pour installer **tous les skills** de votre dépôt d'un coup (sélection interactive) :

```bash
npx skills add Yaici-Yacine/project-docs --all
```

> Le skill est installé dans le dossier de configuration de votre agent (`.claude/skills/project-docs/` pour Claude Code, `.agents/skills/project-docs/` pour OpenCode, etc.). Voir [Où le skill est installé](#où-le-skill-est-installé).

---

## Publier votre skill sur GitHub

Quelques étapes pour que vos utilisateurs puissent installer votre skill :

- **Poussez le dépôt sur GitHub** (`git init`, `git add .`, `git commit`, `git remote add origin ...`, `git push`).
- **Assurez-vous que `SKILL.md` est bien à la racine** du dépôt (ou dans `skills/project-docs/`) — c'est le manifeste lu par l'agent au démarrage.
- **Incluez un dossier `templates/`** à côté du `SKILL.md` (il est consommé par `/create-docs` pour générer l'arborescence `docs/`).
- **Testez localement d'abord** avec `bunx skills add . --skill project-docs` avant de publier.
- *(Optionnel)* Ajoutez un badge en haut de votre README :
  ```markdown
  [![Skills](https://img.shields.io/badge/skills-published-blue)](https://skills.sh)
  ```

---

## Options utiles du CLI

| Flag | Rôle |
| --- | --- |
| `-g, --global` | Installe dans `~/` au lieu du projet |
| `-a, --agent <agents...>` | Cible un ou plusieurs agents (Claude Code, Cursor, OpenCode, etc.) |
| `-s, --skill <nom>` | Installe un skill précis (`'*'` pour tous) |
| `-l, --list` | Liste les skills disponibles sans rien installer |
| `--copy` | Copie les fichiers au lieu de les symlinker |
| `-y, --yes` | Mode non-interactif (CI/CD, scripts) |
| `--all` | Installe tous les skills sur tous les agents |

---

## Où le skill est installé

Selon l'agent utilisé, le skill atterrit dans un dossier de configuration à la racine de votre projet (ou dans `~/.config/`) :

```
.claude/skills/project-docs/     # Claude Code
.agents/skills/project-docs/     # OpenCode, agents.sh générique
.cursor/skills/project-docs/     # Cursor (selon configuration)
```

Vérification rapide après installation :

```bash
ls -la .claude/skills/project-docs/
# ou
ls -la .agents/skills/project-docs/
```

Vous devez voir au minimum `SKILL.md` (manifeste du skill), le dossier `commands/` (commandes séparées pour la détection automatique par les Code CLIs) et le dossier `templates/`.

---

## Utilisation — Commandes du skill

Une fois le skill installé, ces commandes sont disponibles dans votre agent :

| Commande | Description |
| --- | --- |
| `/create-docs` | Initialise l'arborescence `docs/` complète à partir d'une analyse du projet |
| `/read-docs` | Lit et résume la documentation existante |
| `/strict [tâche \| fichier \| on \| off]` | Applique les règles et conventions de `docs/skill/` à la lettre (tolérance zéro) |
| `/add-feature [nom] [us: ...] [ac: ...]` | Ajoute une fiche feature avec User Story et critères d'acceptation |
| `/validate-us [nom \| us: ...]` | Valide une User Story (INVEST), vérifie code/tests et fournit un résumé vulgarisé |
| `/sync-plan [nom]` | Scanne le code et coche automatiquement les tâches de l'Implementation Plan |
| `/add-rule [règle]` | Ajoute une règle ciblée dans `RULES.md` ou `CONVENTIONS.md` |
| `/update-rules` | Met à jour `RULES.md` / `CONVENTIONS.md` (analyse le code) |
| `/fix-feature [nom]` | Corrige ou enrichit une fiche feature existante |
| `/audit-docs` | Audite la couverture de la documentation vs le code |


### 💡 Détection automatique par les Code CLIs (Claude Code, OpenCode, etc.)

Chaque commande est **séparée dans son propre fichier Markdown** dans le dossier `commands/` (`commands/strict.md`, `commands/create-docs.md`, etc.).
Les Code CLIs (Claude Code, OpenCode, Cursor, etc.) détectent et indexent automatiquement ces fichiers pour les proposer dans leur menu d'auto-complétion `/` :
- **Claude Code** : détecte les commandes dans `commands/` (ou directement via `.claude/commands/`).
- **OpenCode** : détecte les commandes dans `commands/` ou `.opencode/commands/`.
---

## Exemples d'usage

### 1. Démarrer un projet from scratch

```bash
# 1) Installer le skill
bunx skills add Yaici-Yacine/project-docs --skill project-docs

# 2) Dans l'agent, générer l'arborescence
/create-docs
```

### 2. Documenter une nouvelle feature avec User Story et critères d'acceptation

```text
/add-feature user-authentication us: En tant qu'utilisateur, je veux me connecter avec email + mot de passe, afin d'accéder à mon tableau de bord. ac: Doit valider l'email, Doit refuser les mots de passe < 8 caractères, Doit émettre un JWT
```
### 3. Auditer la couverture de la doc

```text
/audit-docs
```

L'agent retourne un rapport listant les features non documentées, les pages obsolètes et les sections manquantes.

### 4. Suivre les règles à la lettre (`/strict`)

Exécuter une tâche avec respect absolu des règles :
```text
/strict refactorer le module d'authentification
```

Auditer les modifications en cours par rapport aux règles du projet :
```text
/strict
# ou cibler un fichier précis :
/strict src/features/auth/auth.service.ts
```

Activer le mode strict pour toute la session :
```text
/strict on
```

L'agent consulte immédiatement `docs/skill/RULES.md` et `docs/skill/CONVENTIONS.md`, bloque tout pattern interdit (`❌`), et conclut systématiquement chaque tâche par une checklist de conformité stricte.

### 5. Valider une User Story (`/validate-us`)

Valider la User Story d'une feature et vérifier sa couverture dans le code et les tests :
```text
/validate-us user-authentication
```

Valider une User Story à la volée (critères INVEST + génération de critères Gherkin) :
```text
/validate-us us: En tant qu'utilisateur, je veux réinitialiser mon mot de passe par email, afin de récupérer l'accès à mon compte.
```

Auditer la couverture globale des User Stories du projet :
```text
/validate-us
```

À la fin de chaque analyse, la commande génère un **résumé vulgarisé en langage clair** (Persona, besoin concret, valeur métier et synthèse en une phrase) pour comprendre immédiatement l'enjeu de la User Story.

### 6. Synchroniser le plan d'implémentation (`/sync-plan`)

Scanner le code et cocher automatiquement les tâches terminées dans `Implementation Plan` :
```text
/sync-plan user-authentication
# ou synchroniser l'ensemble des features :
/sync-plan all
```

---

## Mise à jour et suppression

### Mettre à jour le skill

```bash
# Si la sous-commande existe dans votre version du CLI
npx skills update project-docs

# Sinon, réinstaller (idempotent) :
npx skills add Yaici-Yacine/project-docs --skill project-docs
```

### Supprimer le skill

```bash
# Si la sous-commande existe
npx skills remove project-docs

# Sinon, suppression manuelle :
rm -rf .claude/skills/project-docs
# ou
rm -rf .agents/skills/project-docs
```

---

## Dépannage

- **`npx: command not found`** → Installez Node.js ≥ 18 depuis [nodejs.org](https://nodejs.org) puis réessayez.
- **`bunx: command not found`** → Installez Bun depuis [bun.sh](https://bun.sh) puis réessayez.
- **Le skill n'apparaît pas dans l'agent** → Relancez complètement l'agent (les skills sont chargés au démarrage).
- **Conflit de version** → Supprimez l'ancien dossier `.claude/skills/project-docs/` (ou `.agents/...`) puis réinstallez.
- **Erreur réseau / proxy** → Configurez le registre npm :
  ```bash
  npm config set registry https://registry.npmjs.org/
  ```
  Ou clonez manuellement votre dépôt et copiez `SKILL.md`, `commands/` et `templates/` dans `.claude/skills/project-docs/`.

---

## Structure du projet

```
project-docs/
├── README.md          ← ce fichier
├── SKILL.md           ← manifeste (lu par l'agent)
├── commands/          ← commandes séparées détectées par les Code CLIs
│   ├── create-docs.md
│   ├── read-docs.md
│   ├── strict.md
│   ├── add-feature.md
│   ├── validate-us.md
│   ├── sync-plan.md
│   ├── add-rule.md
│   ├── update-rules.md
│   ├── fix-feature.md
│   └── audit-docs.md
├── templates/         ← fichiers générés par /create-docs
│   ├── skill/
│   │   ├── RULES.md
│   │   ├── CONVENTIONS.md
│   │   └── DECISIONS.md
│   └── wiki/
│       ├── INDEX.md
│       └── features/
└── examples/          ← (optionnel) exemples d'arborescence générée
```

---

## 👤 Auteur

**`Yaici-Yacine`** — [GitHub](https://github.com/Yaici-Yacine)

## 📄 Licence

MIT (ou adaptez : Apache-2.0, GPL-3.0, etc.)

---

## Liens utiles

- 📚 Catalogue de skills : [https://skills.sh](https://skills.sh)
- 📦 Paquet npm du CLI : [https://www.npmjs.com/package/skills](https://www.npmjs.com/package/skills)
- 🛠️ Code source du CLI (Vercel Labs) : [https://github.com/vercel-labs/skills](https://github.com/vercel-labs/skills)
- 📖 Spec Agent Skills : [https://agentskills.sh](https://agentskills.sh)

---

✅ Une fois le skill installé, lancez `/create-docs` dans votre agent pour générer votre arborescence `docs/` automatiquement.
