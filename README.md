# Słówka

Prywatna aplikacja do nauki słówek (fiszki, wpisywanie, nagrania wymowy). Ten folder zawiera wyłącznie kod. Dane (słówka, postęp, nagrania) trzymasz w osobnym, prywatnym repozytorium, a token zapisuje się tylko lokalnie na urządzeniu.

Pliki:
- `index.html`: cała aplikacja w jednym pliku
- `manifest.webmanifest`, `sw.js`, `icon-*.png`, `apple-touch-icon.png`: instalacja na ekranie głównym i działanie offline

## 1. Publikacja na GitHub Pages (repozytorium `slowka`)

1. Otwórz repozytorium `slowka` na GitHubie i wybierz **Add file → Upload files**.
2. Przeciągnij wszystkie pliki z tego folderu (bez podfolderów) i kliknij **Commit changes**.
3. Wejdź w **Settings → Pages**. W sekcji *Build and deployment* ustaw **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**, a potem kliknij **Save**.
4. Po około minucie aplikacja będzie dostępna pod adresem `https://TWOJA-NAZWA.github.io/slowka/`.
5. Przy aktualizacji wgrywasz nowy `index.html` w to samo miejsce. Jeśli zmienisz pliki, podbij też wersję `CACHE` w `sw.js` (np. `slowka-v2`), żeby telefony szybciej pobrały nową wersję.

## 2. Dodanie do ekranu głównego

**iPhone (Safari):**
1. Otwórz adres aplikacji w Safari. W innych przeglądarkach na iOS ta opcja może nie działać.
2. Stuknij **Udostępnij** (kwadrat ze strzałką) → **Do ekranu początkowego** → **Dodaj**.
3. Uruchamiaj aplikację z ikony „Słówka”. Otworzy się na pełnym ekranie, bez paska przeglądarki.

**Android (Chrome):**
1. Otwórz adres aplikacji w Chrome.
2. Stuknij menu **⋮** → **Zainstaluj aplikację** albo **Dodaj do ekranu głównego**.

Ważne: aplikacja z ekranu głównego ma osobną pamięć. Na iPhonie trzeba więc w niej wpisać token jeszcze raz, nawet jeśli wcześniej był wpisany w Safari.

## 3. Połączenie z danymi (repozytorium `slowka-dane`)

1. W aplikacji wejdź w **Ustawienia → Synchronizacja z GitHub**.
2. Wpisz nazwę użytkownika i repozytorium `slowka-dane`, a potem wklej token.
3. Kliknij **Sprawdź połączenie**. Instrukcja tworzenia tokenu jest w aplikacji pod „Jak utworzyć token”. Token ma mieć dostęp tylko do `slowka-dane`, z uprawnieniem *Contents: Read and write*.
4. Jeśli w repozytorium nie ma jeszcze `data.json`, aplikacja zaproponuje wysłanie danych z urządzenia (razem z nagraniami).

## 4. Przeniesienie danych z obecnej wersji

**Sposób A (bezpośrednio):**
1. Otwórz obecną aplikację (`Slowka.dc.html`) tam, gdzie z niej korzystałeś. Nowa wersja sama przejmie zapisane tam dane.
2. W Ustawieniach podłącz GitHuba i kliknij **Sprawdź połączenie**, a potem **Wyślij dane**.
3. Na telefonie i komputerze otwórz wersję z GitHub Pages i podłącz ten sam token. Dane pobiorą się same.

**Sposób B (przez plik):**
1. W obecnej wersji: **Ustawienia → Eksportuj wszystko (JSON)**. Plik zawiera też nagrania.
2. W wersji z GitHub Pages: **Ustawienia → Importuj z pliku… → Scal z obecnymi**.
3. Podłącz GitHuba. Zmiany wyślą się automatycznie.

## Jak działa synchronizacja

- Każda zmiana zapisuje się najpierw na urządzeniu. Bez internetu wskaźnik w nagłówku pokazuje „Oczekuje”, a zmiany wyślą się po powrocie sieci.
- Przed każdym zapisem aplikacja pobiera najnowszy `data.json` i zapisuje go z jego SHA. Przy konflikcie scala zmiany po ID słówka i dacie modyfikacji, a potem zapisuje ponownie. Usunięte słówka zostają oznaczone jako usunięte, więc scalanie ich nie przywraca.
- Nagrania trafiają do folderu `audio/` jako krótkie pliki webm/opus (na iPhonie m4a), maksymalnie 15 sekund. W `data.json` jest zapisana tylko ścieżka do pliku.
- Gdy token wygaśnie, pojawi się komunikat z polem na nowy token. Niewysłane zmiany zostają na urządzeniu.
