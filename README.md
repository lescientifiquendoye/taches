# Flutter Notes – Gestion de Notes (SQLite)

Application Flutter complète pour gérer des notes en local avec SQLite (sqflite).

Identifiants par défaut (login local):
- Utilisateur: admin
- Mot de passe: admin123

## Sommaire
- Présentation et fonctionnalités
- Architecture du projet
- Installation et exécution
- Base de données SQLite (schéma et accès)
- Navigation et UX/UI
- Tests
- Wireframes
- Dépannage (Gradle/NDK, réseau, etc.)

## Présentation et fonctionnalités
- Écran de connexion (validation locale, messages d’erreur clairs)
- Liste des notes (tri par date, plus récentes en premier)
- Création/édition d’une note (titre, contenu, timestamps)
- Suppression avec confirmation
- Stockage local via SQLite, CRUD complet

## Architecture du projet
Architecture simple inspirée MVC + Provider pour l’état.

```
lib/
  app.dart                      # MaterialApp, routes
  main.dart                     # Entrée, Providers
  core/
    theme/app_theme.dart        # Thème Material 3
    utils/validators.dart       # Validations simples
  models/
    note.dart                   # Modèle Note
  services/
    database_helper.dart        # Accès SQLite (sqflite)
  providers/
    auth_provider.dart          # Auth locale
    notes_provider.dart         # État des notes (CRUD)
  pages/
    login_page.dart             # Connexion
    home_page.dart              # Liste des notes
    edit_note_page.dart         # Création/édition
  widgets/
    note_card.dart              # Carte de note réutilisable
```

## Installation et exécution
Prérequis:
- Flutter SDK installé
- Android SDK/NDK (voir Dépannage si besoin)

Étapes:
1) Installer les dépendances
```
flutter pub get
```
2) Lancer sur un émulateur/appareil
```
flutter run
```
3) Tests
```
flutter test
```

## Base de données SQLite
Table `notes`:
```
id INTEGER PRIMARY KEY AUTOINCREMENT,
title TEXT,
content TEXT,
createdAt TEXT,   # ISO8601
updatedAt TEXT    # ISO8601
```
Accès via `services/database_helper.dart`:
- insertNote(Note)
- getAllNotes()
- updateNote(Note)
- deleteNote(id)

## Navigation et UX/UI
- Routes:
  - '/': LoginPage → redirection vers Home après succès
  - '/home': HomePage (liste de notes)
  - '/edit': EditNotePage (création/édition)
- Matériel 3, palette douce, espacements généreux, composants cohérents
- Feedback via SnackBars, validations de formulaire

## Tests
- `test/models/note_test.dart`: sérialisation Note
- `test/providers/notes_provider_test.dart`: CRUD en mémoire (sqflite_common_ffi)
- `test/widget_test.dart`: rendu de l’écran de login

## Wireframes
Des wireframes texte se trouvent (si utilisés) sous:
- `assets/wireframes/`

## Dépannage
- Gradle verrou/timeout au téléchargement:
  - Fermer autres builds/IDE
  - Supprimer cache partiel: `C:\Users\mouss\.gradle\wrapper\dists\gradle-8.10.2-all\`
  - Relancer
  - Option: passer à `-bin.zip` dans `gradle-wrapper.properties`

- Conflit de version Android NDK (plugins demandent 27):
  - Dans `android/app/build.gradle.kts`, définir:
    ```kotlin
    android {
      ndkVersion = "27.0.12077973"
    }
    ```
  - Installer la NDK 27 si absente (Android Studio > SDK Manager > SDK Tools)

- Problèmes réseau/proxy:
  - Configurer `android/gradle.properties` avec `systemProp.http(s).proxy*`
  - Essayer un autre réseau/VPN

## Choix techniques
- Provider: léger et suffisant pour cet usage
- sqflite: standard SQLite Flutter
- Material 3: UI moderne et accessible
- Dates en ISO8601 (interop et tri simple)

---
Licence: MIT (ou au choix). Contributions bienvenues.
