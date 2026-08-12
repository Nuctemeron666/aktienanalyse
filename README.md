# Aktienanalyse

Statische Einzelseite zur fundamentalen Aktienanalyse. Eingabe von Firmenname, Ticker, WKN oder ISIN;
Ausgabe von Kennzahlen mit Branchenmaßstab, Mehrjahrestrend, Fair-Value-Bandbreite, Qualitätscheck und Watchlist.

Kein Server, kein Build, kein Tracking — eine einzelne HTML-Datei.

## Auf GitHub Pages veröffentlichen

1. Neues Repository anlegen (öffentlich oder privat; für Pages im Gratis-Tarif muss es öffentlich sein).
2. `index.html` und diese `README.md` in den Hauptzweig hochladen.
3. Im Repository auf **Settings → Pages** gehen.
4. Unter *Build and deployment* → *Source* den Eintrag **Deploy from a branch** wählen.
5. Als Branch `main` und als Ordner `/ (root)` einstellen, dann **Save**.
6. Nach ein bis zwei Minuten ist die Seite unter `https://<benutzername>.github.io/<repository>/` erreichbar.

## API-Schlüssel

Beide Schlüssel werden im Aufklappbereich der Seite eingegeben und ausschließlich im
`localStorage` des Browsers gespeichert. Sie stehen nicht im Quelltext und gehören auch nicht dorthin.

| Dienst | Nötig für | Kosten |
|---|---|---|
| Alpha Vantage | Kennzahlen, Bilanz, Cashflow, Mehrjahrestrend, Qualitätscheck | kostenlos, 25 Abfragen/Tag |
| Anthropic | Auflösung von WKN/ISIN, Kurzfazit, Risikohinweise, Analyse ohne Alpha-Vantage-Key | nach Verbrauch, Bruchteile eines Cents pro Analyse |

Schlüssel holen:
- Alpha Vantage: https://www.alphavantage.co/support/#api-key
- Anthropic: https://console.anthropic.com

Ohne Anthropic-Schlüssel funktioniert die Suche über **Firmenname oder Ticker**; WKN und ISIN
lassen sich dann nicht auflösen. Eine vollständige Analyse verbraucht 6 Alpha-Vantage-Abfragen,
im Gratis-Tarif also rund vier Analysen pro Tag.

**Sicherheitshinweis:** Die Anfragen laufen direkt aus dem Browser an die Anbieter. Der
Anthropic-Schlüssel ist damit im Netzwerkverkehr des eigenen Rechners sichtbar. Für den privaten
Gebrauch ist das unkritisch; auf einer Seite, die andere Personen nutzen, sollte jede Person den
eigenen Schlüssel eintragen. Niemals einen Schlüssel in das Repository committen.

## Lokal ausprobieren

`index.html` genügt per Doppelklick im Browser. Alpha Vantage funktioniert über `file://` in der Regel,
die Anthropic-API kann je nach Browser CORS-Beschränkungen unterliegen — dann hilft ein lokaler Server:

```bash
python3 -m http.server 8000
# danach http://localhost:8000 aufrufen
```

## Was die Seite berechnet

- **Bewertung:** KGV, KCV, KUV, KBV, EV/EBITDA, PEG — Schwellen je nach Branche unterschiedlich
- **Rentabilität:** ROE, ROA, operative Marge, Nettomarge, Free-Cashflow-Rendite
- **Bilanz:** Eigenkapitalquote, Verschuldungsgrad, Liquiditätsgrad 3, Zinsdeckungsgrad
- **Wachstum:** Umsatz- und Gewinnwachstum gegenüber Vorjahresquartal
- **Ausschüttung:** Dividendenrendite, Ausschüttungsquote
- **Marktlage:** Trend gegenüber 50-/200-Tage-Linie, Analystenziel, Beta, 52-Wochen-Position
- **Mehrjahrestrend:** Umsatz, Nettomarge, Eigenkapitalquote, Free Cashflow über bis zu 5 Jahre
- **Fair Value:** Graham-Zahl, Ertragswert über branchenübliches KGV, vereinfachter DCF
- **Qualitätscheck:** Aktienrückkäufe, Dividendenkontinuität, Cashflow-Stabilität, Umsatztrend, Margenstabilität

## Besucherzahlen messen (optional)

GitHub Pages liefert keine Statistik — der Reiter *Insights → Traffic* im Repository
zählt nur Aufrufe der Repository-Seite auf github.com, nicht der veröffentlichten Seite.

Eingebaut ist deshalb eine Anbindung an **Cloudflare Web Analytics**: kostenlos,
ohne Cookies, ohne Wiedererkennung einzelner Personen.

1. Kostenloses Konto auf https://dash.cloudflare.com anlegen
2. Links im Menü **Analytics & Logs** → **Web Analytics** öffnen
3. **Add a site** und die eigene Adresse eintragen,
   z. B. `benutzername.github.io/aktienanalyse`
4. Cloudflare zeigt einen Code-Schnipsel an, darin steht `"token": "abc123..."`
5. Nur diesen Token in `index.html` ganz unten bei
   `const CF_ANALYTICS_TOKEN = '';` zwischen die Anführungszeichen setzen
6. Datei hochladen — nach ein bis zwei Minuten laufen die ersten Zahlen ein

Solange das Feld leer bleibt, wird kein Skript geladen und nichts gemessen.
Sobald ein Token eingetragen ist, erscheint automatisch ein entsprechender
Hinweis in der Fußzeile der Seite.

Sichtbar werden danach: Seitenaufrufe, Besucherzahlen, Herkunftsländer,
verweisende Seiten und verwendete Browser. Einzelne Personen sind nicht
identifizierbar.

## Grenzen

Branchenschwellen, Score, Fair-Value-Bandbreite und Qualitätscheck sind vereinfachte Modellrechnungen
mit pauschalen Annahmen, kein echter Peer-Vergleich. Die Fair-Value-Schätzung reagiert stark auf die
unterstellten Wachstums- und Zinsannahmen und ist bei Banken, Versicherungen, Immobilienwerten und
Verlustunternehmen praktisch nicht aussagekräftig.

Dies ist keine Anlageberatung. Werte vor einer Investitionsentscheidung in einer aktuellen Finanzquelle
gegenprüfen oder eine Finanzberatung hinzuziehen.

## Lizenz

Zur freien Verwendung und Anpassung.
