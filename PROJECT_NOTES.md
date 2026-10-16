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
- **[Fait]** 50 collocations : cartes, liste, quiz (publiées en v10).
- **[Fait, non publié]** L'onglet Vocabulaire a trois sous-onglets : Thèmes (29 thèmes, 1 312 entrées), Expressions (le thème « Expressions », 100 entrées) et Collocations (50 fiches). Vue compacte disponible dans les listes. Voir section 6bis.
- Onglet Examen : entraînement en séries, examen blanc au format officiel, méthode de l'écrit.
- Fonctionnement hors ligne et installation sur l'écran d'accueil.

## 4. Fichiers importants
| Fichier | Rôle |
|---|---|
| `index.html` | Toute l'application |
| `sw.js` | Cache hors ligne. `VERSION` (actuellement `cahier-dalf-v11`, modifiée localement, pas encore publiée) est à incrémenter à chaque mise à jour |
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

## 6. Section Collocations (ajoutée, non commitée ni publiée)
**[Fait]** Modifications locales, vérifiées avec `git diff` :
- `index.html` (+329 lignes, 1 ligne modifiée) :
  - `DATA.colloc` : 50 fiches (39 C1, 11 C2). Champs : `id` (stable), `level`, `colloc`, `pos`, `reg`, `sens`, `ex`, `en`, `piege`, `cle` (partie de l'exemple masquée dans le quiz), `faux` (3 mauvaises réponses).
  - `renderColloc()` (cartes, liste) et `renderCQuiz()` (quiz), placées juste avant la section « Quiz ».
  - Une tuile « Collocations » sur l'accueil du Vocabulaire (`collocTile()`), et une ligne au début de `renderVocab()` qui redirige vers `renderColloc()` quand `state.themeV === "__colloc"`.
  - Une classe CSS `.trap` pour l'encadré « Piège ».
  - Une nouvelle clé `localStorage` : `knownColloc` (dictionnaire `id` → vrai/faux). Les 30 clés existantes ne sont pas modifiées.
- `sw.js` : `VERSION` passée de `cahier-dalf-v9` à `cahier-dalf-v10`. Aucun fichier ajouté, donc `SHELL` inchangée.
- Non modifié : `manifest.webmanifest`, `icons/`, `README.md`, `CLAUDE.md`.

**[Fait] Tests réalisés** (navigateur Chrome sans interface, sur une copie du site, servie par `python -m http.server`) :
- Les 4 onglets s'affichent sans erreur JavaScript.
- Tuile, cartes, retournement, « Je savais » / « À revoir », liste (50 fiches), quiz de 10 questions, retour, filtre C1/C2.
- Une progression `known` préexistante reste intacte après usage des collocations.
- Contrôle automatique des données : 50 identifiants uniques, l'exemple de chaque fiche contient exactement une fois la partie masquée, 3 mauvaises réponses par fiche.

**[Non vérifié]**
- Service worker, mise en cache et mode hors ligne : le test automatique n'a pas pu confirmer l'enregistrement du service worker. À tester à la main (voir section 7).
- Affichage visuel (mise en page, mode sombre, mobile) : aucune capture d'écran examinée.
- Aucun contrôle linguistique sur dictionnaire ou corpus.

## 6bis. Sous-onglets du Vocabulaire (modifications locales, non commitées)
**[Fait]** (`index.html`, `sw.js`) :
- Barre de sous-onglets « Thèmes | Expressions | Collocations » (même style que Structures). Les entrées du thème « Expressions » (100 : 68 C1, 32 C2) n'apparaissent plus dans « Thèmes » ; rien n'est dupliqué ni supprimé dans `DATA.vocab`.
- La tuile « Collocations » et le thème interne `"__colloc"` ont été retirés ; le bouton « ← Thèmes » n'existe plus pour Expressions et Collocations.
- Mode « Liste » : bascule fiches détaillées / vue compacte (lignes dépliables, comme Structures) dans les 3 sous-onglets.
- Nouvelles clés `localStorage` : `vsub` (dernier sous-onglet), `vcompact` (Thèmes et Expressions), `ccompact` (Collocations). Les clés `known` et `knownColloc` sont inchangées.
- Quiz : aucun quiz ajouté ni supprimé. Le quiz existant utilise le contenu du sous-onglet affiché.
- `sw.js` : `VERSION` = `cahier-dalf-v11`.

**[Fait] Tests** (Chrome sans interface, copie du site, progression préexistante injectée avant le chargement) : 4 onglets sans erreur, 3 sous-onglets, compteurs (1 312 / 100 / 50, soit 1 412 entrées de vocabulaire), mots acquis conservés (« étayer », « un cercle vicieux »), listes détaillées et compactes, filtre C2 (32 expressions), quiz des 3 sous-onglets.
**[Non vérifié]** Service worker / hors ligne, rendu visuel et mobile, mode sombre.

## 7. Points linguistiques à faire relire (incertains, rien n'a été corrigé)
Plusieurs « pièges » parlent de calques de l'anglais sans source. Certaines mauvaises réponses du quiz pourraient passer pour correctes :
- **Fiche 12, nouer des liens** : « construire des liens » est courant en français moderne, et c'est une mauvaise réponse du quiz.
- **Fiche 22, subir des pertes** : « souffert » et « pâti de » sont défendables ; « essuyer des pertes » existe aussi.
- **Fiche 24, lever un doute** : « enlever tout doute » s'entend couramment.
- **Fiche 31, suivre un cours** : « prendre des cours » est standard ; « prend » est une mauvaise réponse risquée.
- **Fiche 38, faire l'objet de** : « être l'objet de » est valide ; la mauvaise réponse « est le sujet » est discutable.
- **Fiche 30, réussir un examen** : « l'a brillamment passé » est maladroit mais pas faux ; la phrase est ambiguë.
- **Fiche 37, mettre en œuvre** : « exécutera la réforme » est possible.
- **Fiche 3, mener une enquête** : « conduire une enquête » est correct ; l'explication est trop forte.
- **Fiche 1, passer un appel** : « faire un appel » est courant au Québec.
- **Fiche 20, s'inquiéter de / pour** : la règle « de = situation, pour = personne » est simplifiée ; « s'inquiéter pour l'avenir » est très naturel.
- **Fiche 18, débat houleux** : « débat chaud » s'emploie ; « tiède » est une mauvaise réponse bizarre.
- **Fiche 29, porter plainte** : « déposer une plainte » existe avec article, contrairement à ce que dit la fiche.
- **Fiches 11, 16, 28, 35, 46, 49** : l'étiquette « calque » est discutable ou sans équivalent anglais précis (en particulier 49 : « to voice » n'appelle pas « mettre »).
- **Fiche 50, avoir recours à** : il n'y a pas vraiment de piège de traduction.
- **Fiche 41, poser les bases** : « mis » comme mauvaise réponse est discutable.

## 8. Prochaines étapes envisagées (suggestions, rien n'est décidé)
1. Relire les fiches signalées en section 7 et décider des corrections.
2. Tester à la main avec `python -m http.server` (http://localhost:8000) : ouvrir tous les onglets, faire un quiz de collocations, vérifier le mode hors ligne (outils du navigateur > Application > Service Workers > cocher « Offline »), et vérifier que `cahier-dalf-v10` apparaît dans le cache.
3. Décider si on enregistre les changements avec un commit, puis si on publie.
4. À chaque modification de l'app : incrémenter `VERSION` dans `sw.js`.
5. À plus long terme, étudier un découpage de `index.html` s'il devient trop lourd (après discussion).
