# CLAUDE.md

## Présentation
Cahier DALF C1·C2 : application web pour préparer le DALF (français niveau avancé).
Elle est publiée sur GitHub Pages : https://tbrapor-arch.github.io/cahier-dalf/

## Architecture
- Application **statique** : pas de serveur, pas de framework, pas de build, pas de package.json.
- `index.html` contient TOUTE l'application : contenu pédagogique, CSS et JavaScript, intégrés dans le même fichier (environ 580 Ko).
- C'est une **PWA** (application installable) :
  - `manifest.webmanifest` et `icons/` : installation sur l'écran d'accueil.
  - `sw.js` : service worker qui permet l'usage hors ligne. Il utilise « réseau d'abord, cache en secours » pour les fichiers de l'app, et « cache d'abord » pour les polices Google.
- La progression de l'utilisateur est enregistrée dans `localStorage` (sur l'appareil, sans compte).
- Les polices viennent de Google Fonts, et il y a des liens externes (France Éducation international, TV5Monde, etc.).

## Fonctionnalités à préserver
- Les 203 structures clés : modes en onglets, vue compacte, bascule FR/EN.
- Le vocabulaire : 1 410 mots et expressions en 30 thèmes.
- Les 14 textes d'écoute avec transcription synchronisée, et la pause à l'écoute.
- Les quiz.
- L'onglet Examen : entraînement en séries, examen blanc au format officiel, méthode de l'écrit.
- La sauvegarde de la progression (`localStorage`). Ne pas changer les noms des clés de stockage sans prévoir une migration, sinon les utilisateurs perdent leurs données.
- Le fonctionnement hors ligne et l'installation sur l'écran d'accueil.
- Les exercices sont originaux, au format du sujet de démonstration officiel. Ne pas copier de sujets officiels.

## Précautions lors des modifications
- Faire des modifications ciblées et petites dans `index.html`. Ne pas le réécrire en entier, et ne pas le découper en plusieurs fichiers sans demande explicite.
- Après toute modification de l'app (`index.html`, `manifest.webmanifest`, icônes), incrémenter `VERSION` dans `sw.js` (par exemple `cahier-dalf-v9` devient `cahier-dalf-v10`). Sinon, les téléphones gardent l'ancienne version en cache.
- Si on ajoute un nouveau fichier nécessaire hors ligne, l'ajouter à la liste `SHELL` de `sw.js`.
- Tester en local via un serveur HTTP (par exemple `python -m http.server`). Le service worker ne fonctionne pas en ouvrant le fichier directement (`file://`).
- Ne jamais lancer d'installation, de déploiement ni de publication GitHub (commit, push) sans demande explicite.
- Garder le contenu en français, avec la typographie et les accents corrects.
- Il n'y a pas de tests automatisés : vérifier manuellement chaque onglet touché (structures, vocabulaire, écoute, quiz, examen).

## Préférences de l'utilisateur
- L'utilisateur débute en programmation.
- Expliquer chaque changement simplement, en français, sans jargon inutile. Dire ce qui a été changé, pourquoi, et comment le vérifier.
- Présenter les changements importants avant de les appliquer.
