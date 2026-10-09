# Porovnání cen zlata – PAMP Lady Fortuna 1 oz

Veřejná statická PWA s denním porovnáním cen IBIS a Golden Gate. Nasazuje se zdarma pomocí GitHub Pages.

- produkční data: `data/production.json`;
- zašifrovaná testovací data: `data/test.enc`;
- heslo pro testovací režim se ověřuje a používá pouze lokálně v prohlížeči.

Repozitář neobsahuje scraper, tokeny, soukromý audit ani čekající měření.

## Co aplikace zobrazuje

- Aktuální porovnání z posledního úspěšného hodinového měření (pracovní dny 7:00–17:00) ve dvou kartách.
- Denní porovnání z posledního schváleného večerního měření (pracovní dny 18:20) ve stejných kartách.
- Historii, kde jeden den zabírá dva řádky: první obsahuje prodejní a výkupní ceny, druhý spready v Kč a procentech.
- Zelené zvýraznění výhodnější hodnoty: nižší prodej, vyšší výkup, nižší spread v Kč a nižší spread v procentech. Při shodě jsou zelené obě hodnoty.
- Provozní údaje až napravo: plánovaný čas, skutečný čas načtení a údaj, zda měření vzniklo po opravě scraperu.
- Prvních 30 dnů; tlačítko **Zobrazit vše** zpřístupní celou zveřejněnou historii.

Názvy **IBIS** a **Golden Gate** jsou vycentrované nad příslušnými dvojicemi sloupců. Oba řádky záhlaví zůstávají viditelné při svislém posouvání historie. Tabulku lze na úzkém displeji posouvat také vodorovně.

Podrobný návod pro používání, testovací režim, instalaci a kontrolu po nasazení je v [docs/USER_GUIDE.md](docs/USER_GUIDE.md). Datový kontrakt, mapa souborů, cache a pravidla změn jsou v [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md).

## GitHub Pages

V **Settings → Pages** nastavte zdroj **GitHub Actions**. Workflow `Nasazení PWA` publikuje větev `main`. Po prvním nasazení lze PWA otevřít na `https://petrkohoutek.github.io/gold-price-comparison/` a v mobilním prohlížeči přidat na plochu.

Každý push do `main` spustí nové nasazení. Úspěch se ověřuje v **Actions → Nasazení PWA**. Service worker ukládá statickou část aplikace pro spuštění bez sítě; produkční data se při dostupné síti vždy požadují čerstvá. Při změně vzhledu se zvyšuje jméno cache v `sw.js`, aby instalovaná PWA nepoužívala staré soubory.

Aktuální porovnání čte samostatný `data/latest.json`, denní porovnání a historie
čtou dosavadní `data/production.json`. Večerní měření hodinový soubor nepřepisuje.
U obou porovnání se zobrazuje jejich skutečné datum a čas. Chybějící hodinový
soubor zobrazí informaci o dosud nezveřejněném měření; nenahrazuje se večerními
cenami. Nové ceny se načtou při otevření nebo obnovení stránky. V testovacím
režimu se produkční hodinové porovnání skryje, aby se nemíchalo s testovacími daty.
