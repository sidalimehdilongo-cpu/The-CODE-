# Compiler l'APK de test et l'AAB pour le Play Store

Rien à installer sur votre ordinateur : GitHub compile l'application dans le cloud (gratuit).

## A. Obtenir l'APK de test (à installer sur votre téléphone)

1. Créez un compte gratuit sur github.com, puis un **nouveau dépôt privé** (par exemple `code-route`).
2. Envoyez-y tout le contenu de ce dossier (bouton « Add file » > « Upload files » : glissez le contenu du dossier, y compris `.github`, `scripts`, `lib` et `assets`).
3. Ouvrez l'onglet **Actions**, choisissez **Build Android**, puis **Run workflow**.
   - `app_id` : un identifiant unique, au format `com.monnom.coderoute` (lettres minuscules et chiffres). Il ne pourra plus changer après publication.
   - `app_label` : le nom affiché sous l'icône.
4. Attendez 10 à 15 minutes. Ouvrez l'exécution terminée et téléchargez l'artefact **android-builds** (un zip).
5. Dans le zip, `app-release.apk` est l'APK de test. Copiez-le sur votre téléphone, ouvrez-le et autorisez l'installation depuis cette source.

L'APK de test simule l'achat : le bouton « Débloquer » ouvre les examens gratuitement, pour que vous puissiez les essayer. Les publicités sont des publicités de test Google tant que vous n'avez pas configuré AdMob.

## B. Préparer la publication sur le Play Store

### 1. Compte AdMob (publicités)
Créez l'application dans AdMob, puis un bloc **interstitiel**. Dans GitHub : Settings > Secrets and variables > Actions, ajoutez :
- `ADMOB_APP_ID` : l'identifiant de l'application (forme `ca-app-pub-xxx~yyy`)
- `ADMOB_INTERSTITIAL_ID` : l'identifiant du bloc (forme `ca-app-pub-xxx/yyy`)

### 2. Clé d'envoi (upload key)
Sur un ordinateur avec Java (`keytool`), lancez `bash scripts/make_upload_key.sh`. Gardez `upload-keystore.jks` et ses mots de passe en lieu sûr, avec une copie : sans cette clé, vous ne pourrez plus mettre l'application à jour sans passer par l'assistance Google. Ne la mettez pas dans le dépôt. Puis ajoutez ces secrets GitHub :
- `UPLOAD_KEYSTORE_BASE64` : le contenu de `upload-keystore.base64.txt`
- `UPLOAD_KEY_ALIAS` : `upload`
- `UPLOAD_STORE_PASSWORD` et `UPLOAD_KEY_PASSWORD` : vos mots de passe

### 3. Compiler l'AAB
Relancez **Build Android** avec votre vrai `app_id`. L'artefact contient alors `app-release-signed.aab` : c'est le fichier à envoyer à la Play Console. Il utilise le vrai achat Google Play (pas de simulation).

### 4. Play Console
1. Compte développeur Google Play (frais uniques de 25 $).
2. Créez l'application et choisissez la même valeur que `app_id`.
3. Produits intégrés : créez un **produit géré** (non consommable) dont l'identifiant est exactement `exam_unlock`, avec son prix. Il faut un profil de paiement pris en charge dans votre pays.
4. Fiche : description, icône 512×512, image de présentation 1024×500, captures d'écran.
5. Obligations : politique de confidentialité en ligne (gabarit : `PRIVACY_POLICY_TEMPLATE.md`), formulaire « Sécurité des données » (les publicités AdMob collectent des données), déclaration de publicités, classification du contenu.
6. Test fermé : les comptes développeur personnels récents doivent en général faire tester l'application par un nombre minimum de testeurs pendant une durée minimale avant la production. Vérifiez la règle actuelle dans la Play Console.
7. Envoyez l'AAB dans une version de test, puis en production.

### Avant chaque nouvelle version
Augmentez le numéro de version dans `pubspec.yaml` (`version: 0.1.0+2`) : la partie après `+` doit augmenter à chaque envoi.

## C. À savoir
- L'icône est celle de Flutter par défaut. Remplacez-la (package `flutter_launcher_icons`) avant de publier.
- Les identifiants publicitaires de test ne rapportent rien : configurez AdMob avant de publier.
- Je n'ai pas pu compiler ni tester l'application dans mon environnement. Le premier lancement du workflow peut révéler des erreurs de compilation à corriger ; envoyez-moi le message d'erreur du journal.
