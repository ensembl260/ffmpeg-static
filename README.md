# ffmpeg-static

Archive de ffmpeg utilisée au build de l'API Ensembl' sur Heroku (buildpack `heroku-buildpack-ffmpeg-latest`, variable `FFMPEG_DOWNLOAD_URL`), pour la conversion et le contrôle des vidéos envoyées par les membres.

Elle remplace le téléchargement depuis johnvansickle.com, injoignable le 19/10/2026 : tant que ce site ne répondait pas, aucun build de l'API ne passait.

## Contenu

`ffmpeg-9.0-ensembl.tar.xz` (dans les releases) contient un dossier avec, à sa racine :

- `ffmpeg` et `ffprobe` (Linux x86_64, version `n9.0.2-28-g669f5ab083`), le buildpack les met dans le PATH ;
- `LICENSE.txt` (GPL v3).

Ce sont les binaires du build **ffmpeg-n9.0-latest-linux64-gpl-9.0** de [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds), récupéré le 19/10/2026, sans modification : seuls ffplay, la documentation et les presets ont été retirés, et les deux programmes placés à la racine de l'archive.

- SHA-256 de l'archive BtbN d'origine : `b15fe284b9f59b17149ac20517c8de2ae04e60f0a0ff8903d3704d35698ca6e0`
- SHA-256 de l'archive de ce dépôt : `bfa95c98ba3a327e629e3a61bffcad598e4f0ac78ba05202f4feea924b972e68`

Ils tournent sur Heroku-24 (glibc 2.28 minimum) et incluent notamment libx264 (conversion en H.264) et libzimg (filtre zscale, vidéos HDR d'iPhone).

## Licence et sources

Ces binaires sont distribués sous licence GPL v3 (voir `LICENSE.txt`). Les sources correspondantes :

- FFmpeg 9.0 : https://ffmpeg.org/download.html et https://git.ffmpeg.org/ffmpeg.git (commit `669f5ab083` de la branche `release/9.0`) ;
- scripts de build et versions exactes de chaque bibliothèque incluse : https://github.com/BtbN/FFmpeg-Builds.

## Mettre à jour

1. Télécharger un build `ffmpeg-nX.Y-latest-linux64-gpl-X.Y.tar.xz` de BtbN.
2. Recréer l'archive avec `ffmpeg`, `ffprobe` et `LICENSE.txt` dans un dossier à sa racine (le buildpack l'extrait avec `--strip-components=1`).
3. La publier dans une nouvelle release, mettre à jour ce README, puis `FFMPEG_DOWNLOAD_URL` sur l'app API (dev d'abord).
