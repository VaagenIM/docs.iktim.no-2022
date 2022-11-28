---
title: Uke 47 - Digitale Spor, Koding
aliases: 
  - Uke 47
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-11-23 10:25:35
updated: 2022-11-28 10:21:02
---
# Uke 47
Denne uken jobbet vi med:
- Digitale spor 
- Lesestoff:
	- https://www.nrk.no/norge/xl/avslort-av-mobilen-1.14911685
	- https://developers.facebook.com/docs/marketing-api/audiences/reference/advanced-targeting/
	- Søkeord "Retail Heatmap"
	- <iframe width="560" height="315" src="https://www.youtube.com/embed/kBFMsY5ZP0o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
	- https://nrkbeta.no/2021/09/02/du-kan-spores-i-det-skjulte-av-hodetelefonene-dine/
	- Søkeord: "Bluetooth Beacon"
	- https://twitter.com/ElonJet
- Vi spillte 7 Billion Humans (selv om vi nå er [8 milliarder](https://www.nrk.no/urix/na-har-vi-blitt-8-milliarder-mennesker-pa-jorda-1.16179920)).
- Vi kodet noen skilpadder i [Turtle 🐢](https://oppgaver.kidsakoder.no/python/bokstaver/bokstaver), ved å opprette egne funksjoner for å stave navnet vårt

## Ukens Kode
Vi laget en funksjon som genererte teksten vi laget i forrige uke på en mer effektiv måte:
```python
def bestill_mattype(navn, topping):
    print("Vil du ha noe topping på din " + navn + "?")
    print("Vi har ulike toppinger: " + topping)
    svar = input("Ditt svar: ")
    print(f"En {navn} med {svar}, kommer straks.")
```

Her er det brukt to forskjellige metoder for å sette sammen variabler (`navn`, `topping` og `svar`) på, den ene er via matematikk (+), den andre er gjennom en `f-string`. Du kan lese om f-strings her: [https://realpython.com/python-f-strings/](https://realpython.com/python-f-strings/#f-strings-a-new-and-improved-way-to-format-strings-in-python)

Vi kan benytte forrige ukes kode for å ta i bruk den nye funksjonen:
```python
# start av kode
if __name__ == '__main__':
    print('Hva vil du ha?')
    print('1 for pannekake')
    print('2 for langpannekake')
    svar = input('Ditt svar:')
    if svar == '1':
        bestill_mattype('pannekake', 'vanlije og jordbær')
    elif svar == '2':
        bestill_mattype('langpannekake', 'krem og sjokolade')
    else:
        print('Velg mellom 1 og 2')
```

### Utfordring
Lag en funksjon der man kan si nei til topping på matvaren, ved å for eksempel si "Nei", eller "Nei takk".

Merk at det finnes uttallige måter å oppnå resultatet på.

> [!CODE]- Fasit - enkel
> ```python
> def bestill_mattype(navn, topping):
>     print("Vil du ha noe topping på din " + navn + "?")
>     print("Vi har ulike toppinger: " + topping)
>     svar = input("Ditt svar: ")
>     if svar == 'nei takk':
>         print(f"En {navn} uten topping, kommer straks.")
 >    else: 
 >        print(f"En {navn} med {svar}, kommer straks.")
> ```

> [!CODE]- Fasit - alternativ
> ```python
> def bestill_mattype(navn, topping):
>     print("Vil du ha noe topping på din " + navn + "?")
>     print("Vi har ulike toppinger: " + topping)
>     svar = input("Ditt svar: ")
>     # sjekk om svaret eksisterer i valg av topping
>     if svar in topping:
>         print(f"En {navn} med {svar}, kommer straks.")
 >    else: 
 >        print(f"En {navn} uten topping, kommer straks.")
> ```

### Ting å tenke på
Husk at brukere kan gjøre feil, og det er ikke sikkert de alltid gjør det de skal. Hva vil skje dersom brukeren skriver "alt, takk" eller "ja takk begge deler".

Å beherske brukerintensjoner er ikke enkelt - her programmerer vi et program der brukerflaten er en konsoll. I den virkelige verden kunne vi utviklet en touchskjerm meny med det vi har lært om HTML & CSS, for å oppnå en mer intuitiv opplevelse.
