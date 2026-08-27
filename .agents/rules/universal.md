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
