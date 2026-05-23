# Clean Architecture — Flutter (BLoC / Cubit / get_it)

A concise learning project demonstrating Clean Architecture in Flutter using BLoC/Cubit for state management and `get_it` for dependency injection. This repository is intended as a study scaffold — focused on structure, boundaries, and testability rather than a full production app.

**Overview**

- **Goal:** Learn how to structure a Flutter app with clean separation of concerns (presentation, domain, data), manage state with BLoC/Cubit, and wire dependencies with `get_it`.
- **Audience:** Flutter developers learning architecture patterns and DI + state management best practices.

**Architecture**

- **Presentation:** Flutter UI, `Bloc`/`Cubit` to handle UI state and events.
- **Domain:** Entities, use cases (business rules), and repository interfaces.
- **Data:** Repository implementations, remote/local data sources, models and mappers.

This separation enables unit testing of business logic without Flutter, and easy swapping of data sources.

**Project Structure (suggested)**

- `lib/`
  - `main.dart` — app entry and `get_it` setup
  - `core/` — shared utilities, constants, error handling
  - `features/` — feature folders; each feature follows: `presentation/`, `domain/`, `data/`
    - `presentation/` — screens, widgets, blocs/cubits
    - `domain/` — entities, repositories (abstract), usecases
    - `data/` — models, repository implementations, data sources

**State Management (BLoC / Cubit)**

- Use `flutter_bloc` for predictable state management.
- Prefer `Cubit` for simple state flows and `Bloc` when you need event-to-state mapping.
- Keep UI as dumb as possible: UI reads state and triggers events/actions on the bloc/cubit.

Example Cubit registration and usage (high-level):

```dart
// register in get_it
final getIt = GetIt.instance;
getIt.registerFactory(() => MyCubit(getIt<MyUseCase>()));

// in a widget
final cubit = context.read<MyCubit>();
cubit.load();
```

**Dependency Injection (`get_it`)**

- Use `get_it` to register factories, singletons, and external clients (e.g., http client).
- Keep registration centralized (e.g., `injector.dart` or `service_locator.dart`) and callable from `main()` before `runApp()`.

**Testing**

- Unit test `usecases`, `cubit`/`bloc` logic, and repository implementations using mocking.
- Widget tests for important UI flows using `bloc_test` and `flutter_test`.

Quick commands:

```bash
flutter pub get
flutter run   # or: flutter run -d <device>
flutter test
```

**Conventions & Tips**

- Small, focused use cases — each use case does one thing.
- Repositories expose abstract interfaces in `domain/` and implementations in `data/`.
- Keep models and mapping logic near the `data/` layer to avoid leaking data-layer types into domain.
- Keep state classes (e.g., `MyState`) immutable and equip them with copy/clone helpers if needed.

**Resources**

- Uncle Bob — Clean Architecture (concepts)
- flutter_bloc package docs
- get_it package docs

**Next steps / Learning exercises**

- Implement a simple feature end-to-end: remote data source → repository → usecase → cubit → UI.
- Add unit tests for the usecase and cubit, then add a widget test for the UI flow.
- Explore integrating `freezed` for immutable data classes and union states.

---

If you'd like, I can scaffold a starter feature (folders + example cubit/usecase/repository) and register DI for you.
