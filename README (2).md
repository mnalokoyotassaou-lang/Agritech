# 🌿 AgriTech — Détection IA des Maladies des Plantes

> Application mobile intelligente de détection et gestion des maladies des plantes, conçue pour les agriculteurs d'Afrique de l'Ouest.

[![Flutter](https://img.shields.io/badge/Flutter-3.x-blue?logo=flutter)](https://flutter.dev)
[![FastAPI](https://img.shields.io/badge/FastAPI-Python-green?logo=fastapi)](https://fastapi.tiangolo.com)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-MobileNetV2-orange?logo=tensorflow)](https://tensorflow.org)
[![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203.3%2070B-purple)](https://groq.com)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

---

## 📋 Table des matières

- [Aperçu du projet](#-aperçu-du-projet)
- [Fonctionnalités](#-fonctionnalités)
- [Architecture](#-architecture)
- [Prérequis](#-prérequis)
- [Installation Backend](#-installation-backend-fastapi)
- [Installation Frontend](#-installation-frontend-flutter)
- [Configuration Firebase](#-configuration-firebase)
- [Configuration des clés API](#-configuration-des-clés-api)
- [Lancement](#-lancement)
- [Structure du projet](#-structure-du-projet)
- [Équipe](#-équipe)

---

## 🌍 Aperçu du projet

AgriTech permet à un agriculteur de photographier une feuille malade et d'obtenir en quelques secondes :
- Le **diagnostic IA** de la maladie (15 classes, 4 cultures)
- Des **conseils de traitement** générés par LLM
- La **lecture vocale** des conseils dans sa langue
- Une **carte communautaire** des épidémies proches

**Langues supportées :** Français · Anglais · Haoussa · Zarma · Arabe · Fulfulde · Tamasheq

---

## ✅ Fonctionnalités

| Fonctionnalité | Statut |
|---|---|
| Détection maladie par photo (caméra / galerie) | ✅ |
| Diagnostic multi-photos (1–5 angles) + stade 1/2/3 | ✅ |
| Conseils IA en 7 langues (Groq LLaMA 3.3 70B) | ✅ |
| Lecture vocale (flutter_tts + espeak-ng) | ✅ |
| Carte des maladies (OpenStreetMap) + alertes épidémies | ✅ |
| Météo locale + alertes risques fongiques (Open-Meteo) | ✅ |
| Tableau de bord statistiques (Syncfusion) | ✅ |
| Historique des analyses (SQLite) | ✅ |
| QR code parcelle (génération + scan) | ✅ |
| Chatbot agricole conversationnel (vocal + texte) | ✅ |
| Forum communautaire temps réel (Firebase) | ✅ |
| Pharmacie agricole locale | ✅ |
| Réseau d'experts agronomes | ✅ |
| Mode hors-ligne | ✅ |

---

## 🏗️ Architecture

```
AgriTech/
├── Frontend  →  Flutter (Dart)         — Android / Web
├── Backend   →  FastAPI (Python)        — API REST + IA
├── Modèle    →  MobileNetV2 (TF/Keras) — 15 classes
├── LLM       →  Groq (LLaMA 3.3 70B)  — Conseils multilingues
├── Cloud     →  Firebase               — Auth + Firestore + Storage
└── Carte     →  OpenStreetMap          — Géolocalisation
```

---

## 🔧 Prérequis

### Backend
- Python **3.9+**
- pip
- (Optionnel) GPU CUDA pour TensorFlow — fonctionne aussi sur CPU

### Frontend
- Flutter **3.x** — [Installer Flutter](https://docs.flutter.dev/get-started/install)
- Dart SDK (inclus avec Flutter)
- Android SDK (API 21+)
- Un appareil Android ou un émulateur

### Comptes requis
- [Groq](https://console.groq.com) — clé API gratuite
- [Firebase](https://console.firebase.google.com) — projet gratuit (Spark Plan)

---

## 🐍 Installation Backend (FastAPI)

### 1. Cloner le dépôt

```bash
git clone https://github.com/votre-compte/agritech.git
cd agritech/backend
```

### 2. Créer un environnement virtuel

```bash
# Linux / macOS
python3 -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate
```

### 3. Installer les dépendances

```bash
pip install fastapi uvicorn tensorflow opencv-python numpy python-multipart
```

### 4. Placer le modèle

Déposez vos fichiers dans le dossier `backend/models/` :

```
backend/
└── models/
    ├── mobilenetv2_color.h5   ← modèle entraîné (télécharger ci-dessous)
    └── classes_color.txt      ← liste des 15 classes (inclus dans le dépôt)
```

> **Télécharger le modèle :** Le fichier `.h5` n'est pas inclus dans le dépôt (trop lourd).
> Téléchargez-le depuis : [Google Drive / Lien à ajouter]
> Ou ré-entraînez-le avec `train.py` si vous avez le dataset PlantVillage.

### 5. Lancer l'API

```bash
cd backend
uvicorn app_api:app --host 0.0.0.0 --port 8000 --reload
```

L'API est disponible sur : `http://localhost:8000`
Documentation interactive : `http://localhost:8000/docs`

### 6. Vérifier que l'API fonctionne

```bash
curl http://localhost:8000/health
```

Réponse attendue :
```json
{
  "status": "ok",
  "modele_charge": true,
  "nombre_classes": 15
}
```

---

## 📱 Installation Frontend (Flutter)

### 1. Aller dans le dossier Flutter

```bash
cd agritech   # racine du projet Flutter
```

### 2. Installer les dépendances Flutter

```bash
flutter pub get
```

### 3. Configurer l'adresse IP du serveur

Dans `lib/main.dart`, remplacez l'IP par celle de votre machine :

```dart
static const String _baseIP = "10.10.31.62";  // ← Remplacez par votre IP locale
```

Pour trouver votre IP locale :
```bash
# Linux / macOS
ip a | grep "inet " | grep -v 127

# Windows
ipconfig | findstr "IPv4"
```

> ⚠️ Le téléphone et le PC doivent être sur le **même réseau Wi-Fi**.

### 4. Configurer la clé Groq

Dans `lib/main.dart` :
```dart
static const String _groqApiKey = "VOTRE_CLE_GROQ_ICI";
```

Obtenez votre clé gratuite sur : https://console.groq.com/keys

### 5. Lancer sur Android

```bash
# Vérifier les appareils connectés
flutter devices

# Lancer sur l'appareil Android
flutter run

# Lancer sur Chrome (Web)
flutter run -d chrome

# Lancer sur Linux Desktop
flutter run -d linux
```

---

## 🔥 Configuration Firebase

### 1. Créer un projet Firebase

1. Allez sur [console.firebase.google.com](https://console.firebase.google.com)
2. Créez un projet nommé `agritech`
3. Activez les services suivants :
   - **Authentication** → Email/Mot de passe
   - **Firestore Database** → Mode test
   - **Storage** → Mode test

### 2. Configurer FlutterFire

```bash
# Installer FlutterFire CLI
dart pub global activate flutterfire_cli

# Configurer (remplacez par votre projet-id)
flutterfire configure --project=VOTRE-PROJET-ID
```

Cela génère automatiquement `lib/firebase_options.dart`.

### 3. Règles Firestore recommandées (développement)

Dans Firebase Console → Firestore → Règles :

```
rules_version = '2';
service cloud.firestore.beta {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## 🔑 Configuration des clés API

| Service | Où configurer | Comment obtenir |
|---|---|---|
| **Groq** (LLM) | `lib/main.dart` → `_groqApiKey` | [console.groq.com](https://console.groq.com/keys) — Gratuit |
| **Firebase** | `lib/firebase_options.dart` | Généré par `flutterfire configure` |
| **Open-Meteo** | Aucune clé requise | API publique gratuite ✅ |
| **OpenStreetMap** | Aucune clé requise | API publique gratuite ✅ |

---

## 🚀 Lancement

### Démarrage complet (Backend + Frontend)

**Terminal 1 — Backend :**
```bash
cd agritech/backend
source venv/bin/activate   # Linux/macOS
uvicorn app_api:app --host 0.0.0.0 --port 8000 --reload
```

**Terminal 2 — Frontend :**
```bash
cd agritech
flutter run
```

### TTS sur Linux (optionnel)

Si vous développez sur Linux et souhaitez la synthèse vocale :

```bash
sudo apt-get install espeak-ng
```

---

## 📁 Structure du projet

```
agritech/
│
├── lib/                          # Code Flutter
│   ├── main.dart                 # Page principale (détection)
│   ├── database_helper.dart      # SQLite — modèle Analyse
│   ├── historique_page.dart      # Historique des analyses
│   ├── carte_maladies_page.dart  # Carte OpenStreetMap
│   ├── meteo_service.dart        # Service Open-Meteo
│   ├── meteo_page.dart           # Page météo + alertes
│   ├── widget_meteo_resume.dart  # Widget résumé météo (accueil)
│   ├── dashboard_page.dart       # Tableau de bord statistiques
│   ├── chatbot_page.dart         # Chatbot agricole (Groq + voix)
│   ├── parcelle_helper.dart      # Modèle Parcelle + SQLite
│   ├── parcelles_page.dart       # Liste parcelles + QR code
│   ├── parcelle_detail_page.dart # Fiche détail parcelle
│   ├── auth_service.dart         # Authentification Firebase
│   ├── login_page.dart           # Page connexion / inscription
│   ├── forum_page.dart           # Forum communautaire
│   ├── forum_service.dart        # Service Firebase forum
│   ├── pharmacie_page.dart       # Pharmacie agricole
│   ├── multi_photo_page.dart     # Diagnostic multi-photos
│   └── firebase_options.dart     # Config Firebase (généré)
│
├── backend/                      # Code Python
│   ├── app_api.py                # API FastAPI principale
│   ├── train.py                  # Entraînement MobileNetV2
│   ├── predict.py                # Script prédiction CLI
│   └── models/
│       ├── mobilenetv2_color.h5  # Modèle entraîné (à télécharger)
│       └── classes_color.txt     # 15 classes en français
│
├── android/                      # Config Android
├── pubspec.yaml                  # Dépendances Flutter
└── README.md
```

---

## 📦 Dépendances principales

### Flutter (pubspec.yaml)

```yaml
dependencies:
  http: ^1.0.0
  image_picker: ^1.0.0
  flutter_tts: ^4.0.2
  flutter_markdown: ^0.6.18
  sqflite: ^2.3.2
  sqflite_common_ffi: ^2.3.2
  path: ^1.9.0
  geolocator: ^11.0.0
  flutter_map: ^6.1.0
  latlong2: ^0.9.0
  syncfusion_flutter_charts: ^24.2.9
  syncfusion_flutter_gauges: ^24.2.9
  qr_flutter: ^4.1.0
  mobile_scanner: ^5.2.3
  speech_to_text: ^7.0.0
  firebase_core: ^3.3.0
  firebase_auth: ^5.1.0
  cloud_firestore: ^5.2.0
  firebase_storage: ^12.1.0
  record: ^5.1.2
  audioplayers: ^6.1.0
  path_provider: ^2.1.2
```

### Python (backend)

```bash
pip install fastapi uvicorn tensorflow opencv-python numpy python-multipart
```

---

## 👥 Équipe

| Membre | Rôle |
|---|---|
| **Nalokoyo Tassaou Mahamadou Massour** | Développeur Flutter & Intégration IA |
| **Abdoul Aziz Dangade** | Développeur Backend & Modèles IA |

---

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

<div align="center">
  <strong>🌿 AgriTech — Innovation Africaine au Service de la Terre 🌍</strong>
</div>
