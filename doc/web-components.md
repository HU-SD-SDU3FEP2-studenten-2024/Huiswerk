# Introductie (Vanilla) Web Components

## Welk probleem lossen Web Components op?

Een website bestaat uit content, opmaak en functionaliteit. De content wordt beschreven in HTML, de opmaak in CSS en de functionaliteit in JavaScript. Het updaten van de content, opmaak en functionaliteit van een website is vaak een complexe taak. Het wijzigen van een onderdeel kan vaak onverwachte gevolgen hebben voor andere onderdelen van de website. Web Components helpen dit probleem op te lossen door het mogelijk te maken om herbruikbare componenten te maken die bestaan uit HTML, CSS en JavaScript.

## Geschiedenis van Web Components

Google heeft in 2013 met de Polymer library de eerste aanzet gegeven voor Web Components om het boven geschetste probleem op te lossen. De Polymer library is een verzameling van polyfills en syntactic sugar die het bouwen van Web Components eenvoudiger maakt. Inmiddels zijn de meeste features van de Polymer library opgenomen in de standaard van de browser. Dit betekent dat je geen Polymer library meer nodig hebt om met JavaScript een Web Components te bouwen.

De features van het Polymer project die niet zijn opgenomen in de standaard van de browser zijn ondergebracht in de LitElement library. LitElement is een library die het bouwen van Web Components eenvoudiger maakt. LitElement is een library die is ontwikkeld door de mensen achter de Polymer library.

## Wat zijn Web Components?

Als je een website bekijkt dan zie je dat deze bestaat uit verschillende onderdelen zoals een header, footer, navigatiebalk, formulieren, knoppen, etc. Deze onderdelen worden vaak meerdere keren hergebruikt op verschillende pagina's van de website. Web Components zijn een manier om deze onderdelen te definiëren als herbruikbare componenten die bestaan uit HTML, CSS en JavaScript.
Je kunt dit Top Down benaderen doordat je de gehele pagina als een enkel component ziet en deze opdeelt in kleinere componenten. Of Bottom Up waarbij je begint met het maken van kleine componenten die je vervolgens samenvoegt tot grotere componenten.

## Wat zijn de voordelen van Web Components?

De voordelen van Web Components zijn:

* Herbruikbaarheid: Web Components zijn herbruikbare componenten die bestaan uit HTML, CSS en JavaScript.
* Isolatie: Web Components zijn geïsoleerde componenten die geen invloed hebben op de rest van de website.
* Onderhoudbaarheid: Web Components zijn eenvoudig te onderhouden omdat ze bestaan uit HTML, CSS en JavaScript.
* Compatibiliteit: Web Components zijn compatibel met alle moderne browsers.
* Schaalbaarheid: Web Components zijn schaalbare componenten die eenvoudig kunnen worden toegevoegd aan een website.

## Wat zijn de nadelen van Web Components?

De nadelen van Web Components zijn:

* Complexiteit: Web Components zijn complexe componenten die moeilijk te begrijpen zijn voor beginnende ontwikkelaars.
* Performance: Web Components kunnen de performance van een website negatief beïnvloeden omdat ze extra code bevatten.
* Browser support: Web Components zijn weliswaar onderdeel van de JS standaard maar worden niet door alle browsers ondersteund (met name een probleem bij oudere browsers zoals IE). Er zijn polyfills beschikbaar die dit probleem oplossen.

## Wat is het verschil met frameworks zoals Angular, React en Vue?

Angular, React en Vue kennen ook componenten maar deze zijn specifiek voor het betreffende framework. Web Components zijn onafhankelijk van een specifiek framework en kunnen worden gebruikt in combinatie met Angular, React en Vue.

De komst van Web Componenten maakt dat er een standaard is voor het maken van herbruikbare componenten die bestaan uit HTML, CSS en JavaScript. Dit betekent dat je niet langer afhankelijk bent van een specifiek framework om herbruikbare componenten te maken. Dit zou kunnen betekenen dat je in de toekomst weer een verschuiving ziet van frameworks naar standaarden zoals Web Components.

## Waaruit bestaan Web Components?

Web Components zijn een verzameling van standaarden die het mogelijk maken om herbruikbare componenten te maken die bestaan uit HTML, CSS en JavaScript. Web Components bestaan op dit moment uit drie standaarden:

* Custom Elements: een manier om je eigen HTML elementen te definiëren.
* Shadow DOM: een manier om een afgeschermd deel van de DOM te maken.
* HTML Templates: een manier om HTML te definiëren die niet direct in de DOM wordt opgenomen.

[:house:](../README.md)