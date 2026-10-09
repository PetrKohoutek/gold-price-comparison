# Vývojářský popis veřejné PWA

## Úloha repozitáře

`gold-price-comparison` je veřejná statická prezentační vrstva. Neobsahuje scraper, validaci, Telegram, čekající měření, audit ani tajné hodnoty. Data sem zapisuje soukromý `gold-competitor-monitor` pomocí omezeného tokenu.

## Soubory

| Soubor | Úloha |
|---|---|
| `index.html` | struktura karet, tabulky, dialogu testovacího režimu a ovládacích prvků |
| `styles.css` | responzivní vzhled, barvy, ohraničení, zarovnání a připnuté dvouřádkové záhlaví |
| `app.js` | načtení dat, výpočty vítězů, vykreslení, CSV, tmavý režim a místní dešifrování testu |
| `manifest.webmanifest` | název, barvy, standalone režim a instalační ikony |
| `sw.js` | cache statických souborů; datové požadavky preferují síť |
| `data/production.json` | schválená veřejná produkční historie |
| `data/latest.json` | poslední platný hodinový snímek; nezávislý na večerní historii |
| `data/test.enc` | veřejně dostupný, ale šifrovaný testovací obsah |
| `.github/workflows/pages.yml` | nasazení celého repozitáře na GitHub Pages po pushi do `main` |

## Datový kontrakt

`production.json` je pole objektů se stejným kontraktem jako veřejný export monitoru:

```json
{
  "date": "RRRR-MM-DD",
  "ibis": {"sale_czk": 0, "buyback_czk": 0, "spread_czk": 0, "spread_percent": 0.0},
  "golden_gate": {"sale_czk": 0, "buyback_czk": 0, "spread_czk": 0, "spread_percent": 0.0},
  "corrected": false,
  "scheduled_time": "18:20",
  "actual_time": "HH:MM"
}
```

Nuly ukazují typy. Aplikace očekává číselné ceny a spready, datum ISO a časy jako text. Nejnovější datum je po načtení seřazeno jako první.

`test.enc` je obálka AES-256-GCM. `app.js` odvodí klíč z uživatelského hesla algoritmem PBKDF2-SHA256 a po úspěšném dešifrování očekává stejné pole záznamů. Heslo neopustí prohlížeč.

`latest.json` je jeden objekt, nikoli pole. Obsahuje stejná cenová pole jako denní
řádek, `date`, `actual_time`, `corrected` a časové razítko `captured_at` s pražským
offsetem. Nemá `scheduled_time: 18:20`. Vzniká pouze hodinovým workflow; denní
export ani schvalovací příkazy jej nepřepisují. PWA nezávisle načítá obě datové
sady a používá pro ně stejnou funkci vykreslení karet. Historie a CSV používají
výhradně denní řádky. Do prvního hodinového měření je 404 očekávaný stav.

Výjimka při zavedení: první snímek z 9. 10. 2026 v 20:55 výslovně dodal a
schválil vlastník. Není vydáván za automaticky získané měření a nepatří do historie.
Další platný hodinový sběr jej nahradí.

## Vykreslení a porovnání

Výhodnější je nižší prodej, vyšší výkup a nižší spread v Kč i procentech. Shoda zvýrazní obě hodnoty. Jeden den historie tvoří dva řádky se společnou buňkou data. První řádek zobrazuje ceny, druhý spready.

`table-wrap` je svisle i vodorovně posouvatelný. První řádek záhlaví má pevnou výšku 46 px a je na `top: 0`; druhý je na `top: 46px`. Při změně paddingu nebo výšky prvního řádku je nutné změnit obě hodnoty společně a ověřit mobilní zobrazení.

## Service worker a data

Service worker přednačítá statický shell. Při změně `index.html`, `styles.css`, `app.js`, manifestu nebo ikon zvyšte název `CACHE` v `sw.js`; jinak může instalovaná aplikace dál používat starou verzi.

Požadavky obsahující `/data/` preferují síť. Kód pro ně nepřidává novou odpověď do vlastní cache service workeru; při selhání sítě pouze zkusí již existující položku. Dokumentace proto nesmí slibovat aktuální data bez internetového připojení.

## Nasazení a kontrola

GitHub Pages nasazuje pouze větev `main`. Repozitář v **Settings → Pages** používá zdroj **GitHub Actions**. Workflow nepotřebuje Repository Secrets.

Po změně spusťte kontrolu z [USER_GUIDE.md](USER_GUIDE.md) na počítači a telefonu. Zvlášť ověřte oba připnuté řádky, vodorovný posun, testovací dialog, CSV, tmavý režim a nové načtení po zavření instalované PWA.

Veřejná data se běžně neupravují ručně. Pokud jsou chybná, opravuje se zdroj a stav v soukromém monitoru a data se znovu exportují.
