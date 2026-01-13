# Flutter Clean Architecture Template

A production-ready Flutter starter that ships with clean architecture, BLoC, dependency injection, networking, localisation, theming, and authentication scaffolding. Use it to start projects quickly with a scalable, opinionated foundation.

## Features

- Clean architecture layers: Presentation, Domain, Data
- State management with `flutter_bloc` (base Bloc/State/Event provided)
- Dependency injection via `get_it`
- Networking with `dio`, interceptors, and centralised error handling
- Auth-ready structure with JWT + refresh token flow and secure storage
- Localisation using ARB + `intl` (English included, easy to extend)
- Light/Dark theming with centralised theme control
- Routing with `go_router`
- Utilities: `connectivity_plus`, `permission_handler`, `package_info_plus`

## Project Structure (lib/)

```
lib/
├── app/                    # Root app wiring (router)
├── core/
│   ├── bloc/               # Base Bloc/State/Event
│   ├── config/             # App configs/constants
│   ├── di/                 # Dependency injection setup
│   ├── errors/             # Error handling
│   ├── extensions/         # Common extensions
│   ├── network/            # Dio client, interceptors, API helpers
│   ├── services/           # Platform/services (e.g., permissions)
│   ├── theme/              # App themes
│   └── utils/              # Common utilities
├── features/
│   ├── auth/               # Auth data/domain/presentation
│   └── home/               # Home feature
├── generated/              # Generated localization files
├── l10n/                   # ARB localization inputs
└── main.dart               # Entry point
```

## Getting Started

```bash
git clone https://github.com/charanprasanth/flutter-clean-architecture-template.git
cd flutter-clean-template
flutter pub get
flutter run
```

## Dependency Injection

- Container: lib/core/di/injector.dart (`GetIt`).
- Bootstrapped in lib/main.dart via `initDependencies()` before `runApp`.
- Register new services/blocs there when adding features.

## Authentication Integration

- Repository and API calls: lib/features/auth/data/auth_repository.dart
- Session helper (token presence): lib/features/auth/data/auth_session.dart
- Tokens model: lib/features/auth/domain/auth_tokens.dart
- State management: lib/features/auth/presentation/bloc/auth_bloc.dart and auth_event.dart
- UI: lib/features/auth/presentation/pages/login_page.dart and lib/features/auth/presentation/ui/auth_screen.dart

## Networking

- HTTP client wrapper: lib/core/network/api_client.dart
- Interceptors: lib/core/network/interceptors/{auth_interceptor.dart, logging_interceptor.dart, error_interceptor.dart, referesh_token_interceptor.dart}
- Responsibilities: auth token injection, refresh flow, request/response logging, and error mapping.

## Routing

- Router config: lib/app/app_router.dart (`go_router`).
- Initial redirect checks `AuthSession` to choose `/home` vs `/login`.
- Screens: login (lib/features/auth/presentation/pages/login_page.dart), home (lib/features/home/presentation/pages/home_page.dart).

## Localisation

1. Add an ARB file in lib/l10n/ (e.g., app_es.arb).
2. Generate classes: `flutter gen-l10n`
3. Use strings in UI: `S.of(context).login`

## Permissions

- Wrapper service: lib/core/services/permission_service.dart (uses `permission_handler`).
- Android: optional permissions are commented in android/app/src/main/AndroidManifest.xml (CAMERA, MICROPHONE, STORAGE, LOCATION). Uncomment as needed.
- iOS: optional usage descriptions are commented in ios/Runner/Info.plist (camera, microphone, photos, location). Uncomment as needed.

## Theming

- Theme definitions: lib/core/theme/app_theme.dart
- Theme mode holder: lib/core/theme/theme_service.dart (light/dark/system).
- Current setup uses system mode in lib/app/app.dart; wire ThemeService to state management to toggle at runtime.

## Environment Config

- Env flags live in lib/core/config/env.dart (`BASE_URL`, `ENV`).
- Pass at run time, e.g.:

```bash
flutter run --dart-define=BASE_URL=https://api.example.com --dart-define=ENV=dev
```

## Adding a Feature (example: profile)

1. Create folder: lib/features/profile/{data,domain,presentation}
2. Register dependencies: lib/core/di/injector.dart
3. Add route: lib/app/app_router.dart

## Useful Commands

```bash
flutter pub get          # Install dependencies
flutter gen-l10n         # Generate localisation files
flutter test             # Run tests
flutter run              # Launch the app
```

## Upcoming

1. Tests
2. Flavors
