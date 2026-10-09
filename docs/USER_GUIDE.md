# Používání a kontrola PWA

## Adresa a instalace

Produkční aplikace je na:

`https://petrkohoutek.github.io/gold-price-comparison/`

V mobilním prohlížeči ji lze přidat na plochu. Instalovaná aplikace používá stejná zveřejněná data jako webová stránka. Statické soubory aplikace jsou dostupné z cache; při připojení k internetu se produkční data požadují čerstvá.

## Aktuální porovnání

Nad historií jsou dvě samostatná porovnání:

1. **Aktuální porovnání, každou hodinu 7:00 až 17:00**: poslední platné hodinové měření z `data/latest.json`. Sběr probíhá v pracovní dny, tedy jedenáctkrát denně.
2. **Denní porovnání, pracovní dny v 18:20**: poslední schválené večerní měření z `data/production.json`.

Doplňky nadpisů mají stejnou velikost písma jako datum a čas. Obě části ukazují vlastní skutečné datum a čas načtení, oddělené tečkou `·`. GitHub může plánovaný běh zpozdit. V 19:00 tak může být nahoře měření kolem 17:00 a pod ním večerní kolem 18:20. Večerní sběr hodinový snímek nepřepisuje. O víkendu nebo při chybě zůstává poslední platné měření; stáří poznáte podle časového údaje.

Každé porovnání obsahuje samostatné karty IBIS a Golden Gate. Zelená barva vždy označuje výhodnější hodnotu pro klienta:

| Údaj | Výhodnější hodnota |
|---|---|
| Prodejní cena | nižší |
| Výkupní cena | vyšší |
| Spread v Kč | nižší |
| Spread v % | nižší |

Při přesné shodě jsou zelené obě hodnoty, bez peněžního rozdílu.

Pod výhodnější korunovou hodnotou je menším zeleným písmem rozdíl vůči druhému prodejci: u nižšího prodeje `−N Kč`, u vyššího výkupu `+N Kč`, u nižšího spreadu `−N Kč`. Písmo rozdílu má 12 px proti 20 px hlavního čísla (60 %). U spreadu v procentech se další korunový rozdíl nezobrazuje. Velikost karet se kvůli rozdílům nezvětšovala.

Pokud hodinový soubor ještě neexistuje, aplikace oznámí „Hodinové měření zatím není zveřejněno.“ Při jiné chybě načtení doporučí obnovit stránku. Denní porovnání a historie zůstávají samostatně dostupné; večerní ceny nenahrazují chybějící hodinové měření.

## Historická tabulka

Každé datum zabírá dva řádky a datum je společné pro oba:

1. první řádek: prodejní a výkupní ceny IBIS a Golden Gate;
2. druhý řádek: spread v Kč a spread v procentech obou prodejců.

Sloupce zleva:

1. datum;
2. IBIS prodej nebo spread v Kč;
3. IBIS výkup nebo spread v %;
4. Golden Gate prodej nebo spread v Kč;
5. Golden Gate výkup nebo spread v %;
6. plánovaný čas běhu;
7. skutečný čas načtení cen;
8. informace o opravě scraperu.

Názvy prodejců jsou vycentrované nad jejich dvěma sloupci. Oba řádky záhlaví jsou připnuté k hornímu okraji tabulky. Při prohlížení starších dnů se posouvá obsah tabulky a záhlaví zůstává viditelné. Na mobilu lze tabulku posouvat svisle i vodorovně.

Pod záhlavím je výrazná vodorovná čára. Jednotlivé dny odděluje silnější čára přes celou šířku tabulky, včetně data a provozních údajů. Uvnitř jednoho dne zůstává mezi cenami a spready jemné ohraničení. Oddělovače jsou kontrastní ve světlém i tmavém vzhledu.

Ve výchozím stavu se zobrazuje nejvýše 30 dnů. **Zobrazit vše** načte do tabulky celou zveřejněnou historii. **Stáhnout CSV** uloží právě zvolenou produkční nebo testovací datovou sadu.

## Produkční a testovací data

Produkční režim čte hodinový objekt `data/latest.json` a denní historii `data/production.json`. Oba obsahují úplné zveřejněné čtveřice cen. CSV obsahuje pouze denní historii, nikoli hodinový snímek.

Tlačítko **Testovací data** otevře dialog pro heslo. Heslo se používá pouze v daném zařízení k místnímu rozšifrování `data/test.enc`; neposílá se na server a v repozitáři není uloženo. Tlačítko **Zrušit** dialog ihned zavře. Po úspěšném odemčení aplikace viditelně oznámí, že zobrazuje testovací data, a skryje produkční hodinové porovnání, aby nemíchala obě sady. **Zpět na produkci** znovu načte produkční režim.

## Aktualizace instalované aplikace

Po nasazení nové verze:

1. počkejte, až workflow **Nasazení PWA** v GitHub Actions skončí zeleně;
2. instalovanou PWA úplně zavřete;
3. znovu ji spusťte s internetovým připojením;
4. pokud se změna ještě neprojevila, zavřete a otevřete aplikaci ještě jednou, aby se dokončila aktivace nového service workeru.

## Kontrola po nasazení

Po každé změně tabulky ověřte na počítači i mobilu:

1. IBIS a Golden Gate jsou uprostřed svých dvojic sloupců;
2. při svislém posunu zůstávají oba řádky záhlaví viditelné a nepřekrývají se;
3. tabulka se dá na mobilu posouvat vodorovně;
4. všechny buňky mají ohraničení a číselné hodnoty jsou zarovnané doprava;
5. zelené zvýraznění odpovídá pravidlům výše;
6. **Zobrazit vše**, stažení CSV, tmavý vzhled a testovací režim fungují;
7. produkční a testovací režim zobrazují správnou datovou sadu.

## Obsah veřejného repozitáře

Veřejně smějí být pouze statické soubory PWA, schválená produkční data a zašifrovaná testovací data. Scraper, čekající nebo odmítnutá měření, interní diagnostika, hesla a tokeny patří výhradně do soukromého repozitáře `gold-competitor-monitor`.

## Instalace v Chrome na počítači

Otevřete [aplikaci](https://petrkohoutek.github.io/gold-price-comparison/). Klikněte na instalační ikonu v adresním řádku; případně v nabídce **⋮ → Odeslat, uložit a sdílet → Nainstalovat stránku jako aplikaci**. Názvy nabídky se mohou podle verze Chrome lišit. Potvrďte instalaci. V samostatně otevřeném okně aplikace klikněte pravým tlačítkem na její ikonu na hlavním panelu Windows a vyberte **Připnout na hlavní panel**. [Oficiální návod Chrome](https://support.google.com/chrome/answer/9658361).

Instalace přesune zobrazení do samostatného okna; scraper dále běží na GitHubu, nikoli na počítači nebo telefonu.

## Kdy se obnovují ceny

Ceny se požadují ze sítě při načtení nebo obnovení stránky. Nové zveřejnění na GitHubu nezmění automaticky obrazovku již otevřenou na jiném zařízení. Aplikace nemá pravidelné dotazování ani obnovu při návratu z pozadí. Pro jistotu použijte obnovení stránky nebo aplikaci skutečně zavřete a otevřete online.

Nejprve musí skončit zeleně **Nasazení PWA**; samotný commit ještě neznamená zveřejnění na webu. Nové ceny a nová verze vzhledu jsou různé věci: statická verze může čekat na zavření všech starých oken a projevit se při druhém otevření. To není důvod, aby se při skutečném načtení stránky dál používaly staré ceny. Bez internetu se aktuálnost dat nezaručuje.

## Kontrola nových porovnání

- Ověřte zvlášť datum a čas hodinového i denního porovnání a shodu s jejich datovými soubory.
- Zkontrolujte zelené korunové rozdíly, shody bez nulového rozdílu a mobilní šířku.
- Historie a CSV musí dál obsahovat pouze denní data; testovací režim nesmí zobrazovat produkční hodinové ceny.
- Při testu chybějícího hodinového souboru musí fungovat denní část a historie.

Vlastník může při vědomém testu publikace dočasně změnit údaj `actual_time` v [data/latest.json](https://github.com/PetrKohoutek/gold-price-comparison/blob/main/data/latest.json), počkat na nasazení a ověřit nové načtení. Jde pouze o dočasnou testovací značku, nikoli nové měření; datum, čas a `captured_at` mají v běžném provozu odpovídat sobě. Po testu obnovte původní obsah a znovu počkejte na nasazení. Tento ruční test na zařízení nebyl v záznamu dnešního ověření potvrzen jako provedený.
