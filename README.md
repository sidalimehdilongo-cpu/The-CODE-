# Code de la route / Driving theory — application Flutter multi-pays

Choix du pays au premier lancement, entraînement gratuit (20 questions tirées au hasard, une publicité toutes les 3 questions) et examen blanc payant dont le format suit celui du pays choisi.

**Pour compiler l'APK de test et l'AAB du Play Store : lisez `BUILD.md`.**

## Pays inclus

| Pays | Langue | Banque | Examen blanc (format retenu) |
|---|---|---|---|
| France | fr | 200 questions (103 illustrées) | 40 questions, 35 bonnes réponses minimum |
| Royaume-Uni | en | 50 (35 illustrées) | 50 questions, 43 minimum, 57 min |
| Espagne | es | 50 (40 illustrées) | 30 questions, 3 erreurs max, 30 min |
| Italie | it | 50 vrai/faux (34 illustrées) | 30 affirmations, 3 erreurs max, 20 min |
| États-Unis · Californie | en | 50 règles générales (30 illustrées) | 46 questions, 38 minimum |
| États-Unis · Texas | en | (même banque) | 30 questions, 21 minimum |
| États-Unis · Floride | en | (même banque) | 50 questions, 40 minimum |
| Japon | ja | 50 ○× (25 illustrées) | 50 questions, 45 minimum, 30 min |

Ces formats viennent de mes connaissances générales : **à vérifier auprès des autorités de chaque pays**. Aux États-Unis, les règles varient selon l'État : la banque contient des règles générales, à compléter par État. Les épreuves vidéo (perception des dangers au Royaume-Uni) et les épreuves pratiques ne sont pas incluses. Au Japon, le panneau « 止まれ » n'est pas dessiné (le package d'affichage SVG ne gère pas le texte).

Quand la banque est plus petite que l'examen officiel, l'examen blanc utilise toutes les questions disponibles et le seuil est ramené proportionnellement. Visez plusieurs centaines de questions par pays pour des examens variés.

## Contenu du dossier
- `lib/main.dart` : l'application (pays, langues, quiz, examens, publicités, achat).
- `assets/countries.json` : pays, langues et règles d'examen. `assets/questions/` : banques de questions. `assets/images/` : 87 illustrations SVG.
- `scripts/`, `.github/workflows/` : compilation APK/AAB dans le cloud. `BUILD.md` : mode d'emploi.
- `PRIVACY_POLICY_TEMPLATE.md` : gabarit de politique de confidentialité.

## Ajouter un pays ou une langue
1. Créer `assets/questions/<code>.json` (champs `id`, `theme`, `text`, `choices`, `correct`, `explanation`, `image`). Les questions à 2 choix (vrai/faux, ○×) ne sont pas mélangées.
2. Ajouter une entrée dans `assets/countries.json` : `code`, `name`, `flag`, `language`, `file`, `driveSide`, et `exam` : `questions`, puis `passMark` (bonnes réponses requises) ou `maxErrors` (erreurs maximum), et `minutes` (ou `null`).
3. Si la langue est nouvelle, ajouter ses textes dans la table `_tr` de `lib/main.dart`.
4. Les illustrations sont des SVG sans texte. Les panneaux qui suivent la convention de Vienne se réutilisent facilement ; les autres demandent de nouveaux dessins.

## Avant publication
- Faire relire toutes les questions par une personne qui connaît les règles du pays. Elles sont originales mais écrites sans source officielle. Ne copiez pas les banques officielles.
- Configurer AdMob et Google Play Billing (voir `BUILD.md`).
