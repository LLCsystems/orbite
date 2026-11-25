# Orbite pour Android (Google Play)

Ce dossier emballe le jeu (`../index.html`) dans une app Android avec [Capacitor](https://capacitorjs.com/). Le jeu tourne hors ligne, sans barre d'adresse.

Rien n'est à installer sur l'ordinateur : GitHub construit l'app à chaque modification de `index.html` ou de ce dossier (workflow **Construire l'app Android**, onglet *Actions* du dépôt).

## Ce que produit le workflow

| Fichier | À quoi il sert |
| --- | --- |
| `orbite-android-test.apk` | S'installe directement sur un téléphone Android pour essayer le jeu. |
| `orbite-google-play.aab` | Le fichier à envoyer dans la Play Console. Produit seulement quand la clé d'envoi est dans les secrets. |

Les fichiers se téléchargent en bas de la page d'une exécution, rubrique *Artifacts*.

## Clé d'envoi (à faire une fois)

Google Play exige que le `.aab` soit signé avec une « clé d'envoi ». Ajoutez ces 4 secrets dans *Settings → Secrets and variables → Actions → New repository secret* :

| Nom du secret | Contenu |
| --- | --- |
| `ANDROID_KEYSTORE_BASE64` | Le fichier de clé (`.jks`) encodé en base64 |
| `ANDROID_KEYSTORE_PASSWORD` | Le mot de passe du fichier de clé |
| `ANDROID_KEY_ALIAS` | Le nom de la clé (ici `orbite`) |
| `ANDROID_KEY_PASSWORD` | Le mot de passe de la clé |

Gardez une copie du fichier `.jks` et des mots de passe en lieu sûr (gestionnaire de mots de passe par exemple). Avec la signature d'apps Google Play activée, une clé d'envoi perdue peut être remplacée en contactant Google, mais c'est plus simple de ne pas la perdre.

Pour créer une clé vous-même (Java requis) :

```sh
keytool -genkeypair -v -keystore orbite-upload.jks -alias orbite -keyalg RSA -keysize 2048 -validity 10000
base64 -i orbite-upload.jks | pbcopy   # macOS : copie le base64 dans le presse-papiers
```

## Réglages

- Identifiant de l'app : `com.llcsystems.orbite` (définitif une fois publié sur Google Play).
- Numéro de version : celui de l'exécution du workflow, il augmente tout seul.
- Écran en mode portrait.
- Icône et écran de démarrage : sources dans `assets/`, régénérées avec `npx @capacitor/assets generate --android`.
