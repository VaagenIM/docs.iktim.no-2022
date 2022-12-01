---
title: Uke 48 - API, Python
aliases: 
  - Uke 48
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-11-29 17:04:27
updated: 2022-12-01 09:23:20
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
	querystring = {"country":land,"day":"2020-06-02"} # (1)!
	
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
2. Her erstatter vi `"usa"` med vår nye parameter; `land`.

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
På fredag skal vi se på: `return` funksjone, `for` og `while` løkker. Vi skal også se litt mer på dataobjekter (Lister / Dictionaries).

Korte eksempler, bedre forklaringer kommer:

Return funksjonen:
```python
def test():
    return "Jeg returnerer tekst"

def summer(tall1, tall2):
    return tall1 + tall2

print(test())
print(summer(5, 3))

x = test()  # x får verdien fra test(), som blir returnert
print(x)

# Resultat:
# Jeg returnerer tekst
# 8
# Jeg returnerer tekst
```

Lister
```python
import random
min_liste = ['Hei!', 'Hei sveis!', 'Hallo!', 'Howdy!']

print(min_liste[0])  # Hei!
print(min_liste[2])  # Hallo!
print(random.choice(min_liste))  # Tilfeldig hilsen
```

Dictionaries, som lister, men med nøkler / navn
```python
min_dict = {
	'navn': 'Ola Nordmann',
	'alder': 19,
	'land': 'Norge'
}

print(min_dict['navn'])
print(min_dict['alder'])
print(min_dict['land'])

min_liste_med_dict = [
	{
		'navn': 'Jens Julenisse',
		'alder': 19,
		'land': 'Danmark'
	},
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

For løkker:
```python
# Går gjennom alle objekter i listen, og representerer dem med variabelnavn "person"
for person in min_liste_med_dict:
	print(person['navn'])

# F.eks. sammen med et API om restauranter:
for restaurant in liste_over_restaurantdata_fra_api:
	print(restaurant['name'] + " rating: " + restaurant['rating'])
```

While løkker:
```python
# mens sant = sant, (som alltid er tilfelle), vil programmet gjenta seg selv for alltid
while True:
	print("Kjører for alltid")
print("Kommer aldri her.")
```

Bruk av while løkker i chatbot:
```python
import random

# komma separert liste med tekststrenger
dadjokes = [
	'Where do fruits go on vacation? Pear-is!',
	'What does a lemon say when it answers the phone? Yellow!'
]

print("Velkommen til Chatbot 2.0")
print("Spør meg om hjelp via ?help, eller ?q for å avslutte")
while True:
    svar = input():
    if svar == "?q":
	    break  # avslutt while løkken
	elif svar == "?help":
		print("Funksjoner: ?dadjoke")
	elif svar == "?dadjoke":
		print(random.choice(dadjokes))

print("Hade!")
```

## Gjøremål
Snart er det på tide å definere vårt eget prosjekt som skal jobbes med frem til jul!

> [!TECH]+ Motivasjon
> <iframe width="315" height="560" src="https://www.youtube.com/embed/H4yqjCy0_Z8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
