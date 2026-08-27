---
name: Uniwersalne Reguły AI
description: Twarde reguły określające zachowanie agentów i asystentów AI wewnątrz całego ekosystemu.
---

# Reguły Współpracy z AI

Niniejszy dokument zawiera twarde reguły określające zachowanie agentów i asystentów AI wewnątrz całego ekosystemu.

## 1. Zero Samowolki
AI ma absolutny zakaz wprowadzania jakichkolwiek modyfikacji do istniejących plików, tworzenia nowych katalogów czy uruchamiania skryptów bez wyraźnej, bezpośredniej prośby ze strony użytkownika. Każda większa zmiana architektoniczna wymaga zatwierdzenia.

## 2. Przenośność i Brak Hardcodowania
AI ma bezwzględny zakaz "hardcodowania" (zaszywania na sztywno) konkretnych nazw repozytoriów biznesowych oraz bezwzględnych ścieżek z lokalnego komputera (np. `/Users/kacper/...`) w generowanym przez siebie kodzie źródłowym we wszystkich projektach. Kod ma być zawsze przenośny i oparty na ścieżkach relatywnych lub zmiennych środowiskowych.

- **Wyjątek dla Standardów:** Z poziomu repozytorium standardów (`devex-standards`) dozwolone jest jawne odwoływanie się i linkowanie do oficjalnych repozytoriów szablonowych ekosystemu (`template-single-app`, `template-monorepo`).

## 3. Standardy Technologiczne dla AI
Podczas proponowania lub generowania kodu, AI musi bezwzględnie przestrzegać następujących reguł:
- **Walidacja Danych:** Zawsze używaj biblioteki `zod` do walidacji danych zewnętrznych na wszystkich stykach aplikacji (IPC, API).
- **Stylowanie:** Zawsze używaj `CSS Modules` w projektach frontendowych. Unikaj stylów globalnych.
- **Błędy:** Stosuj wzorzec "Result Object" zapobiegający niezłapanym wyjątkom na poziomie domenowym.

## 4. Kanon Dokumentacji i Breadcrumbs dla AI
Podczas tworzenia lub modyfikacji jakichkolwiek plików `.md` w całym ekosystemie, AI musi bezwzględnie umieszczać na samej górze pliku pasek nawigacyjny:
```markdown
[Strona główna](względna_ścieżka/README.md) > [Folder](README.md) > [Nazwa Pliku](plik.md)

---
```
- Zakaz pomijania breadcrumbs w jakimkolwiek pliku Markdown.
- Zawsze format `[Strona główna](...)` (mała litera "główna") z separatorem ` > ` i linią `---`.

## 5. Higiena Kodu i Zakaz Pozostawiania Śmieci
- **Usuwanie plików tymczasowych:** Po zakończeniu zadania AI ma bezwzględny obowiązek usunąć wszelkie utworzone przez siebie skrypty debugujące (`fix_*.py`, `patch_*.py`, `temp_*.ts`) oraz pliki tymczasowe.
- **Bezpieczeństwo danych:** AI ma kategoryczny zakaz tworzenia i commitowania plików z twardo zapisanymi sekretami, kluczami i tokenami.
- **Format commitów:** AI musi tworzyć wiadomości commitów ściśle według formatu Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`).

## 6. Konwencja Nazewnictwa i Językowa
- Wszystkie nowo tworzone katalogi, pliki skryptów i konfiguracje muszą być nazwane w języku angielskim w formacie `kebab-case`.
- Zabrania się tworzenia folderów takich jak `archiwum/` czy `nowe_pliki/` w root — archiwizacja odbywa się w `docs/archive/` lub `data/archive/`.

## 7. Procedura Standaryzacji Repozytoriów dla AI
Gdy użytkownik zleca agentowi standaryzację lub migrację repozytorium do standardów DevEx, agent musi bezwzględnie postępować według 5-fazowego protokołu zdefiniowanego w:
👉 **[Protokół Standaryzacji Repozytoriów (STANDARDIZATION_PLAYBOOK.md)](https://github.com/kacperczeczot/devex-standards/blob/main/docs/STANDARDIZATION_PLAYBOOK.md)**
- Faza 1: Audyt i klasyfikacja (Single-App vs Monorepo).
- Faza 2: Usunięcie śmieci (`*.bak`, `patch_*.py`) i zabezpieczenie sekretów.
- Faza 3: Reorganizacja katalogów do Kanonu Root.
- Faza 4: Naprawa importów i weryfikacja kompilacji/testów.
- Faza 5: Wdrożenie `docs/STANDARDS.md`, Breadcrumbs i szablonu PR.
