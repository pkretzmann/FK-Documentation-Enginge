# F&K Produktdokumentation

Dette er et eksempel på et professionelt dokument genereret fra Markdown.

## Introduktion

Velkommen til **F&K Documentation Engine**! Dette system gør det nemt at konvertere dine Markdown-filer til flotte Word-dokumenter med *professionel formatering* og dit firmalogo.

### Hovedfunktioner

- Automatisk styling baseret på firmafarver
- Logo integration i header
- Professionelle skrifttyper
- Understøttelse af alle standard Markdown elementer

## Kodeeksempler

Her er et eksempel på Python-kode:

```python
def hello_world():
    """En simpel funktion"""
    message = "Hello, Fich & Kretzmann!"
    print(message)
    return message

if __name__ == "__main__":
    hello_world()
```

Og her er noget AL-kode til Business Central:

```al
procedure CalculateTotal(var SalesLine: Record "Sales Line"): Decimal
var
    TotalAmount: Decimal;
begin
    TotalAmount := SalesLine.Quantity * SalesLine."Unit Price";
    exit(TotalAmount);
end;
```

## Sideskift

Indsæt sideskift med enten === eller 
<div style="page-break-after: always;"></div>


## Tabeller

Her er en oversigt over vores produkter:

| Produkt | Beskrivelse | Pris |
|---------|-------------|------|
| F&K Base | Grundlæggende funktionalitet | 500 kr/md |
| F&K Pro | Avancerede features | 1.200 kr/md |
| F&K Enterprise | Fuld funktionalitet | 2.500 kr/md |

## Lister

### Punktopstilling

- Første punkt med **fed tekst**
- Andet punkt med *kursiv tekst*
- Tredje punkt med `inline kode`
- Fjerde punkt med ~~gennemstreget tekst~~

### Nummereret liste

1. Installer Python 3.8+
2. Kør `pip install python-docx`
3. Download scriptet
4. Kør `python md_to_docx.py din_fil.md`

## Citater

> "Dokumentation er kærlighedsbreve, du skriver til dit fremtidige jeg."
> — Damian Conway

## Horisontal linje

Her kommer en adskillelse:

---

## Inline formatering

Du kan bruge **fed tekst**, *kursiv tekst*, `kode`, og endda ***fed og kursiv*** sammen.

Links ser sådan ud: [Fich & Kretzmann Website](https://fichkretzmann.dk)

## Konklusion

Med dette værktøj kan du nemt:

1. Skrive dokumentation i Markdown
2. Konvertere til professionelle Word-dokumenter
3. Dele med kunder og kolleger

**Tak fordi du bruger F&K Documentation Engine!**
