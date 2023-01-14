---
title: Uke 2 - Classes og Enum
aliases: 
  - Uke 2
lang: nb-NO
authors:
  - Sondre Grønås
created: 2023-01-14 09:44:40
updated: 2023-01-14 10:50:10
---
# Uke 2
Denne uken jobbet vi med:
- Vi begynte en ny Python periode, link til GitHub Classroom finner du i Teams
- Vi opprettet noen python [[04 Classes|Classes]] (Trykk for å lese)
- Vi gikk litt rotete innom [[05 Enum, Identifisering|Enums]] (Trykk for å lese)

## Praktisk eksempler på klasser
Klasser er byggeblokker i programmering hvor en klasse, eller et objekt, kan inneholde flere funksjoner og verdier som kan kommunisere med hverandre og holde samme standard. Målet bør være å forenkle prosesser eller gjøre noe mer tydelig. Ofte fører det også til mer lesbar og forståelig kode.

Cohesion (https://en.wikipedia.org/wiki/Cohesion_(computer_science)) er nøkkelordet, hvor vi samler elementer som har med hverandre å gjøre og setter dem i system.

### Eksempel uten klasse
Se for deg at vi driver en nettside med kunder, hvor vi har et irriterende nyhetsbrev som går via både SMS og e-post. Vi lagrer også informasjon om kundene våre for å gjøre meldingene mer vennlige.

Vanligvis kunne vi lagret informasjonen i en [[02 Dictionaries|Dictionary]], som her:

```python
personer = {
	'0': {
		'navn': 'Ola Nordmann',
		'tlf': '12345678',
		'motta_sms': True,
		'epost': 'ola@nordmann.no',
		'motta_epost': False
	},
	'1': {
		'navn': 'Kari Svenske',
		'tlf': '12345678',
		'motta_sms': True,
		'epost': 'kari@svenske.no',
		'motta_epost': False
	}
}

def send_sms(melding, telefon_nummer):
	print(melding)
	print(f'Sendt til {telefon_nummer}')

def send_epost(melding, epost):
	print(melding)
	print(f'Sendt til {epost}')

# Kode begynner her:
for person in personer():
	if person['motta_sms']:
		send_sms('Test!', person['tlf'])
	if person['motta_epost']:
		send_epost('Test!', person['epost'])
```

Forsåvidt ingenting feil med koden over, men den har høy Coupling (https://en.wikipedia.org/wiki/Coupling_(computer_programming)), som vil si at funksjonene og variablene avhenger av hverandre for å fungere, og lavere cohesion - den er ikke "enkel" å forstå *helt* uten videre, selv om den ikke er så komplisert.

### Eksempel med klasse
Klasser krever litt mer "oppbygging", og kan ofte være mer abstrakt, man må tenke over hva man faktisk er ute etter. Det enkle er ofte det beste, som ofte har mest *cohesion*.

> [!NOTE]+ Dataclass
> Eksempelet under bruker `@dataclass` dekoratøren, denne forenkler prosessen av å lage en klasse betraktelig, ettersom vi ikke trenger å lage en `__init__` funksjon samt andre fordeler: 
> <iframe width="560" height="315" src="https://www.youtube.com/embed/vRVVyl9uaZc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

```python
from dataclasses import dataclass

@dataclass
class Kunde:
	navn: str
	tlf: str
	epost: str

	# Merk, følgende verdier har standardverdier!
	nyhetsbrev_sms: bool = False  # Skal standard være True?
	nyhetsbrev_epost: bool = False  # eller False? 🤔
	
	def __post_init__(self):
		# I post_init kan man kjøre kode når
		# objektet blir opprettet.
		self.send_nyhetsbrev('Du abonnerer nå på vår spam!')

	def send_sms(self, innhold):
		print(innhold)
		print(f'Sendt til {self.tlf}')

	def send_epost(self, innhold):
		print(innhold)
		print(f'Sendt til {self.epost}')

	def send_nyhetsbrev(self, tekst_sms, tekst_epost):
		if self.nyhetsbrev_sms:
			self.send_sms(tekst_sms)
		if self.nyhetsbrev_epost:
			self.send_epost(tekst_epost)

# Normalt ville vi fått denne informasjonen fra f.eks. en nettside
kunder = [
	Kunde('Ola Nordmann', 
		  '12345678', 
		  'ola@nordmann.no', 
		  nyhetsbrev_sms=True,
	),
	Kunde('Kari Svenske', 
		  '12345678', 
		  'kari@svenske.no', 
		  nyhetsbrev_sms=True,
		  nyhetsbrev_epost=True
	)
]

# Kode begynner her:
melding = 'Her er ditt daglige påfyll av spam!'
for kunde in Kunder:
	kunde.send_nyhetsbrev(tekst_sms=melding, tekst_epost=melding)
```

Begge kodene oppnår det samme resultatet, men koden som kommer etter `# Kode begynner her:` er enklere i eksempelet med en `Class`; den kan ikke misforståes, og vi trenger ikke bekrefte om de skal motta nyhetsbrev eller ikke.

### Fordel: utvidbart
Når man har gode `Classes`, så har man fundamentet for å kunne utvikle nesten hva som helst. APIer eller dokumentasjoner i de fleste applikasjoner for eksempel, bruker `Classes` for å by på egenutvikling.

Se for deg at vi må utvikle en funksjon for å slutte å abonnere på nyhetsbrevet, hvilken kode er mest forståelig?

Ikke-class:
```python
def unsubscribe_epost(person):
	person['motta_epost'] = False

# Kode:
unsubscribe_epost(person['0'])
```

Classes:
```python
@dataclass
class Person
	...
	motta_epost: bool = True

	def unsubscribe_epost(self):
		self.motta_epost = False

# Kode:
ola.unsubscribe_epost()
# Alternativ:
ola.motta_epost = False
```

## Utvikling i andre programmer (Modding)
`Classes` er overalt, her er noen eksempler på hvor du kan bruke classes til å oppnå noe i andre applikasjoner:
- **Minecraft**: lag dine egne blokker: https://fabricmc.net/wiki/tutorial:blocks#creating_a_custom_block_class (Språket er Java, ikke så pent å lese nødvendigvis)
- **Home Assistant**: Smarthus applikasjoner, styr noe i huset med en [gullfisk](https://www.youtube.com/watch?v=48-qOC4fCdk)? SMS når noe er i postkassen? https://developers.home-assistant.io/docs/core/entity
- **Discord**: Benytt deg av Discord sin `Class` i Python, lag en chatbot! https://discordpy.readthedocs.io/en/stable/intro.html

## Fredagens kode

> [!IMPORTANT]+ Kvaliteten på koden
> Koden er på ingen måter "god", men et eksempel på hvordan man lager en class, og hvordan man kan implementere funksjoner eller verdier som er til felles for et objekt. For eksempel kan en person ha verdiene `fornavn, etternavn, alder, fødselsdato, fødested, høyde, blodtype, nasjonalitet, etc.` som kan ligge inne i en `Person` klasse.
> 
> ## Eksempel
> Man kan utvide funksjoner eller andre verdier basert på verdiene inne i objektet, for eksempel så kan en slik klasse generere en verdi `fullt_navn` basert på `fornavn` og `etternavn`:
> ```python
> class Person:
>     def __init__(self, fornavn: str, etternavn: str):
>         self.fornavn = fornavn.capitalize()  # Konverter oLA til Ola
>         self.etternavn = etternavn.capitalize()
>         
>         # Ekstra verdier basert på gitte verdier
>         self.fullt_navn = f'{self.fornavn} {self.etternavn}'
>         self.initialer = f'{self.fornavn[0]}{self.etternavn[0]}'
>         self.fornavn_baklengs = self.fornavn[::-1]
>         
>  eksempel = Person('oLA', 'nordmann')
>  print(eksempel.fullt_navn)
>  # Ola Nordmann
>  print(eksempel.fornavn_baklengs)
>  # alO
> ```

Obs. det er tusenvis av måter å forbedre koden under på, nederst kan du lese om noen av dem.

```python title="main.py"
import random
from enum import Enum


class EngineType(Enum):
    ELECTRIC = 0
    DIESEL = 1
    PETROL = 2
    HYDROGEN = 3
    MÅNESTØV = 4
    FRITYROLJE = 5


class PricePerKm:
    ELECTRIC = 2
    DIESEL = 10
    PETROL = 12


class Bil:
    def __init__(self, brand, model, year,
                 engine_type):
        self.brand = brand
        self.model = model
        self.year = year
        self.engine_type = engine_type

        self.last_inspection = year
        self.reg_nr = self.generer_regnr()

    def generer_regnr(self):
        bokstaver = 'ABCDEFGHIJKLMNOPQRSTUVWXYZÆØÅ'
        bokstav1 = random.choice(bokstaver)
        bokstav2 = random.choice(bokstaver)
        tall = random.randint(10000, 99999)
        return bokstav1 + bokstav2 + str(tall)

    def inspect(self, year):
        # Print om bilen er gyldig eller ikke
        # Bilen er gyldig hvis last_inspection ikke er
        # eldre enn 2 år i forhold til "year"
        # Oppdater last_inspection
        self.last_inspection = year

    def kalkuler_reise(self, kilometer):
        if self.engine_type == EngineType.DIESEL:
            return kilometer * PricePerKm.DIESEL
        elif self.engine_type == EngineType.PETROL:
            return kilometer * PricePerKm.PETROL
        elif self.engine_type == EngineType.ELECTRIC:
            return kilometer * PricePerKm.ELECTRIC
        raise Exception("Feil motortype!")

    def __str__(self):
        return f'{self.brand} {self.model} ({self.year})'


def main():
    volvo = Bil('Volvo', '240', 1984, EngineType.DIESEL)
    tesla = Bil('Tesla', 'Model Y', 2023, EngineType.ELECTRIC)

    for bil in [volvo, tesla]:
        print(bil.kalkuler_reise(25))


if __name__ == '__main__':
    main()
```

```python title="karakter.py"
from dataclasses import dataclass


@dataclass
class Karakter:
    name: str
    health: int

    @property
    def is_alive(self):
        return self.health > 0

    def punch(self, skade, motstander):
        print(f'{self.name} slår {motstander.name} for {skade} skade.')
        ny_liv = motstander.health - skade
        print(f'{motstander.name} {motstander.health} -> {ny_liv}')
        motstander.health = ny_liv
        if not motstander.is_alive:
            print(f'{motstander.name} er død.')

bob = Karakter('Bob', 250)
mark = Karakter('Mark', 150)

bob.punch(50, mark)
bob.punch(99, mark)
mark.punch(200, bob)
bob.punch(1000, mark)
```

```python title="spill_class.py"
from enum import Enum
from dataclasses import dataclass

HOTBAR = 9
INVENTORY = 3 * 9

class ItemFlags(Enum):
    PLACEABLE = 0
    INTERACTABLE = 1
    STACKABLE = 2
    WEARABLE = 3
    CONSUMABLE = 4

class ArmorType(Enum):
    HELM = 0
    CHESTPLATE = 1
    LEGGINGS = 2
    BOOTS = 3
    SHIELD = 4

@dataclass
class Item:
    id: int
    name: str
    description: str
    flags: list[ItemFlags]
    count: int = 1
    max_stack: int = 64

    def interact(self):
        print(f'Kan ikke interacte med {self.name}.')


class InteractableItem(Item):
    def interact(self):
        print(f'Du har {self.count} {self.name}')
        self.count -= 1
        print(f'Du interagerte med {self.name}')
        if self.count == 0:
            print(f'Du er tom for {self.name}')
        else:
            print(f'Du har {self.count} {self.name} igjen.')

ITEMS = [
    Item(id=0,
         name='Luft',
         description='Ingenting',
         flags=[ItemFlags.STACKABLE]),
    InteractableItem(id=1,
                     name='Egg',
                     description='En egg',
                     flags=[ItemFlags.INTERACTABLE])
]

inventar = {
    '0': InteractableItem(
        id=1,
        name="Egg",
        description="Et egg",
        flags=[ItemFlags.INTERACTABLE,
               ItemFlags.CONSUMABLE],
        count=64
    ),
    '1': Item(
        id=0,
        name="Luft",
        description='Ingenting',
        flags=[ItemFlags.WEARABLE],
        count=2
    )
}

if __name__ == '__main__':
    current_item = '0'
    inventar.get(current_item).interact()

    current_item = '1'
    inventar.get(current_item).interact()
    print(inventar.get(current_item))
```

