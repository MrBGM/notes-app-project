# 📱 Guide : Créer l’APK de Notes App

Guide pas à pas pour générer l’APK Android (test ou release).

---

## ✅ Étape 0 : Prérequis

1. **Android Studio**  
   Installé et ouvert au moins une fois : [developer.android.com/studio](https://developer.android.com/studio)

2. **Java (JDK 17 ou +)**  
   Souvent fourni avec Android Studio. Vérifier :
   ```bash
   java -version
   ```

3. **Variable ANDROID_HOME (souvent déjà définie par Android Studio)**  
   - **Windows** : `C:\Users\VotreNom\AppData\Local\Android\Sdk`  
   - À ajouter dans *Variables d’environnement* si les commandes `sdkmanager` / Gradle échouent.

---

## 🚀 Étape 1 : Premier build (APK de test)

Ouvrez un terminal à la **racine du projet**, puis :

### 1.1 Aller dans le frontend

```bash
cd frontend
```

### 1.2 Build web en production

```bash
npm run build:prod
```

### 1.3 Ajouter Android (une seule fois)

Si le dossier `frontend/android` **n’existe pas** :

```bash
npx cap add android
```

### 1.4 Synchroniser le projet (web → Android)

```bash
npm run cap:sync
```

### 1.5 Générer l’APK debug

**Option A – Script npm (recommandé)**

```bash
npm run android:build
```

**Option B – À la main**

```bash
cd android
gradlew.bat assembleDebug
cd ..
```

> Sous Windows, utilisez `gradlew.bat`. Sous Mac/Linux : `./gradlew assembleDebug`.

L’APK est généré ici :

```
frontend/android/app/build/outputs/apk/debug/app-debug.apk
```

Vous pouvez l’installer sur un appareil ou un émulateur (glisser-déposer ou `adb install app-debug.apk`).

---

## 📲 Étape 2 : Tester l’APK sur un téléphone

1. Activer le **mode développeur** et le **débogage USB** sur le téléphone.  
2. Brancher le téléphone en USB.  
3. Copier `app-debug.apk` sur le téléphone et l’installer, ou utiliser :
   ```bash
   adb install frontend/android/app/build/outputs/apk/debug/app-debug.apk
   ```
4. **Important** : l’app appelle l’API définie dans `frontend/src/environments/environment.prod.ts`. Si l’IP de ton PC change (autre réseau, autre machine) ou si quelqu’un d’autre teste le projet, il faut mettre à jour cette IP puis reconstruire l’APK.  
   → **Voir le guide dédié : [GUIDE_TEST_APK.md](GUIDE_TEST_APK.md)** (configurer l’IP, rebuild, tester).

---

## 🔐 Étape 3 (optionnel) : APK release signé

Pour publier sur le Play Store ou distribuer un APK signé :

1. Créer un keystore (une fois) :
   ```bash
   keytool -genkey -v -keystore notes-app.keystore -alias notes-app -keyalg RSA -keysize 2048 -validity 10000
   ```
2. Configurer la signature dans le projet Android (fichier `key.properties` et `build.gradle`) comme décrit dans **ANDROID_SETUP.md**.
3. Lancer :
   ```bash
   npm run android:release
   ```
   L’APK signé se trouve dans :  
   `frontend/android/app/build/outputs/apk/release/app-release.apk`.

---

## 🐛 Dépannage

| Problème | Solution |
|----------|----------|
| `gradlew` introuvable ou erreur sous Windows | Utiliser `gradlew.bat` dans le dossier `frontend/android`. |
| Erreur SDK / ANDROID_HOME | Installer Android Studio, ouvrir SDK Manager, installer “Android SDK” et “Android SDK Build-Tools”, puis définir `ANDROID_HOME`. |
| L’app ne se connecte pas au backend | Vérifier `environment.prod.ts` : `apiUrl` doit pointer vers l’URL de votre API (IP ou domaine). |
| Dossier `android` absent | Exécuter `npx cap add android` dans `frontend`. |

Pour plus de détails (icônes, splash, nom d’app, package) : **ANDROID_SETUP.md**.
