---
title: Uke 1
aliases: 
  - Uke 1
lang: nb-NO
authors:
  - Sondre Grønås
created: 2023-01-08 14:42:29
updated: 2023-01-08 14:42:32
---
# Uke 1
Denne uken jobbet vi med:
- Vi så på test-drevet utvikling og øvde oss på Unit Testing

<iframe width="560" height="315" src="https://www.youtube.com/embed/ULxMQ57engo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Unit testing
Unit testing, eller test-drevet utvikling er en metode for å opprette ulike scenarioer som kan oppstå når vi programmerer noe. Ved å teste de ulike scenarioene sørger man for at koden fungerer slik vi forventer.

Dette gjør vi ved å bruke python pakken `pytest`, som har innebygde verktøy i PyCharm Professional, ved å opprette et testdokument som starter med navnet `test_<valgfritt-navn>.py`.  Funksjonene må også starte med `test_`, for eksempel `def test_min_funksjon():`

Via tester kan man også oppdage feil i kode, som vi ellers ikke nødvendigvis hadde fanget opp eller hatt tid til å teste manuelt. I test-drevet utvikling skriver man ofte tester først for å sette koden opp mot uforutsigbare scenarioer, og deretter programmere funksjonene til testene blir godkjent.

I tillegg kan man bekrefte at endringer i kode ikke ødelegger andre deler av appen eller forårsaker andre bugs. Tester til en musikk-app kunne vært å teste de ulike funksjonene en bruker vil gjøre:
- Logge inn med feil brukernavn/passord
- Logge inn med riktig informasjon
- Be om å få nullstilt passord
- Endre kontoinformasjon / passord
- Spille av musikk
- Laste ned musikk
- Sjekke at man kun kan laste ned med gyldig abonnement
- Sjekke at e-post varsler fungerer
- osv...

Man kan aldri skrive for mange tester, og det er ofte hvor nyansatte IT-folk starter ut. Kvalitetssikring er en viktig del av å utvikle en applikasjon som fungerer.

### Eksempel funksjon
Se for deg at vi har en simpel funksjon: vi selger en vare, som betyr at vi sitter igjen med mer penger når vi gjennomfører salget enn det vi hadde fra før.

```python title="main.py"
def selg_vare(pris, lommebok):
    """Returnerer den nye lommebok verdien (lommebok + pris)"""
    # Sørg for at verdiene er tall (float er tall med desimaler)
    pris = float(pris)  # uten konvertering: "100" + "100" = "100100"
    lommebok = float(lommebok)
    ny_lommebok = lommebok + pris
    return ny_lommebok
```

Vår app er enkel og ser slik ut:

```python title="app.py"
from main import selg_vare

print("Tast inn salg av vare")
pris = input("Pris: ")
lommebok = 100

ny_lommebok = selg_vare(pris, lommebok)
print(f"Du har nå {ny_lommebok}kr i lommeboken.")
```

### Problemer med funksjonen
Men hvordan kan vi vite at funksjonen fungerer uten problem? Her er noen scenarier hvor koden vil gjøre det vi ikke forventer av den:
- Pris på vare er `100kr` (ikke `100`)
- Pris på vare er `100,00` (ikke `100.00`)
- Pris på vare er `-100`
- Pris på vare er `quit`
- Lommebok er `100kr` (ikke `100`)

### Eksempel på tester
Koden slik den er nå kan produsere uønskede resultater, men ved å skrive en test kan vi sette opp funksjonen med en påstand/forventning av resultat, altså det vi ønsker at koden skal gjøre - dette er ofte "åpenbart", men fremdeles essensielt for å bekrefte at alt virker.

```python title="test_main.py"
import pytest
from main import selg_vare

def test_selg_vare_standard():
    assert selg_vare(50, 100) == 150

def test_selg_vare_tekst():
    assert selg_vare("50", "100") == 150
    assert selg_vare("50", 100) == 150

def test_selg_vare_feilverdi():
    # "with pytest.raises(<feilmelding>):" er en metode for å bekrefte at
    # koden spytter ut en feilmelding (krasjer) når den kjøres
    with pytest.raises(ValueError):
        selg_vare("quit", 100)
    # Testen vil gi en feilmelding dersom den "ikke" krasjer.
    
    with pytest.raises(ValueError):
        selg_vare("100kr", 100)
    
def test_selg_vare_negativ():
    assert selg_vare(-100, 100) == 100
    # Eller om det er ønskelig med feilmelding
	with pytest.raises(ValueError):
        selg_vare(-100, 100)
```

Nå har vi opprettet tester for å sjekke påstandene våre, og satt dem opp med det vi forventer. 

Merk at man kan bruke alle ulike sammenligningsoperatører (`==` er lik, `>` mindre enn, `<` større enn, `not` er ikke, osv..)

Nå gjenstår det kun å kjøre testene, og eventuelt korrigere koden slik at testene blir godkjent! (Lettere sagt enn gjort).

### Rette opp koden
Eksempel på håndtering av negative tall gjennom en if-sjekk (se [Guard-clauses](https://medium.com/lemon-code/guard-clauses-3bc0cd96a2d3)):

```python title="main.py"
def selg_vare(pris, lommebok):
    """Returnerer den nye lommebok verdien (lommebok + pris)"""
    # Sørg for at verdiene er tall (float er tall med desimaler)
    pris = float(pris)  # uten konvertering: "100" + "100" = "100100"
    if pris < 0:
        raise ValueError("Kan ikke selge varer for mindre enn 0!!")
    lommebok = float(lommebok)
    ny_lommebok = lommebok + pris
    return ny_lommebok
```

Les om hvordan få koden til å sende ut feilmeldinger her: https://www.w3schools.com/python/gloss_python_raise.asp

## Gjøremål
Du finner lenke til GitHub Classroom oppgaven i Teams - unittesting er et viktig verktøy, men ikke et vi skal jobbe så veldig mye med. Bruk det om du vil gjøre prosjektene dine proffere! 😎