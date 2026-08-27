[Strona główna](../../README.md) > [Dokumentacja](../README.md) > [Narzędzia](RULES.md)

---
# Narzędzia i Ciągła Integracja

Dokument ten określa obowiązujący zestaw narzędzi deweloperskich oraz zasady utrzymania jakości kodu w repozytoriach. 

## 1. Menedżer Pakietów - pnpm
Globalnym standardem we wszystkich projektach opartych na ekosystemie JavaScript/Node.js jest **`pnpm`**.

- **Zasada wyłączności:** Docelowo `pnpm` jest jedynym dopuszczalnym menedżerem. Zabrania się używania `npm` (i generowania `package-lock.json`) oraz `yarn` w nowych projektach.
- **Dlaczego?** Gwarantuje ujednolicony interfejs instalacji, znaczną oszczędność miejsca na dysku (globalny store) oraz bezproblemową obsługę monorepo (workspaces).

## 2. Strażnicy Jakości (Pre-commit) - Husky
Dbamy o jakość kodu zanim w ogóle opuści on lokalną maszynę dewelopera.

- **Wymóg Husky:** Każde repozytorium powinno posiadać skonfigurowanego `husky` w połączeniu z `lint-staged`.
- **Automatyzacja:** Podczas każdej próby commita automatycznie uruchamiany jest linter (ESLint) oraz formater (np. Prettier). Zapobiega to wrzucaniu kodu niespełniającego standardów do repozytorium.

## 3. Ciągła Integracja - GitHub Actions (Uniwersalne CI)
Nie budujemy własnych, skomplikowanych skryptów bashowych do weryfikacji. 

- **Zasada "Golden Path":** Opieramy się na prostych, uniwersalnych i czytelnych plikach `.github/workflows/ci.yml`.
- **Minimum weryfikacyjne:** Każde repozytorium w ramach Pull Requestu musi automatycznie pomyślnie przejść instalację paczek, weryfikację typów, linter oraz testy jednostkowe za pomocą standardowych akcji GitHuba.

## 4. Standard Wiadomości Commitów (Conventional Commits)
Wszystkie commity w całym ekosystemie muszą stosować ustandaryzowane prefiksy:
- `feat:` — nowa funkcjonalność, mechanika, moduł
- `fix:` — naprawa błędu logicznego, typowania lub błędu w UI
- `docs:` — zmiany w dokumentacji Markdown, diagramach, komentarzach
- `refactor:` — przebudowa struktury kodu bez zmiany jego zachowania zewnętrznego
- `test:` — dodanie, poprawa lub rozbudowa testów
- `chore:` — zmiany w konfiguracji narzędzi, zależnościach, pipeline CI

## 5. Strategia Wersjonowania i Wydań (Semantic Versioning)
Wszystkie pakiety, biblioteki i aplikacje w ekosystemie podlegają ścisłemu wersjonowaniu semantycznemu (**SemVer**):
- **Format:** `MAJOR.MINOR.PATCH` (np. `1.2.3`).
  - `MAJOR`: Zmiany niekompatybilne wstecz (breaking changes).
  - `MINOR`: Nowe funkcjonalności kompatybilne wstecz.
  - `PATCH`: Poprawki błędów kompatybilne wstecz.
- **Tagowanie Git:** Każde wydanie produkcyjne musi posiadać tag w formacie `vX.Y.Z` (np. `v1.0.0`).
- **Dziennik Zmian:** Każde oficjalne wydanie wymaga podsumowania w pliku `CHANGELOG.md` na podstawie commitów.

## 6. Determinizm Zależności (Strict Lockfile Policy)
- **Frozen Lockfile w CI:** Wszystkie zadania CI muszą bezwzględnie instalować zależności z flagą zamrażającą plik lock (`pnpm install --frozen-lockfile` lub odpowiednik).
- **Zabezpieczenie Lockfile:** Pliki `pnpm-lock.yaml`, `Cargo.lock`, `poetry.lock` są obowiązkowo śledzone w Git.
- **Wersje Środowiska:** Wersje środowisk wykonawczych (np. Node.js, kompilatory) muszą być zdefiniowane w plikach konfiguracyjnych (`.nvmrc`, `package.json engines`).

## 7. Strategia Gałęzi i Szablony GitHub (Branching & PR Standards)
- **Model Gałęzi:**
  - `main` — gałąź główna, zawsze stabilna, chroniona przed bezpośrednim pushem.
  - `feat/<nazwa>` — gałęzie dedykowane nowym funkcjonalnościom.
  - `fix/<nazwa>` — gałęzie dedykowane poprawkom błędów.
  - `chore/<nazwa>` — gałęzie prac konfiguracyjnych i toolingowych.
- **Wymóg Szablonu PR:** Każdy Pull Request tworzony w repozytorium musi korzystać z ujednoliconego szablonu `.github/pull_request_template.md`.

## 8. Standard Formatowania i Lintera (Code Style Baseline)
- **Formatowanie:** Obowiązuje automatyczne formatowanie kodu przy użyciu Prettier / Biome w oparciu o reguły `.editorconfig`.
- **Weryfikacja:** Kod przed commitem (Husky) oraz w ramach CI (GitHub Actions) musi bezwzględnie przejść weryfikację `lint` oraz `typecheck`.

