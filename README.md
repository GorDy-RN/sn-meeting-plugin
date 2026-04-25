# Plugin Standard Notes — Notes de rencontre

Éditeur personnalisé pour prendre des notes de rencontre structurées dans Standard Notes.

## Fonctionnalités

- **Titre + date** de la rencontre en en-tête
- **Présences** : colle une liste brute (retour de ligne, virgule, point-virgule ou tab) → parsée en puces cliquables
- **Ordre du jour** : zone de texte libre
- **Notes** : liste dont chaque entrée peut être assignée à un participant
- **Tâches** : liste avec case à cocher, assigné, échéance, filtres (toutes / à faire / terminées)
- **Filtrage par personne** : clique sur une puce de participant pour ne voir que ses notes/tâches
- **Export Markdown** : génère un résumé prêt à coller dans un courriel ou Teams
- **Chiffré bout-en-bout** : les données sont stockées dans le contenu de la note, donc héritent du chiffrement SN
- **Thème adaptatif** : utilise les variables CSS de SN, s'adapte automatiquement à Default, Dark, Solarized, etc.

## Déploiement

Deux options. La plus simple est GitHub Pages.

### Option A — GitHub Pages (recommandé)

Le dépôt est déjà créé : <https://github.com/GorDy-RN/sn-meeting-plugin>

1. Pousser `index.html` et `ext.json` à la racine du dépôt (branche `main`).
   ```bash
   git clone https://github.com/GorDy-RN/sn-meeting-plugin.git
   cd sn-meeting-plugin
   # copier index.html et ext.json dans ce dossier
   git add index.html ext.json
   git commit -m "Initial plugin release"
   git push origin main
   ```
2. Sur GitHub : **Settings → Pages → Source = `main` branch, dossier = `/ (root)`**. Sauvegarder.
3. Attendre 1-2 minutes que le site soit publié. Vérifier en ouvrant <https://gordy-rn.github.io/sn-meeting-plugin/index.html> dans un navigateur — le plugin devrait apparaître (avec un message d'erreur de communication SN, c'est normal hors de l'app).
4. Le `ext.json` fourni contient déjà les bonnes URLs, aucune modification requise :
   ```json
   "url":          "https://gordy-rn.github.io/sn-meeting-plugin/index.html",
   "download_url": "https://github.com/GorDy-RN/sn-meeting-plugin/archive/refs/heads/main.zip",
   "latest_url":   "https://gordy-rn.github.io/sn-meeting-plugin/ext.json"
   ```
5. Dans Standard Notes : **Preferences → Plugins → Advanced → Import Extension** (ou « Import from URL »). Coller :
   ```
   https://gordy-rn.github.io/sn-meeting-plugin/ext.json
   ```
6. Activer le plugin. Dans une note, changer l'éditeur (menu sous le titre) pour « Notes de rencontre ».

### Mises à jour ultérieures

Quand tu modifies `index.html`, incrémente `"version"` dans `ext.json` (p. ex. `1.0.0` → `1.0.1`), commit et push. Standard Notes ping `latest_url` automatiquement et te proposera de mettre à jour.

### Option B — Local (pour tester avant de publier)

```bash
cd dossier-du-plugin
npx http-server --cors --port 8001
```

Dans `ext.json`, mettre `"url": "http://localhost:8001/index.html"`, puis importer `http://localhost:8001/ext.json` dans SN Desktop (fonctionne seulement sur le client de bureau, pas sur l'app web).

## Format de stockage

Le plugin stocke l'état sous forme de JSON dans le champ `content.text` de la note, donc :
- Si tu désinstalles le plugin, la note reste lisible en brut (JSON).
- Les backups SN/exports contiennent toutes les données.
- Aucune donnée n'est envoyée à un serveur tiers.

Structure :
```json
{
  "title": "Comité investissement — Q2",
  "date": "2026-04-21",
  "participantsRaw": "Alice\nBob\nCarole",
  "participants": ["Alice", "Bob", "Carole"],
  "agenda": "1. Revue CFADS\n2. Sizing dette",
  "notes": [{"id": "ab12", "text": "P99 DSCR trop serré", "assignee": "Bob"}],
  "tasks": [{"id": "cd34", "text": "Refaire sculpting", "assignee": "Alice", "due": "2026-04-28", "done": false}]
}
```

## Ergonomie

- **Entrée** dans un champ note ou tâche → crée une nouvelle ligne
- **Clic sur une puce participant** → filtre pour ne voir que ses items (re-clic pour enlever le filtre)
- **Case à cocher** → barre la tâche et la marque terminée
- **Compteurs** à droite de chaque section-tête (participants, notes, tâches terminées/total)

## Pistes d'extension

Si le plugin devient central à ton workflow, voici ce qui vaudrait la peine d'ajouter plus tard :
- Glisser-déposer pour réordonner les tâches/notes
- Lien d'une rencontre à la suivante (suivi d'action items récurrents)
- Commande `/` pour insérer rapidement un participant mentionné dans une note
- Bilingue : toggle FR / EN pour les libellés d'interface
- 
