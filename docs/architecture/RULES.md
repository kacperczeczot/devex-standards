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

### Polityka Czystości Katalogu Root
Katalog główny (`root`) każdego projektu musi bezwzględnie przestrzegać **zamkniętej listy dopuszczalnych folderów** zdefiniowanej w głównym `README.md`. Oprócz tego obowiązuje rygorystyczna polityka dotycząca plików w root:

- **ZAKAZ umieszczania kodu źródłowego:** W root nie może znaleźć się żaden plik `.js`, `.ts`, `.cpp`, `.py` z logiką wykonawczą. Miejsce na kod to `src/` (Single-App) lub `apps/` (Monorepo).
- **ZAKAZ umieszczania plików danych:** Zrzuty, logi, bazy `.sqlite`, surowe pliki `.csv` lub stany `.json` (niebędące konfiguracją narzędzi) bezwzględnie muszą trafić do `data/`.
- **ZAKAZ luźnej dokumentacji:** Dozwolona jest wyłącznie "Złota Trójca" plików w root: `README.md`, `LICENSE` oraz `CHANGELOG.md` (lub `STATUS.md`). Wszelkie inne notatki, plany i ADR-y lądują w `docs/`.

Główny katalog służy **wyłącznie jako tablica rozdzielcza dla ekosystemu i narzędzi**. Dopuszczalne są w nim tylko i wyłącznie manifesty i pliki konfiguracyjne (np. `.gitignore`, `.editorconfig`, `package.json`, `.env`, `CMakeLists.txt`, `tsconfig.json`, `pnpm-workspace.yaml`, `turbo.json`).
