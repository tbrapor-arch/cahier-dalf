# Notes du projet : Cahier DALF C1·C2

> Légende : **[Fait]** = vérifié dans les fichiers ou pendant la session. **[Suggestion]** = idée, rien n'est décidé.

## 1. Objectif et état actuel
- **[Fait]** Le projet est une application web pour préparer le DALF C1 et C2 (examen de français niveau avancé).
- **[Fait]** L'application est déjà publiée sur GitHub Pages : https://tbrapor-arch.github.io/cahier-dalf/
- **[Fait]** Branche `main`. Au début de la session, aucun changement n'était en attente (dépôt propre).
- **[Fait]** Derniers commits : onglets et vue compacte pour les structures, onglet Examen (séries, examen blanc, méthode), nouvelle icône.

## 2. Architecture
- **[Fait]** Application statique : pas de serveur, pas de framework, pas de build, pas de `package.json`.
- **[Fait]** `index.html` (environ 580 Ko) contient tout : contenu, CSS et JavaScript.
- **[Fait]** C'est une PWA (application installable) : `manifest.webmanifest`, `icons/` et `sw.js` (service worker, pour le hors ligne).
- **[Fait]** La progression est enregistrée dans `localStorage`, sur l'appareil (pas de compte).
- **[Fait]** Les polices viennent de Google Fonts ; des liens externes mènent vers des ressources officielles et audio.

## 3. Fonctionnalités principales
- 203 structures clés (onglets, vue compacte, FR/EN).
- 1 410 mots et expressions en 30 thèmes.
- 14 textes d'écoute avec transcription synchronisée.
- Quiz.
- Onglet Examen : entraînement en séries, examen blanc au format officiel, méthode de l'écrit.
- Fonctionnement hors ligne et installation sur l'écran d'accueil.

## 4. Fichiers importants
| Fichier | Rôle |
|---|---|
| `index.html` | Toute l'application |
| `sw.js` | Cache hors ligne. `VERSION` (actuellement `cahier-dalf-v9`) est à incrémenter à chaque mise à jour |
| `manifest.webmanifest` | Informations d'installation |
| `icons/` | Icônes de l'application |
| `README.md` | Présentation courte du projet |
| `CLAUDE.md` | Consignes pour Claude (architecture, précautions, ton à adopter) |
| `PROJECT_NOTES.md` | Ce fichier : suivi du travail |

## 5. Ce qui a été fait dans cette session
**Inspecté (lecture seule) :**
- La liste des fichiers du dépôt.
- `README.md` et `sw.js`, en entier.
- Quelques passages ciblés de `index.html` (titre, balises `script`, polices, `localStorage`, liens externes). Le fichier n'a **pas** été lu en entier.

**Réellement créé :**
- `CLAUDE.md` (contenu validé par toi avant écriture).
- `PROJECT_NOTES.md` (ce fichier).

**Non modifié :** `index.html`, `sw.js`, `manifest.webmanifest`, `icons/`, `README.md`.

**Non fait :** aucune installation, aucun commit, aucun déploiement, aucune publication GitHub. Les deux nouveaux fichiers sont donc seulement présents en local, non enregistrés dans Git.

## 6. Prochaines étapes envisagées (suggestions, rien n'est décidé)
1. Relire `CLAUDE.md` et `PROJECT_NOTES.md`, puis décider si tu veux les enregistrer avec un commit.
2. Tester l'app en local avec un petit serveur HTTP (par exemple `python -m http.server`), pour vérifier chaque onglet.
3. Décider du prochain contenu ou de la prochaine amélioration à ajouter (nouveaux thèmes, textes d'écoute, exercices).
4. À chaque modification de l'app : incrémenter `VERSION` dans `sw.js` pour que les téléphones reçoivent la mise à jour.
5. À plus long terme, si `index.html` devient trop lourd à maintenir, étudier un découpage en plusieurs fichiers (à n'envisager qu'après discussion).
