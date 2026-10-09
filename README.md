<div align="center">

# Flutter MVVM Template

**A production-ready Flutter starter — feature-first MVVM, simple enough for small and medium apps.**

[![Flutter](https://img.shields.io/badge/Flutter-3.11+-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.11+-0175C2?logo=dart&logoColor=white)](https://dart.dev)
[![BLoC](https://img.shields.io/badge/State-Cubit%2FBLoC-2E7D32)](https://bloclibrary.dev)
[![GetIt](https://img.shields.io/badge/DI-GetIt-7B1FA2)](https://pub.dev/packages/get_it)

[Quick Start](#-quick-start) · [Architecture](#-architecture) · [Commands](#-commands) · [New Feature](#-add-a-feature)

</div>

---

## Highlights

| | Feature |
|---|---|
| | **Feature-first MVVM** — `presentation` (View + Cubit) → `data` (Repository · DataSource) |
| | **Cubit + GetIt** — Cubit is the ViewModel; predictable state & DI |
| | **Either\<Failure, T\>** — typed errors via `dartz` + `safeCall` |
| | **Dio + Hive** — network with local cache fallback |
| | **go_router** — shell navigation with bottom tabs |
| | **Flavors + i18n** — `dev` / `prod` + `easy_localization` |
| | **Mason brick** — scaffold a full feature in one command |

> **Reference feature:** `home` — pagination, cache fallback, cancelable cubit.

---

## Architecture

```
┌──────────────────────────────────────────────────────┐
│  presentation   Pages · Widgets · Cubit · State      │
│                 (View + ViewModel)                   │
├──────────────────────────────────────────────────────┤
│  data           Models · DataSources · Repository    │
└──────────────────────────────────────────────────────┘
```

**Data flow:** `UI → Cubit → Repository → DataSource → Either<Failure, T> → emit State`

No domain layer or use cases — Cubit talks to the repository directly. Suited for simple and medium projects.

```
lib/
├── main_dev.dart / main_prod.dart     # Flavor entry points
├── config/          # env · routing · theme
├── core/            # DI · network · errors · storage
├── features/        # home · cart · profile · theme
│   └── <feature>/
│       ├── data/ · presentation/
│       └── <feature>_injection.dart
└── shared/          # widgets · navigation · mixins
```

---

## Quick Start

```bash
# 1. Install
flutter pub get

# 2. Configure env (edit lib/config/env/.env)
API_KEY=your_api_key
BASE_URL=https://your-api.example.com

# 3. Generate code
dart run build_runner build --delete-conflicting-outputs

# 4. Run
flutter run --flavor dev -t lib/main_dev.dart
```

> **Android:** `--flavor` is required (`dev` / `prod` in `build.gradle.kts`).

---

## Commands

### Run & Build

```bash
# Dev
flutter run --flavor dev -t lib/main_dev.dart

# Prod
flutter run --flavor prod -t lib/main_prod.dart

# Release
flutter build apk --flavor prod -t lib/main_prod.dart
flutter build appbundle --flavor prod -t lib/main_prod.dart
```

### Code Generation

```bash
# Envied + Hive (after editing .env or Hive models)
dart run build_runner build --delete-conflicting-outputs

# Translation keys
dart run easy_localization:generate --source-dir ./assets/translations -f keys -o locale_keys.g.dart
```

### New Feature (Mason)

```bash
dart pub global activate mason_cli
mason get
mason make feature --feature_name order --entity_name Order
```

### Tests

```bash
flutter test
```

---

## Add a Feature

After `mason make feature ...`, wire it up in **3 steps**:

| Step | File | Action |
|:----:|------|--------|
| 1 | `lib/core/constants/api_endpoints.dart` | Add API endpoint |
| 2 | `lib/core/di/injection_container.dart` | Call `initYourFeature()` in `initCore()` |
| 3 | `lib/config/routing/app_router.dart` | Register `GoRoute` |

**Pattern checklist** (follow `home`):

- [ ] Cubit injects repository and folds `Either` → `Loading / Loaded / Error`
- [ ] `RepositoryImpl` wraps calls with `safeCall`
- [ ] Page uses `BlocProvider(create: (_) => sl<YourCubit>())`

---

## Tech Stack

| Layer | Packages |
|-------|----------|
| Architecture | `dartz` · `equatable` · `get_it` |
| State | `flutter_bloc` |
| Network | `dio` · `internet_connection_checker_plus` |
| Routing | `go_router` |
| Storage | `hive` · `shared_preferences` · `flutter_secure_storage` |
| Config | `flutter_flavor` · `envied` |
| UI / i18n | `flutter_screenutil` · `shimmer` · `easy_localization` |

---

<div align="center">

**Built with care for scalable Flutter apps**

[Report Bug](https://github.com/MokarmDev/flutter-clean-architecture-template/issues) · [Request Feature](https://github.com/MokarmDev/flutter-clean-architecture-template/issues)

</div>
