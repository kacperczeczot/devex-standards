[Strona główna](README.md)

---

# Standardy Inżynierii i Architektury (DevEx)

Repozytorium stanowi centralny punkt odniesienia (**Konstytucję**) dla całego naszego ekosystemu. Definiuje żelazne reguły architektoniczne, wytyczne dotyczące stacku narzędziowego oraz twarde instrukcje dla asystentów AI.

---

## 1. Dopuszczalne Modele Projektowe

W ekosystemie obowiązują dokładnie **dwa deterministyczne modele projektowe** (reprezentowane przez dedykowane szablony):

| Model | Punkt wejścia kodu | Przeznaczenie |
| :--- | :--- | :--- |
| **Single-App** | `src/` | Pojedyncze aplikacje, gry, biblioteki i narzędzia jednozadaniowe. |
| **Monorepo** | `apps/` + `packages/` | Wielomodułowe platformy (`pnpm workspaces`, `Turborepo`, multi-target). |

---

## 2. Zamknięty Kanon Katalogu Root

Katalog główny (`root`) każdego projektu podlega rygorystycznemu ograniczeniu. Dozwolone są wyłącznie foldery z poniższego zamkniętego słownika:

```text
├── src/           # [Tylko Single-App] Kod źródłowy aplikacji
├── apps/          # [Tylko Monorepo] Aplikacje i moduły wykonawcze
├── packages/      # [Tylko Monorepo] Współdzielone paczki i biblioteki
├── docs/          # Dokumentacja projektowa, GDD, ADR-y
├── assets/        # Zasoby statyczne (grafiki, audio, modele, pliki PnP)
├── data/          # Lokalne bazy SQLite, zrzuty JSON, dane deweloperskie
├── scripts/       # Lekka automatyzacja (Node.js, bash) dla projektów webowych/hybrydowych
├── tools/         # Ciężkie narzędzia, kompilatory, symulatory (C++, Rust)
├── tests/         # Globalne testy integracyjne i E2E
└── .agents/       # Reguły i instrukcje dla systemów AI
```

> [!CAUTION]
> **Bezwzględny Zakaz Samowolki Folderowej**
> Tworzenie w korzeniu repozytoriów jakichkolwiek innych folderów najwyższego rzędu (np. `templates/`, `playtesting/`, `game/`, `tmp/`) jest zabronione. Wszelkie dodatkowe domeny muszą zostać zagłębione na 2. poziomie w ramach jednego z powyższych katalogów.

---

## 3. Konstytucja i Zasady (Dokumentacja)

| Sekcja | Ścieżka | Opis |
| :--- | :--- | :--- |
| **Baza Dokumentacji** | [`docs/README.md`](docs/README.md) | Centralny hub dokumentacyjny standardów |
| **Architektura** | [`docs/architecture/RULES.md`](docs/architecture/RULES.md) | Zod, Result Object, CSS Modules, czystość root |
| **Narzędzia i CI** | [`docs/tooling/RULES.md`](docs/tooling/RULES.md) | Stack narzędziowy (pnpm, Husky, CI) |
| **Rejestr Decyzji (ADR)** | [`docs/adr/README.md`](docs/adr/README.md) | Format i zasady rejestrowania decyzji architektonicznych |
| **Reguły AI** | [`.agents/rules/universal.md`](.agents/rules/universal.md) | Zasady Zero Samowolki i neutralności domenowej |
