# Journal Locations (production)

Suivi des évolutions de l'appli `GestionLocations.html` et de l'organisation des fichiers.

---

## Convention de sauvegarde des versions

À chaque bump de version (cf `CLAUDE.md` du projet) :

1. **Copier** `GestionLocations.html` actuel en `GestionLocations-vX.Y.html` (la version qu'on quitte)
2. **Ranger** ce fichier dans `Bkp/` (pas à la racine — la racine reste propre, seul le HTML "live" y reste)
3. **Tag git** : `git tag vX.Y master`
4. Commit la backup avec un message clair "Sauvegarde VX.Y figée (rollback en un clic)"

## Comment rollback à une version précédente

```bash
cd "c:/Users/Dumè/Dropbox/Steph/Locations"
cp "Bkp/GestionLocations-vX.Y.html" "GestionLocations.html"
# Bumper le CACHE dans sw.js pour forcer un refresh des PWA
git add GestionLocations.html sw.js
git commit -m "Rollback vers vX.Y"
git push
```

→ GitHub Pages redéploie en ~30-60s, puis sur chaque appareil → **Réglages → 🔄 Forcer la mise à jour**.

---

## 2026-04-26 — Rangement

- Déplacement de `GestionLocations-v2.1.html` et `GestionLocations-v2.2.html` de la **racine** vers **`Bkp/`**
- Aucun impact sur l'appli en production (seul `GestionLocations.html` à la racine est servi par GitHub Pages)
- L'historique git des 2 fichiers est préservé (`git mv`)
- Création de ce journal pour tracer l'organisation

## Backups disponibles dans `Bkp/`

| Fichier | Notes |
|---|---|
| `GestionLocations-v2.1.html` | Snapshot v2.1 figée (avant le récap des changements en haut du message Bea/Steph) |
| `GestionLocations-v2.2.html` | Snapshot v2.2 figée (avant le détail des changements + force update fiable iOS) |
| `GestionLocations_backup_v2.1.html` | Backup avec convention de nommage classique (cf historique session du 25/04) |
| `GestionLocations_backup_v1.0.html` à `v1.5-pre-reminder-fix.html` | Anciens backups historiques |

---

## 2026-04-25 (récap soirée)

Travail intense en soirée :
- v2.1 → v2.2 : récap des changements en haut du message envoyé à Bea/Steph
- v2.2 → v2.3 : détail des changements + bouton "Forcer la mise à jour" rendu fiable sur iOS
- Ajout de la convention "ne jamais afficher les modifs sur résas passées" dans CLAUDE.md du projet
- Ajout d'un rappel daté Dropbox 26/04 dans CLAUDE.md du projet (pour l'utilisateur)

Tous les commits sont sur `master`. Cache `sw.js` à v23 actuellement.
