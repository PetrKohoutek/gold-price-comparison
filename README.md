# Porovnání cen zlata – PAMP Lady Fortuna 1 oz

Veřejná statická PWA s denním porovnáním cen IBIS a Golden Gate. Nasazuje se zdarma pomocí GitHub Pages.

- produkční data: `data/production.json`;
- zašifrovaná testovací data: `data/test.enc`;
- heslo pro testovací režim se ověřuje a používá pouze lokálně v prohlížeči.

Repozitář neobsahuje scraper, tokeny, soukromý audit ani čekající měření.

## GitHub Pages

V **Settings → Pages** nastavte zdroj **GitHub Actions**. Workflow `Nasazení PWA` publikuje větev `main`. Po prvním nasazení lze PWA otevřít na `https://petrkohoutek.github.io/gold-price-comparison/` a v mobilním prohlížeči přidat na plochu.
