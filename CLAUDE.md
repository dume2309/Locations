# Conventions du projet Locations

## 🚨 RÈGLE CRITIQUE — GitHub Pages NE DOIT JAMAIS ÊTRE DÉSACTIVÉ

L'appli Locations est servie par **GitHub Pages** depuis le repo `dume2309/Locations`.
Si GitHub Pages est désactivé, l'appli devient inaccessible (404) sur tous les appareils (iPad, iPhone, PC).

**Vérifications obligatoires :**
1. **Au début de chaque session de travail sur Locations** : vérifier que Pages est bien actif
   ```bash
   gh api repos/dume2309/Locations/pages | grep '"status"'
   ```
   Doit retourner `"built"` (ou `"building"` si déploiement en cours).
   Si erreur 404 ou `has_pages: False` → réactiver immédiatement :
   ```bash
   gh api -X POST repos/dume2309/Locations/pages -f source[branch]=master -f source[path]=/
   ```

2. **Ne JAMAIS** modifier les paramètres GitHub Pages depuis l'interface web sans raison majeure.

3. **Le repo est PUBLIC** par choix de l'utilisateur. Cela permet à GitHub Pages de fonctionner gratuitement.
   - Conséquence : `locations-data.json` et `locations-backup.txt` sont **publiquement consultables** sur github.com
   - C'est accepté par l'utilisateur (URLs obscures, faible risque)
   - Ne JAMAIS repasser en privé sans demander explicitement (Pages cesserait de fonctionner sur le plan gratuit)

## Versionnage de l'appli

- **Toujours incrémenter de 0.1** à chaque nouvelle version (V2.1 → V2.2 → V2.3 → V2.4...).
- **Jamais** utiliser de schéma type SemVer (V2.2.1, V2.0.0, etc.).
- Le numéro de version est affiché dans Réglages, en bas (`Locations vX.Y`).
- Vérifier la version actuelle dans `GestionLocations.html` avant de bumper.

## Sauvegarde avant chaque nouvelle version

À chaque bump de version :
1. **Copier** le fichier actuel `GestionLocations.html` en `GestionLocations-vX.Y.html` (la version qu'on quitte).
2. **Tag git** correspondant : `git tag vX.Y master`.
3. Commit la backup avec un message clair "Sauvegarde VX.Y figée (rollback en un clic)".
4. Mentionner dans le commit de la nouvelle version : "Rollback en un clic : renommer GestionLocations-vX.Y.html en GestionLocations.html".

## Architecture / fiabilité

- L'appli est une **PWA installée sur iPad** via Safari → Ajout à l'écran d'accueil.
- **Cache iOS Safari très agressif** : toujours utiliser des cache-busters (`?v=Date.now()`) pour les rechargements forcés. `location.reload(true)` est ignoré sur iOS.
- À chaque bump version : penser à bumper `CACHE` dans `sw.js` pour invalider le service worker.

## Sync GitHub (compat descendante)

- L'appli sync `locations-data.json` et `locations-backup.txt` automatiquement via `ghPush()` à chaque modification.
- **NE JAMAIS** faire dépendre une nouvelle feature de champs ajoutés dans `D` (l'objet sync) si une version précédente tourne encore sur un autre appareil — la version précédente n'écrasera pas ces champs au push.
- Pour les nouveaux états locaux (récap des changements, méta, etc.), utiliser une **clé localStorage séparée** (ex : `loc_meta_v1`), jamais incluse dans le push GitHub.

## Récap des changements (message Bea/Steph)

- Le récap en haut du message envoyé à Bea/Steph est focalisé sur l'**avenir**.
- Les modifications, ajouts ou suppressions de **réservations passées** (endDate < aujourd'hui) **NE DOIVENT JAMAIS** apparaître dans le récap. Elles n'intéressent pas Bea/Steph.
- L'utilisateur a déjà un toast "Modifié" / "Ajouté" / "Supprimé" en vert dans l'appli au moment de l'action — c'est sa confirmation visuelle, pas besoin de la dupliquer dans le récap envoyé.

## Branche de dev

- Branche dédiée pour les changements : `claude/add-change-summary-fg9eb` (ou autre suivant la fonctionnalité).
- Toujours merger dans `master` quand l'utilisateur valide → GitHub Pages sert master.
