# Saml ordrer og tilskriv gebyrer - Rapport 50101

**Kilde:** `src/Sales/OrderHandling/CombineLinesAddCharge.Report.al`

Denne rapport udfører **to ting** i rækkefølge:

## 1. Samler flere salgsordrer til én pr. debitor

Hvis en debitor har **flere salgsordrer** med samme **leveringsdato**, samler rapporten dem. Den tager den **første ordre** for hver debitor og flytter alle linjer fra de øvrige ordrer ind i den første. Det undgår, at flere små ordrer sendes separat til samme debitor på samme dag.

- Kun ordrer der **ikke** er markeret som "Særskilt ordre" medtages (så specielle ordrer forbliver uændrede).

## 2. Tilskriver gebyr på små ordrer

Efter samlingen kigger rapporten på debitorer, der er markeret med **"Gebyr på små ordrer" = Ja**. For disse debitorer tjekker den, om det **samlede ordrebeløb er under en beløbsgrænse**, som du angiver. Hvis det er tilfældet, tilføjer rapporten automatisk en **gebyr-vare** (et gebyrgebyr-linje) til ordren.

Tænk på det som: *"Hvis en debitors ordre er for lille, tilskriv et gebyr."*

## Hvad du udfylder, når du kører rapporten

- **Leveringsdato** - Hvilken dato's ordrer skal behandles (skal være i dag eller senere)
- **Beløbsgrænse** - Minimumsbeløb for ordren; ordrer under dette beløb får tilskrevet et gebyr
- **Gebyr-vare** - Hvilket varenummer der repræsenterer gebyret (f.eks. en "Småordre-gebyr"-vare)

## Kort sagt

> "Tag alle ordrer for den valgte dato, saml flere ordrer pr. debitor til én, og hvis den samlede ordre stadig er for lille, tilskriv et gebyr."
