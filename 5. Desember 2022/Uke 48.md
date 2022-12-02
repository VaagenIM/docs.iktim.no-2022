---
title: Uke 48 - API, Python
aliases: 
  - Uke 48
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-11-29 17:04:27
updated: 2022-12-02 22:22:26
---
# Uke 48
Denne uken jobbet vi med:
- Vi snakket om hvordan maskiner kommuniserer ([[HTTP Metoder]] / Responser, 404/403/418/200, [[GET]]/[[POST]]) 🕸
- Vi logget inn på https://rapidapi.com/hub med vår GitHub konto 💻
- Vi testet ulike APIer ved å velge `Python -> Requests` metoden 🐍
- Vi lærte å navigere i [[JSON]] objekter i Python 📊 
- Vi lærte litt om lister og dictionaries
- Vi så på hvordan vi kan lage et uendelighetsprogram gjennom en `while` loop/løkke ➰

> [!TECH]+ Fun fact om API - Discord Bots!
> APIer brukes overalt! På Discord for eksempel, kan du motta en API nøkkel for å logge på en chatterobot gjennom Discords egen API; https://discord.com/developers/applications.
> 
> <mark style="background: #FFF3A3A6;">Ja, det kreves litt lesing, forkunnskaper, feilsøking og prøving - men det er ikke umulig å få til!</mark>
> 
> Her vil man finne funksjoner i Python som lar deg lese meldinger eller discord handlinger, samt sende meldinger tilbake, gjennom Python!
> 
> Se https://realpython.com/how-to-make-a-discord-bot-python/ for mer informasjon!

> [!IMPORTANT]+ Ikke viktig å få med seg alt
> Når vi lærer å kode, så vil det nesten være umulig å forstå alt med en gang, dette er vanlig!
> 
> Det blir dessverre mange løse tråder, vår jobb er å vite littegrann om dem, slik at vi vet om mulighetene, uten å nødvendigvis forstå dem helt. Vi trenger å vite søkeord - "request weather data from api" er mye bedre søkeord enn "get weather python", selv om det helt sikkert fungerer.
> 
> Vær derfor ikke bekymret om ikke alt faller naturlig på plass i hodet, selv de beste må google de enkleste ting og lære seg dem på ny en gang i blant.

## Onsdags kode
På onsdag så vi på API'er på siden RapidAPI. API betyr Application Programming Interface, og er en metode for å kommunisere med en applikasjon via programmering. For eksempel i spillet Minecraft kan man referere til "blokker" via ulike metoder som objekter i programmering, da det er grensesnittet utviklerne har laget.

<mark style="background: #FF5582A6;">**API kan virke som et litt stort hopp fra forrige uker, læringskurven kan være stor, men mulighetene er enda større, teksten under er derfor litt lang.**</mark>

Vi så først på YR sin utviklerportal, hvor det står mye informasjon om hvordan man kan hente data-vennlig informasjon, men dette er ikke "nybegynner" vennlig: https://developer.yr.no/doc/GettingStarted/. RapidAPI er en mye enklere plattform for å benytte seg av APIer.

I eksempelet brukte vi Covid-19 APIet: https://rapidapi.com/api-sports/api/covid-193/. Merk at man er nødt til å abonnere på API'er for å motta en Token / Nøkkel, som er tilsvarende brukernavn og passord for å kunne benytte seg av tjenesten. <mark style="background: #FFF3A3A6;">Dersom prisen er på $0.00, behøver man ikke å taste inn betalingsinfo.</mark> Velg derfor Subscribe for å få en gyldig `X-RapidAPI-Key`.

![[subscribe-api.png]]

På fanen `Endpoints` får man ulike metoder å interagere med tjenesten, som vil variere fra ulike API'er. [[GET]] betyr at en henter informasjon, mens [[POST]] betyr at en sender. På fanen `Example Responses` vil man se hva slags informasjon de ulike endpointene inneholder. I Covid-19 tjenesten mottar man informasjon som antall nye tilfeller, antall smittede, antall tester, dato for oppdatert statistikk samt litt informasjon om landet (innbyggere, kontinent, etc.).

![[endpoints-c19.png]]

Når man har valgt Endpoints får man velge ulike parametre å gi til tjenesten på midten av siden. <mark style="background: #FFF3A3A6;">Dette er viktig, ettersom dette er parametre vi kan endre gjennom kode, for eksempel å velge land gjennom et brukergrensesnitt/chatterobot.</mark> For eksempel dersom man benytter seg av en vær-varsel API, vil det (nok) være ønskelig å kunne skrive inn ønsket by man vil sjekke været i.

Tjenesten generer automatisk en `Code Snippet` basert på programmeringsspråk. Standard er satt til `Node.js - Axios`, som er standard for webutvikling. <mark style="background: #BBFABBA6;">Vi skal bruke Python, med `Requests` som metode.</mark> 

![[codesnipper-api.png]]
### PyCharm
Opprett en ny fil som du navngir `api.py`, her kan du kopiere og lime inn code-snippeten. Vi skal gjøre noen modifikasjoner.

> [!CODE]- Installer Requests pakken
> Dersom du får en rød strek under `import requests`, betyr det at pakken `requests` ikke er installert. Som regel kan du høyreklikke på feilmeldingen, og velge `Install missing package`. Alternativt kan du åpne fanen `Python Packages` og søke etter `requests`, og installere den fra der.
> 
> Eventuelt må du åpne et konsollvindu/terminal, og skrive inn følgende:
> ```bash
> pip install requests
 > ```

Øverst i filen plasserer man `imports`, annen kode som kommer fra andre steder. Her må vi importere `requests`, som i snippeten.

```python
import requests
```

I snippeten er det generert kode for oss, men skal kun brukes for å teste endpointet. Vi ønsker å konvertere den til en funksjon, der vi har mer kontroll over bruken:

```python
# Start av funksjon
def min_funksjon():
	url = "https://covid-193.p.rapidapi.com/history" # (1)!
	
	querystring = {"country":"usa","day":"2020-06-02"} # (2)!
	
	headers = {
		"X-RapidAPI-Key": "SIGN-UP-FOR-KEY", # (3)!
		"X-RapidAPI-Host": "covid-193.p.rapidapi.com"
	}
	
	response = requests.request("GET", url, headers=headers, params=querystring) # (4)!
	
	print(response.text) # (5)!

# Bruk funksjonen
min_funksjon()
```

1. Endpointet, hvor dataen kommer fra
2. Vår "query", hva kan vi spørre tjeneren om? Her kan vi velge `country` og `day`
3. X-RapidAPI-Key er vårt brukernavn OG passord, denne får vi av RapidAPI når vi abonnerer på API'et. Uten denne får vi ingen tilgang.
4. Her lagrer vi resultatet vi får gjennom å kjøre `requests.request` funksjonen, `GET` er metode for innhenting, `url` er sted, `headers` inneholder vår token (innlogging / autentikasjon), `params` er de ulike parametrene vi har lov til å redigere.
5. Standard for å teste, skriver ut hele resultatet vi får fra serveren som tekst.

Dersom vi nå bruker funksjonen `min_funksjon()`, vil koden kjøre som normalt, og vi vil få resultatet printet til konsollen i tekst (`print(response.text)`). Dette er nyttig for å teste ut, men vi ønsker som regel å gjøre noe mer med informasjonen vi mottar.

På fredag går vi mer gjennom dataobjekter ([[JSON]] / [[02 Dictionaries|Dictionaries]]), som her kommer i form av en variabel: `querystring`. <mark style="background: #FFF3A3A6;">Query betyr spørring (etter informasjon), som er verdien vi må endre for å påvirke resultatene.</mark>

I eksempelet har vi to verdier, `country` og `day`. Ulike APIer krever ulik info. Hvis vi tenker oss at vi vil hente informasjon fra ulike land, er det verdien til `country` vi må endre:

```python
# Norge
querystring = {"country":"norway","day":"2020-06-02"}
# Sverige
querystring = {"country":"sweden","day":"2020-06-02"}
# Island
querystring = {"country":"iceland","day":"2020-06-02"}
```

Problemet nå er at vi oppdaterer querystring, der kun den siste definisjonen gjelder (island). Men, `country` er bare tekst, som betyr at vi kan erstatte det med en variabel, som kan defineres gjennom funksjonen.

Vi kan derfor gjøre om på funksjonen vår ved å endre parametre inn til funksjonen, som er strukturert med: `def <funksjon_navn>(<kommaseparerte parametre>):`

```python hl_lines="5 9 21 22 23"
# import av moduler
import requests

# Start av funksjon, parameter = land
def min_funksjon(land): # (1)!
	url = "https://covid-193.p.rapidapi.com/history"
	
	# country = verdi av land, gitt av parameter
	querystring = {"country":land,"day":"2020-06-02"} # (2)!
	
	headers = {
		"X-RapidAPI-Key": "SIGN-UP-FOR-KEY",
		"X-RapidAPI-Host": "covid-193.p.rapidapi.com"
	}
	
	response = requests.request("GET", url, headers=headers, params=querystring)
	
	print(response.text)

# Bruk funksjonen, med ulike land
min_funksjon("norway")
min_funksjon("sweden")
min_funksjon("iceland")
```

1. Funksjonen tar nå inn en parameter; `land`.
2. Her erstatter vi `"usa"` med vårt nye parameter; `land`.

Hurra 🥳 - nå har vi en funksjon som henter data, basert på et eksternt parameter (land)!

> [!CODE]- Bonus: velg ut informasjon fra responsen
> I eksempelet over skrives hele resultatet fra APIen ut som tekst, som ikke nødvendigvis er det vi ønsker, kanskje vi vil ha en app som viser antall innbyggere i ulike land? Da behøver vi kun den biten av informasjonen fra tjenesten.
> 
> Vi skal utforske JSON verdier, eller Python Dictionaries som de også heter i mer detalj på fredag.
> 
> Her er et eksempel på hvordan du kan hente ut `population` verdien til resultatet:
> 
> I `Example Responses` delen av RapidAPI, kan man se hvordan response `body`-en ser ut, som er objektet vi får (`response` variabelen). Her kan vi se alle nøklene som inneholder ulik informasjon, og kan notere oss hvilke som er relevante. Her kan vi se at under `response` nøkkelen, finner man `0`, hvor vi finner nøkkelen `population`:
> 
> ![[c19-population-exampleresponse.png]]
> 
> For å velge ut denne, må vi konvertere response variabelen til en `JSON` verdi. Vi navigerer gjennom JSON ved å bruke firkant-braketter (`[ ]`), med tekst/nummer for å få verdien til den bestemte nøkkelen. Merk at en nøkkel, kan inneholde flere nøkler, som betyr gjentagelse av `[ ]` symbolene:
> 
> ```python
> # Gammel kode i funksjonen:
> # print(response.text)
> 
> data = response.json()
> population = data['response'][0]['population']
> 
> # Husk å konvertere tall (population) til tekst
> print(land + ", antall innbyggere: " + str(population))
> ```

## Fredags kode
På fredag gikk vi gjennom mye rart! Mange dataobjekter (Lister og Dictionaries/JSON), løkker og match/case. Merk at det kan nok føles som mye, men man må nesten hoppe i det! ***Ikke vær redd for å google, vi kommer til å gjøre mange feil - og det er faktisk en positiv ting.***

### Funksjoner - Dad joke funksjoner
> [!TECH]+ Video om å lage funksjoner fra ArjanCodes:
> <iframe width="560" height="315" src="https://www.youtube.com/embed/yatgY4NpZXE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Vi laget noen funksjoner i en `util.py` fil (Utilities), som vi importerer til en annen fil (`bot.py`). En funksjon er noe som gjør noe, og kan returnere en verdi basert på det vi ønsker. Vi programmerte en funksjon som gir oss en tilfeldig dad-joke, og en funksjon som lar oss legge til flere dad-jokes.

```python title="util.py"
import random # (1)!

# Liste over dadjokes
dadjokes = [
    "Hva spiser frukter på ferie, Pæreis",
    "Jeg er redd for kalenderen, dens dager er talte.",
    "En blind mann går inn i en bar, og en bord og en stol."
]

def get_dadjoke() -> str: # (2)
	"""Returns a random dad joke""" # (3)!
	# dadjokes = load_dadjokes() (4)
	return random.choice(dadjokes)

def add_dadjoke(joke: str): # (5)
	dadjokes.append(joke)
	# save_dadjokes(dadjokes) (6)
```
1. `random` modulen er en pakke som lar oss generere tilfeldigheter, f.eks. tilfeldig tall, eller objekt i liste
2. `-> str` forteller utvikleren / PyCharm om hva funksjonen generer, som er tekst (`return <tekst>`)
3. Dette er en docstring, som er en forklaring på hva funksjonen gjør til mennesker som leser den
4. Hypotetisk funksjon, vi kunne ha lastet inn dadjokes fra et API, eller fra en tekstfil
5. `joke: str` betyr at verdien som blir sendt inn i funksjonen (joke) må være en streng/tekst
6. Hypotetisk funksjon, vi kunne ha lagret nye vitser til f.eks. en tekstfil.

> [!CODE]- Ekstra `return` eksempel
> ```python
> def test():
 >    return "Jeg returnerer tekst"
> 
> def summer(tall1, tall2):
 >    return tall1 + tall2
> 
> print(test())
> print(summer(5, 3))
> 
> x = test()  # x får verdien fra test(), som blir returnert
> print(x)
> 
> # Resultat:
> # Jeg returnerer tekst
> # 8
> # Jeg returnerer tekst
> ```

Koden i `util.py` gjør ingenting når man kjører den, men man kan importere funksjonene der vi skal ha vår chatbot, i en fil som heter `bot.py` i samme mappe.
```python title="bot.py"
from util import * # (1)!

# Onboarding
navn = input('Hei, hva heter du? ')
print(f'Hyggelig å treffe på deg, {navn}. Start med å skrive ?help')

# Start av chatbot
while True: # (2)!
	match input('$: '): # (3)!
		case '?help': #(4)!
			print('Jeg kan fortelle deg dadjokes, skriv ?joke')
		case '?joke':
			print(get_dadjoke()) #(5)!
```
1. Importer alle funksjoner fra `util.py`, slik at vi kan bruke dem
2. `while True:` er en løkke som vil gjenta seg for alltid (til programmet avsluttes), eller til koden treffer på en `break` funksjon
3. `match` funksjonen lar deg enkelt sammenligne en verdi med ulikt utfall. Den fungerer på nesten samme måte som `if input() == '?help'`, men har mange fordeler.
4. `case` er koden som vil kjøre dersom teksten matcher input fra bruker
5. Her bruker vi funksjonen fra `util.py`, `get_dadjoke()`!

### Lister
Lister er en av de store datatypene i Python som defineres av firkanta braketter (`[]`) og inneholder flere verdier i en og samme variabel. Se [[03 Tuples og lister|Tuples og lister]]. I eksempelet over, har vi laget en liste over dadjokes. Objekter i en liste er separert med komma.

```python
import random
min_liste = ['Hei!', 'Hei sveis!', 'Hallo!', 'Howdy!']

print(min_liste[0])  # Hei!
print(min_liste[2])  # Hallo!
print(random.choice(min_liste))  # Tilfeldig hilsen

for hilsen in min_liste:
	print(hilsen)  # Printer alle de unike hilsene en etter en, i en løkke.
```

### Dictionaries / Ordlister
Dictionaries er det viktigste og største dataformatet vi har, og kalles også ofte for [[JSON]]. (Se også [[01 Dataformater|Dataformater]]). Det er i dette formatet vi blant annet jobber med APIer eller eksterne ressurser. Ordlister defineres med curly brackets (`{}`).

I motsetning til vanlige lister som over, definerer man verdier med nøkler / navn. Eksempler på nøkler kan være brukernavn, passord, e-post og lignende, i en liste over brukere. Merk at verdiene til nøkler kan være nye ordlister eller lister, som kan by på mange muligheter!

Ved hjelp av `for` løkker kan man også gå gjennom alle enheter i en liste/ordliste, og utføre handlinger på flere datasett på samme tid. Nedenfor finner du et eksempel som gjør litt av hvert.

```python
min_dict = { # (1)!
	'navn': 'Ola Nordmann',
	'alder': 19,
	'land': 'Norge'
}

print(min_dict['navn'])
print(min_dict['alder'])
print(min_dict['land'])

min_liste_med_dict = [ # (2)!
	{
		'navn': 'Jens Julenisse',
		'alder': 19,
		'land': 'Danmark'
	}, # (3)!
	{
		'navn': 'Kari Svenske',
		'alder': 18,
		'land': 'Sverige'
	}
]

print(min_liste_med_dict[1]['navn'])  # Kari Svenske

# Legg til info i en dict:
min_dict['ny-nøkkel'] = 'hallo'
print(min_dict['ny-nøkkel'])  # hallo

# Legg til i en liste
min_liste_med_dict += min_dict
# Legger til "Ola Nordmann" i listen med Jens Julenisse og Kari Svenske
```
1. Ordlister / dictionaries defineres av curly-brackets `{}`, der nøkkel/verdi settene skilles ved komma.
2. Du kan også lage lister som inneholder ordlister! Nyttig dersom man har en liste over for eksempel brukere
3. Komma separer enhetene! Det er viktig!

### For løkker
For løkker er **UTROLIG** viktige og nyttige, de lar oss iterere (gå gjennom en og en) verdier i lister, eller ordlister. Merk at navnet som settes mellom for/in (`for <X> in`) , er valgfritt.
```python
# Går gjennom alle objekter i listen, og representerer dem med variabelnavn "person"
for person in min_liste_med_dict:
	print(person['navn'])

# F.eks. sammen med et API om restauranter:
for restaurant in liste_over_restaurantdata_fra_api:
	print(restaurant['name'] + " rating: " + restaurant['rating'])
```

### While løkker
While løkker er gjentagende kode som stopper når tilstanden ikke lenger er sann. Ofte bruker man en while løkke som sjekker om `True` er sant, som det alltid vil være. Formålet med det er å definere kode som aldri stopper å kjøre. Dette gjelder alle applikasjoner som ikke stopper uten brukeren trykker stopp, som for eksempel i spill.
```python
# mens sant = sant, (som alltid er tilfelle), vil programmet gjenta seg selv for alltid
while True:
	print("Kjører for alltid")
print("Kommer aldri her.")
```

Bryt ut av en while løkke med `break`, eller når en tilstand er møtt:
```python
while True:
	print('Jeg bryter meg ut')
	break
print('Im free!')

i = 0
while i != 10:  # mens i ikke er 10
	i += 1  # legg til 1 på i
	print('i er enda ikke 10...')
print('Nå er i 10!')
```

### Dagens kode
Vi lagde 2 dokumenter, `bot.py` og `util.py`. Begge må ligge i samme mappe, slik at filene finner hverandre.
> [!CODE]- bot.py
> Her ligger det litt ekstra som ikke er gått gjennom over, men som vi gikk gjennom litt ekstra i timen. Det gjelder:
> 
> - Bruk av `subprocess` - et eksempel på at det går an å gjøre ting på maskinen, som å åpne en kalkulator app via en chatbot
> - Legge til ekstra nøkler/verdier i en ordliste (hysj = vær stille)
> - Legge til flere alternative skrivemåter i en `match/case` del via `|` tegnet.
> - En funksjon (`roast_me(navn, alder)`) som sender informasjon fra bot.py til en util.py funksjon
> 
> ```python title="bot.py"
> from util import *
> import subprocess
> 
> kommandoer = {
>     '?joke': 'Jeg forteller en dårlig dad-joke.',
>     '?addjoke': 'Legg til en vits i dadjokes listen.',
>     '?roastme': 'Bli roastet.',
>     'quit': 'Avslutt samtalen.',
> }
> 
> kommandoer['hysj'] = "vær stille."
> 
> # Onboarding
> navn = input("Hva heter du? ")
> if not navn:
>     raise ValueError
> print("Hei, " + navn)
> alder = input("Hvor gammel er du? ")
> 
> # Intro
> print("Skriv ?hjelp for å komme i gang.")
> # Start robot
> while True:
>     match input("$: "):
>         case "?hjelp":
>             print("Jeg kan følgende kommandoer:")
>             for kommando in kommandoer:
>                 print(kommando + " - " + kommandoer[kommando])
>         case "?joke":
>             print(get_dadjoke())
>         case "?addjoke":
>             joke = input("Skriv vitsen: ")
>             add_dadjoke(joke)
>             print("Da er den lagt til.")
>         case "?roastme":
>             do_roast(navn, alder)
>         case 'calc':
>             subprocess.call('calc')
>         case 'quit' | 'exit' | 'q' | 'avslutt' | '?q' | '?quit':
>             break
> ```

> [!CODE]- util.py
> Inkluderer en ekstra funksjon, `roast_me(navn, alder)` som har en punchline over tid ved å bruke `time` modulen. 
> 
> ```python title="util.py"
> import random
> import time
> 
> dadjokes = [
>     "Hva spiser frukter på ferie, Pæreis",
>     "Jeg er redd for kalenderen, dens dager er talte.",
>     "En blind mann går inn i en bar, og en bord og en stol."
> ]
> 
> def get_dadjoke() -> str:
>     """Returns a random dad joke"""
>     # load_jokes()
>     return random.choice(dadjokes)
> 
> def add_dadjoke(joke: str):
>     dadjokes.append(joke)
>     # save_jokes()
> 
> def load_jokes():
>     print("lastet inn (hypotetisk)")
> 
> def save_jokes():
>     print("lagret (hypotetisk)")
> 
> def do_roast(navn, alder):
>     roasts = [
>         f'Hvem er {alder} år og hører på Justin Bieber?',
>         f'Hva står på 2 bein og bråker i timen?',
>     ]
> 
>     print(random.choice(roasts))
>     time.sleep(0.5)
>     print('..')
>     time.sleep(0.5)
>     print('...')
>     time.sleep(0.5)
>     print(navn + '!!!!')
> ```

## Gjøremål
> [!TECH|inline end]+ Motivasjon
> <iframe width="150" height="280" src="https://www.youtube.com/embed/H4yqjCy0_Z8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Frem til jul skal dere definere et Python prosjekt, det kan være å opprette funksjoner på chatteroboten din, til å for eksempel hente data gjennom et API, eller legge til andre spenstige ting, som å kunne spille stein/saks/papir med en robot!

Du trenger ikke å forholde deg til chatteroboten din om du har andre ideer. Om du ønsker å teste andre språk - så må du gjerne det! Du finner oppgavetekst med litt inspirasjon på Teams.

<mark style="background: #FFF3A3A6;">Husk å last opp endringene dine til GitHub underveis, og eventuelt legg til informasjon om planene dine i README.md!</mark>

