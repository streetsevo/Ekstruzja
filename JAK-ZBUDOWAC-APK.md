# Ekstruzja — jak zrobić z tego aplikację na telefon

Zawartość paczki:

| plik | do czego |
|---|---|
| `index.html` | cała aplikacja |
| `manifest.json` | nazwa, ikona, kolory — potrzebne do instalacji |
| `sw.js` | praca bez internetu |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` | ikony |
| `.nojekyll` | wyłącza przetwarzanie plików przez GitHub Pages |

Trzy drogi, od najszybszej. Wystarczy wybrać jedną.

---

## 1. Bez budowania — 2 minuty

Rozpakuj paczkę, otwórz `index.html` w Chrome na telefonie, potem menu `⋮` → **Dodaj do ekranu głównego**.

Dostajesz ikonę, pełny ekran i pracę bez zasięgu. Dane zapisują się w telefonie.

Jedyne, czego tu nie ma: pliku `.apk` do rozdania kolegom. Każdy musiałby zrobić to samo u siebie.

---

## 2. GitHub Pages + PWABuilder — prawdziwy APK, za darmo, z telefonu

Nic nie trzeba instalować. Wszystko w przeglądarce.

**Krok 1 — wrzuć pliki**

1. Załóż konto na `github.com`.
2. **New repository** → nazwa `ekstruzja` → **Public** → **Create**.
3. **Add file → Upload files** → wrzuć wszystkie pliki z paczki (bez tego README) → **Commit**.

Pliki muszą leżeć w głównym katalogu, nie w podfolderze.

**Krok 2 — włącz stronę**

**Settings → Pages** → Source: `Deploy from a branch` → Branch: `main`, folder `/ (root)` → **Save**.

Po 1–2 minutach dostaniesz adres:
`https://TWOJA-NAZWA.github.io/ekstruzja/`

Otwórz go i sprawdź, czy aplikacja działa.

**Krok 2b — SPRAWDŹ, zanim pójdziesz dalej**

Otwórz w przeglądarce dwa adresy:

1. `https://TWOJA-NAZWA.github.io/ekstruzja/` — musi pokazać aplikację
2. `https://TWOJA-NAZWA.github.io/ekstruzja/manifest.json` — musi pokazać tekst zaczynający się od `{ "id": "./"`

Jeśli drugi adres daje 404, PWABuilder napisze **Missing Name** i przycisk `Package For Stores` zostanie szary. Najczęstsze przyczyny:

| objaw | przyczyna | co zrobić |
|---|---|---|
| 404 na obu adresach | do repozytorium trafił ZIP zamiast plików | rozpakuj i wrzuć pliki pojedynczo |
| działa `/ekstruzja/ekstruzja-pwa/` | pliki są w podfolderze | przenieś do katalogu głównego repozytorium |
| 404 tylko na manifest.json | plik się nie wgrał | wrzuć `manifest.json` jeszcze raz |
| strona pusta, brak zielonego komunikatu w Pages | strona się jeszcze buduje | odczekaj 2 minuty i odśwież |

W repozytorium mają leżeć osobno: `index.html`, `manifest.json`, `sw.js`, `.nojekyll` i trzy pliki `icon-*.png`.

**Krok 3 — zbuduj APK**

1. Wejdź na `pwabuilder.com`.
2. Wklej adres strony ze **slashem na końcu**: `https://TWOJA-NAZWA.github.io/ekstruzja/` — nie adres repozytorium `github.com/...` → **Start**.
3. **Package for stores** → **Android**.
4. Zostaw domyślne ustawienia, zaznacz opcję podpisania nowym kluczem → **Download**.

W pobranym ZIP jest `app-release-signed.apk`. To jest plik do instalacji.

**Zachowaj plik `signing.keystore` i hasła z tego ZIP-a.** Bez nich nie zrobisz aktualizacji tej samej aplikacji — Android potraktuje nową wersję jako obcą i każe odinstalować starą.

**Krok 4 — instalacja**

Prześlij APK na telefon, otwórz, pozwól na instalację z nieznanych źródeł.

Aktualizacja później: podmień `index.html` na GitHubie, zmień numer w `sw.js` (`ekstruzja-v3` → `ekstruzja-v4`) i gotowe. APK sam pobierze nową wersję — nie trzeba go budować od nowa.

---

## 3. Prosto na telefonie, bez GitHuba

Jeśli nie chcesz zakładać konta:

- **WebIntoApp** / **Median.co** — wrzucasz ZIP z paczką, dostajesz APK mailem. Darmowy limit wystarcza.
- **Sketchware Pro** lub **AIDE** — kompilują APK bezpośrednio na telefonie. Wstawiasz komponent `WebView`, pliki lądują w `assets`.
- **Kodular** / **Thunkable** — układanka z klocków w przeglądarce, komponent `WebViewer`, przycisk `Export APK`.

Te drogi są szybsze, ale aktualizacja wymaga budowania APK od nowa za każdym razem.

---

## Powiadomienia — co działa, a co nie

W ustawieniach jest przycisk **Włącz powiadomienia**. Po zgodzie aplikacja alarmuje na 5 minut przed podmianą (czas do wyboru: 3, 5, 10, 15 min) i dopisuje, jeśli ta podmiana domknie paletę. Liczy wszystkie maszyny naraz, nie tylko tę otwartą na ekranie. Oprócz powiadomienia leci wibracja i dwa sygnały dźwiękowe — na wypadek, gdy system powiadomienia przytnie.

**Działa:** aplikacja otwarta, albo zwinięta i wisząca w tle.

**Nie zadziała pewnie:** ekran zgaszony i aplikacja zamknięta albo ubita przez oszczędzanie baterii. Android usypia wtedy stronę i alarm nie zadzwoni.

Żeby zwiększyć szanse, w ustawieniach telefonu wyłącz oszczędzanie baterii dla tej aplikacji (Ustawienia → Aplikacje → Ekstruzja → Bateria → bez ograniczeń).

Jeżeli alarm ma dzwonić pewnie przy zgaszonym ekranie, trzeba aplikację przepisać na natywną — Capacitor z wtyczką `local-notifications` albo Android Studio + Kotlin. Sama logika obliczeń zostaje bez zmian, przepisuje się tylko obudowę.

## Podmiany — harmonogram zmiany

Zakładka **Podmiany** w bocznym menu pokazuje wszystkie maszyny na jednej osi czasu: o której która schodzi, do końca zmiany. Godziny bliższe niż 15 minut są oznaczone na żółto — wtedy dwie maszyny wołają naraz i trzeba coś przestawić.

Pod spodem, przy każdej maszynie, pole **Pierwsza podmiana na zmianie**. Wpisujesz godzinę faktycznej pierwszej podmiany i naciskasz `Ustaw` — od tej godziny liczy się cały dalszy cykl. Przydaje się na początku zmiany, kiedy nie wiadomo, kiedy schodziło u poprzedników.

N1A i N1B mają własne godziny. Ekstruder E1 podaje nitkę do obu, ale nawijają niezależnie: zryw na N1A nie zatrzymuje N1B, więc po przewleczeniu jednej godziny rozjeżdżają się o tyle, ile trwał postój.

## Raporty zmian

W bocznym menu jest pozycja **Raporty zmian**. Każda zmiana zapisuje się sama w chwili zmiany warty — nic nie trzeba klikać. Archiwum trzyma **13 miesięcy**, starsze kasuje się automatycznie.

W raporcie zmiany siedzi: data, zmiana dzienna czy nocna, maszyna, zlecenie i lot, liczba obciągów, sztuki i kilogramy w rozbiciu A1 / A2 / B, utylizacja, zrywy, włókienka, straty w kg oraz czas odpadu i postoju.

Dwa przyciski eksportu: bieżący miesiąc i całe archiwum. Plik CSV otwiera się w Excelu, średnik jako separator, więc polskie Windows otworzy go bez kombinowania.

Cały rok to około **720 KB** — mieści się w pamięci telefonu z siedmiokrotnym zapasem.

Dane siedzą w jednym telefonie. Przed zmianą telefonu zrób `Eksport danych` w ustawieniach i `Import` na nowym.

## Po dniach wolnych

Aplikacja zapisuje tylko to, co widzi — jeśli przez dwa dni nikt nie miał jej otwartej, w archiwum będzie dziura. Dlatego zlecenie liczy się z dwóch rzeczy, które można sprawdzić fizycznie:

**Spakowane w kg** — sekcja `Zlecenie`. Dwa przyciski:
- `+ Dodaj paletę` — dokłada wagę jednej palety do sumy, na bieżąco w czasie zmiany.
- `= Ustaw łącznie` — zastępuje całą sumę. Po wolnych dniach bierzesz łączną wagę z magazynu i wpisujesz jedną liczbą, bez liczenia, ile palet doszło.

Pod spodem lista ostatnich wpisów i `Cofnij ostatni wpis`, gdyby palec się omsknął.

**Niespakowane** — zakładka `Magazyn i palety`, pola A1 / A2 / B przy każdej maszynie. Wpisujesz stan faktyczny z hali.

Te dwie liczby razem dają postęp zlecenia i prognozę końca. Obie biorą się z wagi i z przeliczenia palet, nie z tego, czy telefon chodził.

## Czego świadomie tu nie ma

**Wspólna baza dla całego wydziału.** Teraz każdy telefon liczy osobno. Przenoszenie danych — przyciski `Eksport` / `Import` w ustawieniach.

**Odczyt liczb ze zdjęcia.** Aparat czyta kod kreskowy z bloczka i sam wpisuje numer zlecenia. Liczb obciągów i gatunków ze zdjęcia nie da się czytać pewnie — zostają liczniki.
