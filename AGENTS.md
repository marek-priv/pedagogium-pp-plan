# AGENTS.md — Protokół aktualizacji planu (pedagogium-pp-plan, pierwsze stopień)

**ZAWSZE przeczytaj ten plik PRZED wprowadzeniem jakichkolwiek zmian w `index.html`.**

## Mapowanie żądań użytkownika

- „update PP / plan PP / pierwsze stopień" → **to repozytorium** (`marek-priv/pedagogium-pp-plan`, strona https://marek-priv.github.io/pedagogium-pp-plan/)
- „update magisterka / magisterskie" → repozytorium `marek-priv/pedagogium-plan` (osobny projekt — analogiczny protokół)

## Źródło danych

- Użytkownik dostarcza nowy plan jako plik `.docx` w promptcie (ścieżka w `C:\Users\Dell\CascadeProjects\Studia\`).
- **NIGDY nie polegaj na spłaszczonym tekście dokumentu** — wypakuj `word/document.xml` i parsuj strukturę `w:tbl` → `w:tr` → `w:tc` komórka po komórce. Klasy w slocie czasowym to osobne akapity wewnątrz komórki; lista `/zjazd: .../` jest na końcu opisu przedmiotu.

## Zasady aktualizacji

1. **Zjazdy 1–3 są przeszłe i nietknięte** — nie wolno zmieniać ich wpisów, dat, prowadzących ani statusów. Zmiany stosuj tylko dla zjazdów 4–9 (`PAST_ZJ`).
2. **Weryfikacja komórka po komórce:** zbuduj oczekiwaną siatkę (dzień × slot × zjazd → klucz przedmiotu+forma) bezpośrednio z docx i porównaj z `CLASSES.sobota` / `CLASSES.niedziela` **programowo** (skrypt Node), a nie „na oko". Dopuszczalne różnice: wpisy wyłącznie dla zjazdów 1–3 (historia) oraz ręczne elementy dodane wcześniej.
3. **Zachowuj mechanizmy:** `PAST_ZJ`, `PAST_SOB_ZJ`, `ONLINE_NDZ_ZJ`, `SALA_NDZ`, `SALA_NDZ_ALT`, `exemptZj`, `notHeldZj`, `star`, `note` — nie usuwaj ich bez wyraźnej potrzeby.
4. **Zaliczenia i kontakty:** jeśli docx zmienia prowadzących — synchronizuj `SUBJECTS` i sekcję Kontakt.
5. **Lokalizacja:** w tym planie sobota i niedziela odbywają się **online (MS Teams)** — sprawdź aktualny stan stałych w pliku przed edycją.

## Dokumentowanie zmian

- **KAŻDA** zmiana musi być opisana w zakładce **Zmiany** (`<div class="changes">`) — nowa grupa `chg-group` z datą na górze listy + zaktualizuj stempel `Aktualizacja:` w nagłówku.
- Format: zwięzłe `<li>` z podziałem na dni / przedmioty / lokalizacje.

## Jakość renderowania

- Tekst w elementach graficznych (karty `.cls`, chipy `.fchip`, kolumny tabeli, notatki) **nie może być ucinany ani wychodzić poza kontener**. Jeśli dodajesz dłuższe napisy — sprawdź, czy nie rozjeżdża layoutu.
- Zwijane kolumny zjazdów: `display:none` na `td` jest ZAKAZANY (psuje wyrównanie tabeli) — używaj `col-hidden` (zwężanie komórek).

## Weryfikacja przed commitem

1. `node --check` na wyekstrahowanym `<script>`.
2. Programowe porównanie siatki z docx (punkt 2 powyżej).
3. `git add -A && git commit && git push` → GitHub Pages aktualizuje się w ~2–4 minuty.
