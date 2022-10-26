---
title: Uke 36 IKT - HTML, Hero
aliases: 
  - Uke 36 IKT
lang: nb-NO
authors:
  - Sondre Grønås
created: 2022-09-10 21:25:15
updated: 2022-10-26 11:53:43
---
# Uke 36 IKT
Denne uken jobbet vi med:
- Vi snakket om en Hero / landingsside og Call To Action ([awebco.com](https://www.awebco.com/blog/hero-section/#:~:text=your%20own%20webpages.-,What%20is%20a%20Hero%20Section%3F,Why%20people%20should%20trust%20you))
- Vi så på følgende nettsider:
	- https://www.arngren.net/
	- https://www.awwwards.com/
	- https://www.behance.net/search/projects/?search=hero+section
- Vi lærte litt om [[06 Design og CSS|CSS]] (https://www.w3schools.com/css/)
- Vi lærte om inline styling i HTML: `<h1 style="color:red">Rød</h1>`
- Vi lærte om CSS selectors (https://www.w3schools.com/cssref/css_selectors.asp); `h1{}` selekterer alle `<h1>` tagger.
- Vi lærte å legge til CSS i `<head>` taggen av et dokument:
```html
<html>
<head>
	<style>
	h1{
		color: red;
	}
	</style>
</head>
<body>
	<h1>Rød</h1>
</body>
</html>
```
- Vi lærte å legge til et eksternt CSS dokument:
```html title="index.html"
<html>
<head>
	<link rel="stylesheet" href="style.css">
</head>
<body>
	<h1>Rød</h1>
</body>
</html>
```
```css title="style.css"
h1{
	color: red;
}
```
- Vi lærte om plassering av elementer og bakgrunner
```css title="style.css"
body{
	background-color: blue;
}

h1{
	color: red;
	position: absolute;
	top: 50%;
	left: 20%;
}
```
- Vi var litt innom html `class` og `id` data-attributter og hvordan man kan bruke dem til å selektere etter navn i CSS; **vi skal gjenta dette senere.**
- Vi var såvidt innom publisering av nettsider via [GitHub Pages](https://pages.github.com/).

Dagens kode og side: https://github.com/VaagenIM/hero-demo-v1
Web: https://vaagenim.github.io/hero-demo-v1/

## Gjøremål
Du skal kunne legge til en ekstern CSS dokument på en HTML side og skal minst endre ett element via CSS.

Veiledning: https://www.w3schools.com/css/css_howto.asp

> [!NOTE] Husk!
> Man lærer alltid best dersom man er nysgjerrig og prøver ut nye ting på egen hånd. Spør onkel Google! Alt er mulig, alt er lov.
> 
> I HTML kan ting gjøres på tusen forskjellige måter, det er vanlig å gjøre feil; det er faktisk *slik man lærer*.

## Fremtidige gjøremål
Vi skal se på hvordan vi lager seksjoner / inndelinger i HTML, slik at vi kan flytte samlete elementer, istedenfor individuelle elementer. - Dette er mye bedre praksis og er ofte enklere å jobbe med.

> [!code]- Et fremtidig blikk inn i seksjoner / inndelinger
> En section eller inndeling lages via `<section>` eller `<div>` tagger (se under). HTML har også noen innebygde navn for seksjoner; `<header>` og `<footer>`:
> ```html title="index.html"
> <html>
> <head>
> 	<title>Min Side</title>
> 	<link rel="stylesheet" href="style.css">
> </head>
> <body>
> 	<header>
> 		<a href="/"><img src="logo.png"></a>
> 		<div id="menu">
> 			<a href="#">Link 1</a>
> 			<a href="#">Link 2</a>
> 			<a href="#">Link 3</a>
> 		</div>
> 	</header>
> 	<section id="content">
> 		<h1>Lorem Ipsum</h1>
> 		<p>Dolor sit amet...</p>
> 	</section>
> 	<footer>
> 		<h3>Copyright Ola Nordmann 2022</h3>
> 	</footer>
> </body>
> </html>
> ```
> Via CSS kan man selektere individuelle seksjoner og endre for eksempel plassering, uten å påvirke innholdet i seksjonen
> ```css title="style.css"
> header{
> 	position: fixed; /* På toppen av siden, følg scrollbar til siden */
> }
> #content{
> 	background-color: green;
> 	padding: 20%; /* Elementene på INNSIDEN av seksjonen skal ha 20% innrykk på siden (marg i boka) */
> }
> footer{
> 	margin-top: 20%; /* Skal plasseres 20% lenger ned enn forrige element (UTSIDEN) (#content) */
> 	background-color: blue;
> }
> ```



Vi skal også fullføre vår Hero Section / landingsside, og lenke til indre del av siden, som vi skal jobbe med etterpå.

> [!IMPORTANT] Målet fremover
> Vi skal ha lært nok om HTML og CSS til å kunne eksperimentere selv og kunne utvikle vårt eget nettsted. 
> 
> Nettsiden kan handle om deg hvor du eventuelt viser frem ting du har gjort på skolen, eller den kan handle om noe du er interessert i. 
> 
> Den kan også handle om noe helt annet, selvfølgelig.

