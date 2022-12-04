---
title: Uke 49 - Python Eksempler
aliases: 
  - Uke 49
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-12-03 09:09:48
updated: 2022-12-03 10:21:07
---
# Uke 49
Denne uken jobbet vi med:
- Valgfri koding

<iframe width="560" height="315" src="https://www.youtube.com/embed/brffDCE5hXs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Eksempler på kode for ulike nivå
Nå skal dere definere et prosjekt som dere kan jobbe med, og prøve å lære mer på egenhånd. Men husk - vi er kun i vg1, så ikke legg opp høye forventninger - det gjør ikke vi heller 😄.

Nedenfor finner du noen eksempler på løsninger til kode på ulike nivåer, fra enkelt til avansert. I vg1 forventer vi leveringer av den enkle varianten, men den brutale sannheten er at man må tvinge oss til å lære de mer avanserte metodene, og bli vant til å jobbe med ting vi ikke føler oss kvalifiserte for. <mark style="background: #FFF3A3A6;">Problemløsning og feilsøking er førsteprioritet.</mark>

Mye av eksemplene som blir vist begynner med enkle metoder, men utvikler seg til et sted mellom avansert og komplisert, *ikke vær redd for å forholde deg til den enkle metoden!* Det betyr ofte at koden kan bli uoversiktlig og vanskelig å jobbe med over tid, men ikke at den ikke fungerer!

### Chatbot løkker (While loops)
En chatbot kjører kontinuerlig, om det er i Discord, eller i terminalen, eller som en NPC i et spill, spiller ingen rolle. *De gjør alle det samme: lytt på en hendelse, og responder deretter.*

#### Enkel
```python
def adder():
    print('La oss plusse tall!')
    tall1 = input('Tall 1: ')
    tall2 = input('Tall 2: ')
    total = int(tall1) + int(tall2)
    print(tall1 + ' + ' tall2 ' = ' str(total))

print('Skriv ?hjelp for å starte, ?quit for å avslutte')
while True:
	query = input()
	if query == '?hjelp':
		print('Jeg kan følgende kommandoer:')
		print('- ?hjelp: Vis tilgjengelige kommandoer')
		print('- ?adder: Legg sammen to tall')
		print('- ?quit: Avslutt programmet')
	if query == '?adder':
		adder()
	if query == '?quit':
		break
```

#### Avansert: Pattern matching
Pattern matching tillater oss å gjøre mye ekstra dill, for enkelhetens skyld er dette tilsvarende av koden over, som ikke tar i bruk de største fordelene med pattern matching.

```python
print('Skriv ?hjelp for å starte, ?quit for å avslutte')
while True:
	match input('$: '):
		case '?hjelp':
			print('Jeg kan følgende kommandoer:')
			print('- ?hjelp: Vis tilgjengelige kommandoer')
			print('- ?adder: Legg sammen to tall')
			print('- ?quit: Avslutt programmet')
		case '?adder':
			adder()
		case '?quit':
			break
```

> [!TECH]+ Hvorfor pattern matching?
> <iframe width="560" height="315" src="https://www.youtube.com/embed/scNNi4860kk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> [!CODE]- Eksempel på mer avansert bruk av pattern matching
> ```python
> match input('$: ').split():
> 	case ['download', url, 'please']
> 		print(f'Downloading {url}')
> 	case ['download', url]:
> 		print(f'Say please...')
> 	case ['adder', *nums]:
> 		total = 0
> 		for num in nums:
> 			total += int(num)
> 		print(total)
> 	case ['?quit' | 'exit' | 'q' ]:
> 		break
> 
> # 'sum 5 6 8' -> 19
> # 'download https://www.youtube.com/watch?v=dQw4w9WgXcQ' -> Say please...
> # 'download https://www.youtube.com/watch?v=dQw4w9WgXcQ please' -> Downloading...
> ```

#### Komplisert
Selv om denne er "komplisert", kan komplisert kode ofte være lettere å forstå seg på. En må litt være komfortabel med språket, og gode nøkkelord er decoupling og cohesion. Decoupling og cohesion betyr å bryte opp koden i mindre, velfungerende biter, samt gjøre dem mer menneskelesbar og forståelige.

Merk at selv om koden er 20 ganger lengre, så gjør den til syvende og sist det samme som de over - her hadde det vært ideelt og fordelt de ulike funksjonene til ulike python dokumenter for å holde prosjektet ryddig, for eksempel funksjonene i `util.py`, og ordlistene samt `run_command` i `bot.py`.

> [!CODE]- "Komplisert" kode, lang
> Obs: Merk at denne koden er på ingen måte "god" den heller, men veldig generell og verbose. Her finnes ingen fasit.
> 
> ```python
> import sys  # sys.exit() is called when the user types 'quit' or 'exit'  
> import time  # time.sleep() is used to simulate a delay when downloading a file  
> from functools import reduce  # reduce() is used to add or subtract multiple numbers  
>   
> # A dict of all available commands and their descriptions, used by the display_commands function  
> # Why not just put this in the display_commands function? Because it's more readable to have a separate variable,  
> # and it's easier to change  
> commands_help = {  
>     'help': 'display this help message',  
>     'hello': 'display a friendly greeting',  
>     'quit': 'exit the program',  
>     'add': 'add two or more numbers',  
>     'subtract': 'subtract two or more numbers',  
>     'download _url_': 'download a file from the internet',  
> }  
> # A dict of all available commands and their corresponding functions, used by the run_command function  
> # For commands that don't take any arguments, we can just use a lambda function to call the function directly, e.g. 'quit': lambda: quit_bot()  
> # This is a bit of a hack, but it's a lot easier than creating a separate function for each command that doesn't take any arguments  
> # This works well for simple commands, but for more complex commands, it's better to use a separate function  
> simple_commands = {  
>     'help': lambda: display_commands(),  
>     'hello': lambda: send_message('Hi there!'),  
>     'quit': lambda: quit_bot('Bye!'),  
> }  
>   
>   
> def quit_bot(msg: str = '') -> None:  
>     """Exits the chatbot program"""  
>     # Why not use sys.exit() directly? Because we might want to change how the program exits in the future  
>     if msg:  
>         send_message(msg)  
>     sys.exit(0)  
>   
>   
> def send_message(message: str) -> None:  
>     """Prints a message to the console"""  
>     # Why not use print() directly? Because we might want to change how messages are displayed in the future  
>     # I.e. we might want to add a timestamp or a username, or send the message to a server instead of printing it  
>     print(message)  
>   
>   
> def display_commands() -> None:  
>     """Displays all available commands"""  
>     send_message('Available commands:')  
>     for command, description in commands_help.items():  
>         send_message(f'{command} - {description}')  
>   
>   
> def cmd_add(numbers: list) -> None:  
>     """Adds two or more numbers"""  
>     # Why not just put this code in the run_command function? Because it's more readable to have a separate function  
>     if not numbers:  
>         send_message('Please provide at least two numbers to add')  
>         return  # return early to avoid running the rest of the function  
>   
>     total = reduce(lambda x, y: int(x) + int(y), numbers)  
>   
>     send_message(f'The sum of {" + ".join(numbers)} is {total}')  
>   
>   
> def cmd_subtract(numbers: list) -> None:  
>     """Subtracts two or more numbers"""  
>     if not numbers:  
>         send_message('Please provide at least two numbers to subtract')  
>         return  # return early to avoid running the rest of the function  
>   
>     total = reduce(lambda x, y: int(x) - int(y), numbers)  
>   
>     send_message(f'The difference of {" - ".join(numbers)} is {total}')  
>   
>   
> def cmd_download(url: str) -> None:  
>     """Downloads a file from the internet"""  
>     # This is just a placeholder function for now  
>     send_message(f'Downloading {url}')  
>     time.sleep(1)  
>     send_message('Download complete')  
>   
>   
> def run_command(command: str) -> None:  
>     """Takes an input command and runs it if it exists in the simple_commands dict, or matches a pattern"""  
>     # Why run_command instead of just putting this code in the main loop? Because it's more readable to have a separate function  
>   
>     # If the command is in the simple_commands dict, run the corresponding function  
>     if command in simple_commands:  
>         return simple_commands[command]()  
>   
>     # If the command matches a pattern, run the corresponding function  
>     match command.split():  
>         case ['add', *numbers]:  
>             cmd_add(numbers)  
>         case ['subtract', *numbers]:  
>             cmd_subtract(numbers)  
>         case ['download', url]:  
>             cmd_download(url)  
>         # If the command doesn't match any pattern, display an error message  
>         case _:  
>             send_message('Unknown command')  
>   
>   
> while True:  
>     # Get user input, and send it to the run_command function  
>     user_input = input('> ')  
>     # Why not just put this as part of the pattern matching? Because it's more readable to have a separate function  
>     # I.e. we might want to use a GUI instead of the console, or use voice recognition instead of typing, etc.  
>     # Connecting to a server to get the user input is also a possibility in the future this way, such as Discord bots  
>     run_command(user_input)  
>
> ```
