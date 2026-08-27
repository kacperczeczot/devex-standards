[Strona główna](README.md)

---

# Standardy Inżynierii i Architektury (DevEx)

Repozytorium stanowi centralny punkt odniesienia (**Konstytucję**) dla całego naszego ekosystemu. Definiuje żelazne reguły architektoniczne, wytyczne dotyczące stacku narzędziowego oraz twarde instrukcje dla asystentów AI.

---

## 1. Dopuszczalne Modele Projektowe

W ekosystemie obowiązują dokładnie **dwa deterministyczne modele projektowe** (reprezentowane przez dedykowane szablony):

| Model | Punkt wejścia kodu | Oficjalny Szablon Repo | Przeznaczenie |
| :--- | :--- | :--- | :--- |
| **Single-App** | `src/` | [template-single-app](https://github.com/kacperczeczot/template-single-app) | Pojedyncze aplikacje, gry, biblioteki i narzędzia jednozadaniowe. |
| **Monorepo** | `apps/` + `packages/` | [template-monorepo](https://github.com/kacperczeczot/template-monorepo) | Wielomodułowe platformy (`pnpm workspaces`, `Turborepo`, multi-target). |

---

## 2. Zamknięty Kanon Katalogu Root

Katalog główny (`root`) każdego projektu podlega rygorystycznemu ograniczeniu. Dozwolone są wyłącznie foldery z poniższego zamkniętego słownika:

| Katalog | Model / Zastosowanie | Opis |
| :--- | :--- | :--- |
| [src/](docs/architecture/RULES.md#4-modele-projektowe-i-czystość-katalogu-głównego-root) | Single-App | Główny kod źródłowy aplikacji |
| [apps/](docs/architecture/RULES.md#4-modele-projektowe-i-czystość-katalogu-głównego-root) | Monorepo | Aplikacje i moduły wykonawcze |
| [packages/](docs/architecture/RULES.md#4-modele-projektowe-i-czystość-katalogu-głównego-root) | Monorepo | Współdzielone paczki i biblioteki |
| [docs/](docs/README.md) | Uniwersalny | Dokumentacja projektowa, GDD, ADR-y |
| [assets/](docs/architecture/RULES.md#konwencje-folderów-pomocniczych) | Single-App | Zasoby statyczne (grafiki, audio, modele, pliki PnP) |
| [data/](docs/architecture/RULES.md#polityka-czystości-katalogu-root) | Uniwersalny | Lokalne bazy SQLite, zrzuty JSON, dane deweloperskie |
| [scripts/](docs/architecture/RULES.md#konwencje-folderów-pomocniczych) | Uniwersalny | Lekka automatyzacja (Node.js, bash) |
| [tools/](docs/architecture/RULES.md#konwencje-folderów-pomocniczych) | C++ / Rust | Ciężkie narzędzia, kompilatory, symulatory |
| [tests/](docs/architecture/RULES.md#4-modele-projektowe-i-czystość-katalogu-głównego-root) | Uniwersalny | Globalne testy integracyjne i E2E |
| [.agents/](.agents/rules/universal.md) | Uniwersalny | Reguły i instrukcje dla systemów AI |

> [!CAUTION]
> **Bezwzględny Zakaz Samowolki Folderowej**
> Tworzenie w korzeniu repozytoriów jakichkolwiek innych folderów najwyższego rzędu (np. `templates/`, `playtesting/`, `game/`, `tmp/`) jest zabronione. Wszelkie dodatkowe domeny muszą zostać zagłębione na 2. poziomie w ramach jednego z powyższych katalogów.

---

## 3. Konstytucja i Zasady (Dokumentacja)

| Dokument | Opis |
| :--- | :--- |
| [Baza Dokumentacji (`docs/README.md`)](docs/README.md) | Centralny hub dokumentacyjny standardów |
| [Architektura (`docs/architecture/RULES.md`)](docs/architecture/RULES.md) | Zod, Result Object, CSS Modules, czystość root |
| [Narzędzia i CI (`docs/tooling/RULES.md`)](docs/tooling/RULES.md) | Stack narzędziowy (pnpm, Husky, CI, SemVer) |
| [Rejestr Decyzji ADR (`docs/adr/`)](docs/adr/README.md) | Format i zasady rejestrowania decyzji architektonicznych |
| [Reguły AI (`.agents/rules/universal.md`)](.agents/rules/universal.md) | Zasady Zero Samowolki i neutralności domenowej |

