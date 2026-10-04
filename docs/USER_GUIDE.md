# Používání a kontrola PWA

## Adresa a instalace

Produkční aplikace je na:

`https://petrkohoutek.github.io/gold-price-comparison/`

V mobilním prohlížeči ji lze přidat na plochu. Instalovaná aplikace používá stejná zveřejněná data jako webová stránka. Statické soubory aplikace jsou dostupné z cache; při připojení k internetu se produkční data požadují čerstvá.

## Aktuální porovnání

Horní část zobrazuje poslední zveřejněný den samostatně pro IBIS a Golden Gate. Zelená barva vždy označuje výhodnější hodnotu pro klienta:

| Údaj | Výhodnější hodnota |
|---|---|
| Prodejní cena | nižší |
| Výkupní cena | vyšší |
| Spread v Kč | nižší |
| Spread v % | nižší |

Při přesné shodě jsou zelené obě hodnoty.

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

Ve výchozím stavu se zobrazuje nejvýše 30 dnů. **Zobrazit vše** načte do tabulky celou zveřejněnou historii. **Stáhnout CSV** uloží právě zvolenou produkční nebo testovací datovou sadu.

## Produkční a testovací data

Produkční režim čte `data/production.json`. Soubor obsahuje pouze úplné schválené záznamy.

Tlačítko **Testovací data** otevře dialog pro heslo. Heslo se používá pouze v daném zařízení k místnímu rozšifrování `data/test.enc`; neposílá se na server a v repozitáři není uloženo. Tlačítko **Zrušit** dialog ihned zavře. Po úspěšném odemčení aplikace viditelně oznámí, že zobrazuje testovací data. **Zpět na produkci** znovu načte produkční režim.

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
