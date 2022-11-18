---
title: Uke 46 - Google-Fu og Python
aliases: 
  - Uke 46
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-11-17 09:07:00
updated: 2022-11-18 13:29:23
---
# Uke 46
Denne uken jobbet vi med:
- Vi tok oss inn på dojoen og lære litt om [Google-Fu.](https://github.com/VaagenIM/files/blob/ikt/powerpoints/GoogleFu.pptx?raw=true)
- Vi så en dokumentar om Google søk og nysgjerrighet:
> [!TECH]+ Dokumentarvideo om Google Søk
> <iframe width="560" height="315" src="https://www.youtube.com/embed/tFq6Q_muwG0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
- Vi snakket litt om feilsøking av problemer, selvinnsikt og løsningsorientering
- Vi kodet Python, via Ozaria (CodeCombat) 🐍
- Vi koda Python i PyCharm, og lagde en enkel chatterobot.

## Dagens kode
```python
def bestill_pannekake():
    print(' ')
    print('Hvilken sirup vil du ha på panne kaken. ')
    print('Vi har Sjokolade, vanilje eller tyttebær.')
    svar= input('Svaret ditt:')
    print('En pannekake med', svar, 'sirup, kommer straks.')

def bestill_langpannekake():
    print(' ')
    print('Hvilken sirup vil du ha på lang pannekaken. ')
    print('Vi har Sjokolade, vanilje eller tyttebær.')
    svar = input('Svaret ditt:')
    print('En pannekake med', svar, 'sirup, kommer straks.')

# start av kode
if __name__ == '__main__':
    print('Hva vil du ha?')
    print('1 for pannekake')
    print('2 for langpannekake')
    svar = input('Ditt svar:')
    
    if svar == '1':
        bestill_pannekake()
    elif svar == '2':
        bestill_langpannekake()
    else:
        print('Velg mellom 1 og 2')
```

## Gjøremål
Alle **SKAL** ha installert en IDE som for eksempel JetBrains, og en Python Interpreter. Se [[Uke 42]] for instruksjoner.

Sjekk at oppsettet ditt virker med å prøve å kjøre koden:
```python
print("Hello Vågen!")
```

## Ukens innlegg
> [!WARNING]+ Is FAANG f**ked?
> Code report om Facebook, Apple, Amazon, Netflix & Google sine oppsigelser.
> <iframe width="560" height="315" src="https://www.youtube.com/embed/2pfcynxODJc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Utforskningslenker
- Ingen merkverdige ting å utforske denne uken
