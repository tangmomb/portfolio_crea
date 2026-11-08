# Portfolio de Tanguy M.

Ce dépôt contient le site du portfolio et les fichiers utilisés pour gérer son contenu.

## Mettre à jour le contenu

1. Modifier `update.xlsm` en conservant les quatre onglets et leurs en-têtes.
2. Fermer ou enregistrer le classeur.
3. Double-cliquer sur `mettre-a-jour.bat`.
4. Enregistrer puis envoyer les changements sur GitHub :

   ```powershell
   git add update.xlsm data
   git commit -m "Met à jour le contenu du portfolio"
   git push origin main
   ```

Le site https://tanguym.fr se met à jour tout seul dans les 10 minutes qui suivent (voir « Publication »).

Le générateur repose uniquement sur PowerShell, déjà fourni avec Windows. Il ignore les lignes vides formatées par Excel et produit :

- `data/all.json`, utilisé par le site ;
- `data/websites.json` ;
- `data/designs.json` ;
- `data/videos.json` ;
- `data/photos.json`.

## Ajouter une image

1. Copier l'image dans `images`.
2. Dans la colonne `image` du classeur, indiquer son nom (`mon-image.jpg`).
3. Relancer `mettre-a-jour.bat`.
4. Envoyer les changements sur GitHub :

   ```powershell
   git add update.xlsm images data
   git commit -m "Ajoute une image au portfolio"
   git push origin main
   ```

## Tester localement

Les navigateurs empêchent généralement un fichier HTML ouvert par double-clic de lire du JSON local. Double-cliquer sur `apercu.bat`, puis laisser la fenêtre ouverte pendant le test. Cette commande sert seulement à la prévisualisation locale.

## Publication

Depuis le 08/11/2026, le site est servi par Caddy sur le VPS Infomaniak (179.237.98.117), et non plus
par l'hébergement IONOS. Le VPS garde dans `/srv/portfolio` une copie partielle de ce dépôt, mise à jour
depuis `main` toutes les 10 minutes par le timer systemd `site-pull@portfolio.timer`. Pour forcer la
mise à jour : `sudo systemctl start site-pull@portfolio.service`.

Les fichiers de travail ne sont pas copiés sur le serveur : `update.xlsm`, les fichiers `.bat`, le dossier
`tools`, le README et la configuration GitHub restent dans le dépôt. Caddy refuse aussi tout chemin qui
commence par un point (`.git`…).

La configuration du serveur (Caddyfile et timer) se trouve dans `deploy/infomaniak/` du dépôt
`tangmomb/agent_mots_cles`.

L'ancien workflow [`.github/workflows/deploy-ionos.yml`](.github/workflows/deploy-ionos.yml), qui envoyait
le site par SFTP sur IONOS, est désactivé dans GitHub.

## Structure du classeur

- `website` : `title`, `description`, `link`, `technologies`, `image`
- `design` : `image`, `category`, `description`
- `video` : `url`, `category`, `description`, `platform` (`nom` est facultatif)
- `photo` : `image`, `category`, `description`
