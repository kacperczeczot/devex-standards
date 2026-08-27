[Strona główna](../../README.md) > [Dokumentacja](../README.md) > [Playbook Standaryzacji](STANDARDIZATION_PLAYBOOK.md)

---

# Protokół Standaryzacji Repozytoriów dla Agentów AI

Niniejszy dokument jest **operacyjnym podręcznikiem wykonawczym** dla modeli i agentów AI (Cursor, Claude Code, Antigravity, Copilot, Windsurf). Definiuje deterministyczny, bezbłędny algorytm dostosowywania istniejących, nieuporządkowanych projektów do oficjalnych standardów DevEx.

---

## 🎯 Cel Protokołu
Transformacja dowolnego repozytorium do **100% zgodności ze standardami DevEx** przy zachowaniu pełnej sprawności kodu, poprawności importów oraz bezstratności danych biznesowych.

---

## 🚦 Algorytm Wykonawczy (Krok po Kroku)

### FAZA 1: Audyt i Klasyfikacja Modelu
1. **Identyfikacja modelu:**
   - **Single-App:** Jedna aplikacja, biblioteka, gra, silnik symulacji lub skrypt (`template-single-app`).
   - **Monorepo:** Wiele aplikacji klienckich, backendów lub współdzielonych pakietów (`template-monorepo`).
2. **Inwentaryzacja naruszeń katalogu `root`:**
   - Wykryj luźne pliki z kodem wykonawczym (`*.js`, `*.ts`, `*.py`, `*.cpp`, `*.sh`).
   - Wykryj luźne pliki z danymi (`*.json`, `*.sqlite`, `*.csv`, `*.log`).
   - Wykryj foldery spoza Kanonu Root (np. `playtesting/`, `archiwum/`, `game/`, `tmp/`, `nowe_pliki/`).

---

### FAZA 2: Bezpieczeństwo i Sprzątanie Śmieci
1. **Ochrona Sekretów:**
   - Jeśli w root leżą pliki uwierzytelniające (`token.json`, `credentials.json`, `.env`), natychmiast dopisz je do `.gitignore`.
   - Stwórz bezpieczny szablon `.env.example` (bez prawdziwych haseł/kluczy).
2. **Usuwanie Plików Roboczych:**
   - Usuń wszelkie pliki tymczasowe: `*.bak`, `*.tmp`, `*.old`, `*.orig` oraz porzucone skrypty patchy (`patch_*.py`, `temp_*.ts`).
3. **Archiwizacja:**
   - Przenieś przestarzałą dokumentację z `root` lub `docs/` do `docs/archive/`.
   - Przenieś stare zrzuty danych do `data/archive/`.
   - Usuń foldery `archiwum/` z katalogu głównego.

---

### FAZA 3: Fizyczna Reorganizacja Katalogów
Przenieś pliki do odpowiednich kontenerów zgodnie z Kanonem Root:

| Typ Zasobu | Gdzie przenieść? | Uwagi |
| :--- | :--- | :--- |
| **Kod Źródłowy (Single-App)** | `src/` | Zastosuj architekturę kolokacji domenowej (np. `src/auth/`, `src/sim/`). |
| **Aplikacje (Monorepo)** | `apps/<nazwa>/` | Każda aplikacja otrzymuje własny podfolder (np. `apps/web/`, `apps/api/`). |
| **Pakiety Współdzielone** | `packages/<nazwa>/` | Schematy Zod do `packages/schemas/`, konfiguracje do `packages/config/`. |
| **Zasoby Webowe (Single-App)** | `public/` | Pliki serwowane verbatim przez frameworki webowe (robots.txt, favicon, CMS). |
| **Multimedia i Binaria** | `assets/` | Grafiki źródłowe, audio, czcionki, modele 3D, pliki do druku PnP. |
| **Bazy i Surowe Dane** | `data/` | Bazy SQLite, zrzuty JSON/CSV, cache. Pamiętaj o regułach `.gitignore`. |

---

### FAZA 4: Aktualizacja Ścieżek, Importów i Weryfikacja
1. **Aktualizacja Importów:**
   - Zaktualizuj ścieżki relatywne (`../../`) oraz aliasy ścieżek w `tsconfig.json` (np. `@/*` wskazujące na `src/*`).
   - Zaktualizuj pliki konfiguracyjne narzędzi (`vite.config.ts`, `CMakeLists.txt`, `Dockerfile`, `package.json`).
2. **Weryfikacja Kompilacji i Testów:**
   - Uruchom `pnpm lint` / `npm run lint` (brak błędów lintera i formatowania).
   - Uruchom `pnpm typecheck` / kompilator (brak błędów typowania).
   - Uruchom testy jednostkowe (`pnpm test` / `cargo test`).

---

### FAZA 5: Wdrożenie Dokumentacji i Kanonu DevEx
1. **Wdrożenie Certyfikatu Standardów:**
   - Utwórz plik `docs/STANDARDS.md` wskazujący na model projektu oraz centralną Konstytucję `https://github.com/kacperczeczot/devex-standards`.
2. **Uporządkowanie Spisów Dokumentacji:**
   - Utwórz lub zaktualizuj `docs/README.md` oraz zainicjalizuj rejestr `docs/adr/0000-szablon-decyzji.md`.
3. **Kanon Breadcrumbs:**
   - Dodaj pasek nawigacyjny na samej górze **każdego** pliku `.md` w repozytorium:
     ```markdown
     [Strona główna](względna_ścieżka/README.md) > [Katalog](README.md) > [Nazwa](plik.md)

     ---
     ```
4. **Szablon PR:**
   - Utwórz `.github/pull_request_template.md`.
5. **Format Commita:**
   - Zacommituj zmiany z czytelnym komunikatem Conventional Commits:
     `refactor(structure): migrate repository layout to DevEx standards`
