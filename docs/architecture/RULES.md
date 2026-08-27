[Strona główna](../../README.md) > [Dokumentacja](../README.md) > [Architektura](RULES.md)

---
# Architektura i Konwencje Kodu

Niniejszy dokument definiuje fundamentalne wzorce architektoniczne obowiązujące we wszystkich projektach w naszym ekosystemie.

## 1. Strażnik Granic (Boundary Guardian) - Zod
Wszelkie dane przekraczające granice systemów (np. IPC Tauri ↔ React, odpowiedzi z API, wejścia z plików konfiguracyjnych) muszą być rygorystycznie walidowane.

- **Jedno źródło prawdy:** Schematy Zod są trzymane w wydzielonej, reużywalnej paczce (np. `@prefix/schemas`). 
- **Zakaz cichego ufania typom:** Typy TypeScript są generowane z tych schematów (`z.infer`). Nie ufamy nagim typom `any` ani rzutowaniom `as` dla danych pochodzących z zewnątrz.

## 2. Obsługa Błędów - Result Object
Odchodzimy od tradycyjnego rzucania wyjątków (`throw new Error()`) w logice biznesowej na rzecz ustandaryzowanego obiektu wynikowego.

- **Struktura:** Funkcje i endpointy API powinny zwracać unię typów:
  `type Result<T, E> = { ok: true; data: T } | { ok: false; error: E; code?: string };`
- **Dlaczego?** Wymusza to na programiście jawną obsługę błędu na etapie kompilacji TypeScript oraz zapobiega nagłemu przerwaniu działania aplikacji. Instrukcji `throw` używamy wyłącznie do prawdziwie krytycznych stanów (fatal errors).

## 3. Architektura Kolokacji
Układamy kod domenowo, a nie technologicznie. 

- **Wszystko w jednym miejscu:** Każdy komponent (np. UI w React) powinien mieć wszystkie swoje zasoby w jednym, dedykowanym folderze. Dotyczy to logiki, stylów, testów oraz zapytań (GraphQL/REST).
- **Style CSS:** Wymuszamy stosowanie CSS Modules. Zabrania się tworzenia stylów globalnych dla konkretnych komponentów. Globalnie definiujemy wyłącznie tokeny CSS (zmienne określające paletę kolorów, typografię, spacing).

## 4. Modele Projektowe i Czystość Katalogu Głównego (Root)

W ekosystemie obowiązują dwa wykluczające się modele struktury kodu:
- **Model Single-App:** Cały kod aplikacji znajduje się w folderze `src/`. Nie stosuje się folderów `apps/` ani `packages/`.
- **Model Monorepo:** Cały kod wykonawczy znajduje się w podkatalogach wewnątrz `apps/`, a współdzielone moduły w `packages/`. W korzeniu projektu nie stosuje się folderu `src/`.

### System Statusów Katalogów w Szablonach
Szablony prezentują strukturę z kontrolowaną nadwyżką, w której każdy katalog posiada określony status:
- 🔴 **`[WYMAGANY]`**: Musi istnieć w każdym projekcie danego modelu (np. `src/` w Single-App, `apps/` i `packages/schemas/` w Monorepo, `docs/adr/`, `.agents/`).
- 🟡 **`[ZALECANY]`**: Powinien istnieć w projektach o umiarkowanej i dużej złożoności (np. `tests/e2e/`, `tests/integration/`, `docs/architecture/`, `packages/config/`).
- ⚪ **`[OPCJONALNY]`**: Stosowany wyłącznie wtedy, gdy projekt tego realnie wymaga (np. `assets/` dla grafik/audio, `data/` dla SQLite/zrzutów, `docs/api/` dla kontraktów OpenAPI, `packages/ui/` dla design systemu).

### Konwencje Folderów Pomocniczych
- **Automatyzacja (`scripts/` vs `tools/`):** Domyślnym folderem automatyzacji dla projektów webowych i skryptowych jest `scripts/`. Dla projektów natywnych (C++, Rust) oficjalny standard dopuszcza używanie `tools/` (np. na wewnętrzne kompilatory, narzędzia telemetryczne) jako alternatywy lub uzupełnienia dla `scripts/`.
- **Zasoby statyczne (`assets/`):** Występują w root w modelu Single-App. W modelu Monorepo nie stosuje się folderu `assets/` w root — zasoby należą do poszczególnych aplikacji lub współdzielonych pakietów UI.

### Polityka Czystości Katalogu Root
Katalog główny (`root`) każdego projektu musi bezwzględnie przestrzegać **zamkniętej listy dopuszczalnych folderów** zdefiniowanej w głównym `README.md`. Oprócz tego obowiązuje rygorystyczna polityka dotycząca plików w root:

- **ZAKAZ umieszczania kodu źródłowego:** W root nie może znaleźć się żaden plik `.js`, `.ts`, `.cpp`, `.py` z logiką wykonawczą. Miejsce na kod to `src/` (Single-App) lub `apps/` (Monorepo).
- **ZAKAZ umieszczania plików danych:** Zrzuty, logi, bazy `.sqlite`, surowe pliki `.csv` lub stany `.json` (niebędące konfiguracją narzędzi) bezwzględnie muszą trafić do `data/`.
- **ZAKAZ luźnej dokumentacji:** Dozwolona jest wyłącznie "Złota Trójca" plików w root: `README.md`, `LICENSE` oraz `CHANGELOG.md` (lub `STATUS.md`). Wszelkie inne notatki, plany i ADR-y lądują w `docs/`.

Główny katalog służy **wyłącznie jako tablica rozdzielcza dla ekosystemu i narzędzi**. Dopuszczalne są w nim tylko i wyłącznie manifesty i pliki konfiguracyjne (np. `.gitignore`, `.editorconfig`, `package.json`, `.env`, `CMakeLists.txt`, `tsconfig.json`, `pnpm-workspace.yaml`, `turbo.json`).

## 5. Standard Nawigacji Dokumentacji (Kanon Breadcrumbs)

Każdy plik dokumentacyjny (`.md`) w całym ekosystemie **musi rozpoczynać się od paska nawigacyjnego (breadcrumbs)** oddzielonego poziomą linią od tytułu dokumentu.

### Wzorzec Formatowania
```markdown
[Strona główna](względna_ścieżka/README.md) > [Folder Nadrzędny](README.md) > [Bieżący Plik](plik.md)

---

# Tytuł Dokumentu
```

### Żelazne Reguły Breadcrumbs:
1. **Nazewnictwo:** Zawsze `[Strona główna](...)` (mała litera „główna”).
2. **Separator:** Zawsze spacja, znak większości, spacja: ` > `.
3. **Ścieżki relatywne:** Każdy człon ścieżki musi być aktywnym, klikalnym linkiem względnym prowadzącym do odpowiedniego pliku `README.md` lub nadrzędnego węzła.
4. **Ostatni element:** Ostatni człon w ścieżce to nazwa bieżącego dokumentu (linkująca do siebie lub reprezentująca aktywny plik).
5. **Linia oddzielająca:** Pod paskiem nawigacyjnym zawsze musi znajdować się pusta linia, separator `---` oraz kolejna pusta linia przed głównym nagłówkiem `# H1`.

## 6. Higiena Repozytorium i Zakaz Śmieci (No Scratch Leftovers)
Repozytorium musi pozostawać czyste w każdym commicie:
- **Zakaz porzucania skryptów jednorazowych:** Wszelkie pomocnicze skrypty debugujące, testowe zrzuty czy skrypty patchy (`patch_*.py`, `fix_*.py`, `temp_*.ts`) muszą zostać bezwzględnie usunięte po zakończeniu prac. Jeśli skrypt ma trwałą wartość narzędziową, jego miejsce jest w `scripts/` (lub `tests/`).
- **Zakaz plików backupowych:** Pliki typu `*.bak`, `*.old`, `*.tmp`, `*.orig` nie mogą być commitowane do repozytorium.

## 7. Bezpieczeństwo Sekretów i Danych Autoryzacyjnych
- **Brak poświadczeń w Git:** Zakazuje się commitowania jakichkolwiek kluczy prywatnych, tokenów sesyjnych (`token.json`), plików poświadczeń (`credentials.json`) czy haseł.
- **Wymóg `.env.example`:** Każdy projekt korzystający ze zmiennych środowiskowych ma obowiązek utrzymywać plik `.env.example` jako bezpieczny szablon bez wartości wrażliwych.

## 8. Konwencja Nazewnictwa i Językowa (Kebab-Case & English for Infra)
- **Struktura techniczna:** Wszystkie katalogi, pliki kodu źródłowego, skrypty i konfiguracje muszą być nazwane w języku angielskim w formacie `kebab-case` (np. `sim-reports/`, `balance-notes/`, `test-runner.sh`).
- **Język polski:** Dopuszczalny jest wyłącznie w treści merytorycznej dokumentacji Markdown, lore gier oraz specyficznych nazwach encji biznesowych.

## 9. Standard Archiwizacji (Archive Policy)
- **Zakaz `archiwum/` w root:** Tworzenie folderów archiwalnych w katalogu głównym jest zabronione.
- **Archiwizacja dokumentacji:** Nieaktualne pliki dokumentacyjne trafiają do `docs/archive/`.
- **Archiwizacja danych:** Stare zrzuty danych trafiają do `data/archive/`.
- **Kod źródłowy:** Kodu źródłowego nie archiwizuje się w folderach — historię zmian przechowuje Git.

## 10. Pragmatyczna Strategia Pokrycia Testami (Pragmatic Coverage Policy)
W całym ekosystemie obowiązuje zasada: **Nie gonimy ogólnego % pokrycia dla całego repozytorium**. Wymagania dotyczące testów są definiowane **per warstwa architektoniczna**:
- **Logika domenowa i pakiety współdzielone (`packages/schemas`, `packages/core`, algorytmy gier):** Rygorystyczny próg (rekomendowane **≥ 85%**).
- **Warstwa serwisów i backendu (`apps/api`, serwery):** Umiarkowany próg (rekomendowane **≥ 75%**).
- **Powłoki UI i komponenty wizualne (`apps/web`, widoki):** Weryfikowane przez testy integracyjne / E2E (Playwright), a nie sztuczny coverage liniowy.

## 11. Dokumentowanie Kodu w Miejscu (TSDoc & Doxygen)
Wszystkie publiczne interfejsy, funkcje eksportowane z modułów oraz schematy danych muszą posiadać ustandaryzowaną dokumentację w kodzie:
- **TypeScript / JavaScript:** Format **TSDoc** ([tsdoc.org](https://tsdoc.org/)) z tagami `@param`, `@returns`, `@throws`, `@example`.
- **C++ / Rust:** Format **Doxygen** / rustdoc dla kluczowych struktur silnika i nagłówków.

