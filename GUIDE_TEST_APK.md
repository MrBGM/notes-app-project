# 📱 Guide : Tester l’APK sur un téléphone Android

Ce guide explique comment lancer l’APK et le connecter au backend sur ton PC. **L’IP du PC peut changer** (autre réseau, autre machine, quelqu’un d’autre qui récupère le projet) : il faut alors la mettre à jour une fois, puis reconstruire l’APK.

---

## 1. Lancer le backend sur le PC

Sur le PC qui héberge l’API :

```bash
cd backend
npm run dev
```

Le serveur doit afficher par exemple : `Server running on port 3000`.  
Laisse ce terminal ouvert tant que tu testes l’APK.

---

## 2. Mettre à jour l’IP du PC (si besoin)

L’APK appelle le backend à l’adresse définie dans le projet. **Si tu changes de PC, de réseau ou de machine, il faut mettre la bonne IP.**

### 2.1 Trouver l’IP du PC

**Sous Windows (PowerShell ou CMD) :**

```bash
ipconfig
```

Repère l’**Adresse IPv4** de la carte **Wi-Fi** (souvent `192.168.x.x` ou `172.x.x.x`).  
Exemple : `192.168.1.25`.

**Sous macOS / Linux :**

```bash
# Souvent
ipconfig getifaddr en0
# ou
hostname -I
```

### 2.2 Mettre l’IP dans le projet

Ouvre le fichier :

```
frontend/src/environments/environment.prod.ts
```

Modifie la ligne `apiUrl` avec **l’IP de ton PC** et le port `3000` :

```ts
export const environment = {
  production: true,
  apiUrl: 'http://TON_IP_ICI:3000/api',
};
```

Exemple si ton IP est `192.168.1.25` :

```ts
apiUrl: 'http://192.168.1.25:3000/api',
```

Sauvegarde le fichier.

### 2.3 Reconstruire l’APK après changement d’IP

Dès que tu changes `environment.prod.ts`, il faut **reconstruire l’APK** pour que la nouvelle URL soit prise en compte :

```bash
cd frontend
npm run android:build
```

Le nouvel APK se trouve ici :

```
frontend/android/app/build/outputs/apk/debug/app-debug.apk
```

Réinstalle cet APK sur le téléphone (remplace l’ancienne version).

---

## 3. Tester l’APK sur le téléphone

1. **Réseau** : le téléphone et le PC doivent être sur le **même Wi‑Fi**.
2. **Backend** : le backend tourne sur le PC (`npm run dev` dans `backend`).
3. **APK** : installe (ou réinstalle) `app-debug.apk` sur le téléphone.
4. Ouvre l’app : connexion, inscription et appels API doivent fonctionner.

---

## 4. En résumé (checklist)

| Étape | Action |
|-------|--------|
| 1 | Lancer le backend : `cd backend` puis `npm run dev` |
| 2 | Vérifier l’IP du PC : `ipconfig` (Windows) |
| 3 | Mettre à jour `frontend/src/environments/environment.prod.ts` avec cette IP |
| 4 | Reconstruire l’APK : `cd frontend` puis `npm run android:build` |
| 5 | Installer (ou réinstaller) `app-debug.apk` sur le téléphone |
| 6 | Téléphone et PC sur le même Wi‑Fi → tester l’app |

---

## 5. Quelqu’un d’autre récupère le projet

Si une autre personne clone le projet pour tester :

1. Elle suit les prérequis (Node, Android Studio, etc.) et le guide **BUILD_APK.md** pour générer l’APK une première fois.
2. Elle utilise **ce guide** : elle met **sa propre IP** dans `environment.prod.ts`, refait `npm run android:build`, puis installe l’APK sur son téléphone et lance le backend sur son PC.

Aucune modification du code n’est nécessaire à part l’IP dans `environment.prod.ts` et un nouveau build d’APK.
