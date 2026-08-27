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
