# Bits & Bytes

De kans is groot dat je - als software developer (of toekomstige) - dat je al van bits & bytes hebt gehoord.  
Denk hierbij maar aan:

* De grootte van een file of opslagmedium (kilobytes, megabytes, ...)
* Processoren van 64 bit
* Een RAM-geheugen van 8 gigabyte
* De codering van IP-adressen
* RGB-kleurensysteem
* ...

## Computerhardware kent enkel bits en bytes

Om te weten waarom bits & bytes zo belangrijk zijn moeten we gaan kijken naar computerhardware.  

* TODO: transistors als schakelaars
* Von Neumann...

## Getallenstelsels

### Cijfers vs getallen

De eenvoudigste manier om bits te begrijpen is door ze te vergelijken met iets dat je kent, namelijk **cijfers**.  

Een (decimaal) getal bestaat uit verschillende cijfers die numerieke waarden tussen 0 en 9 kan bevatten.  Deze cijfers worden dan gecombineerd om grotere getallen te creëren.  

### Volgorde vs waarde

Bijvoorbeeld het getal 1234 bestaat uit 4 cijfers: 1, 2, 3 en 4.  
De waarde die dit getal heeft wordt bepaald door dat de volgorde van deze cijfers.  

Zo weten we dat 3124 groter is dan 1234 ondanks het feit dat dit uit dezelfde cijfers bestaat.

### Decimale getallen-stelsel

Dit kan je wiskundig heel gemakkelijk uitrekenen door elk cijfer te vermenigvuldigen
met 10-tallen.

~~~
  (1 * 1000) + (2 * 100) + (3 * 10) + (4 * 1)
=    1000    +    200    +    30    +    4

  (3 * 1000) + (1 * 100) + (3 * 10) + (4 * 1)
=    3000    +    100    +    20    +    4
~~~

maw Je vermenigvuldigt telkens met een hogere macht van 10


Vandaar dat we het getallenstelsel decimaal noemen.  
De **deci** in decimaal betekent **10**, elke **positie** - van rechts naar links - stelt een
hogere macht van 10 voor

* het meest rechtse cijfer wordt vermenigvuldigd met 10 tot de 0de macht
* het 2de van rechts met de 1ste macht
* het 3de met de 2 macht
* ...

zoals je hieronder kan zien:

~~~
  (1 * 10^3) + (2 * 10^2) + (3 * 10^1) + (4 * 10^0)
=    1000    +    200     +     30     +    4

  (3 * 10^3) + (1 * 10^2) + (2 * 10^1) + (4 * 10^0)
=    3000    +    100     +    20      +    4
~~~

## Van decimaal naar een binair getallenstelsel...

Bovenstaande zou nog in je comfortzone moeten zitten uit het lager en middelbaar onderwijs...  
Decimale getallen zijn voor ons ook heel logisch, de mens heeft leren werken met het decimale
tallenstelsel gezien wij 10 vingers hebben.

![](5vingers.jpeg)

Stel dat we - zoals de 4 vingers zouden hebben zouden een 8-tallig tallenstelsel hebben

![](simpsons.png)

Dat zou allemaal redelijk comfortabel moeten aanvoelen: we werken elke dag met decimale cijfers. Het leuke van getalsystemen is dat er niets is dat je dwingt om 10 verschillende waarden in een cijfer te hebben. Ons getallenstelsel met grondtal 10 is waarschijnlijk ontstaan ​​omdat we 10 vingers hebben, maar als we in plaats daarvan zouden evolueren naar acht vingers, zouden we waarschijnlijk een getalstelsel met grondtal 8 hebben. Je kunt een getalsysteem met een grondtal hebben. Er zijn zelfs veel goede redenen om verschillende bases in verschillende situaties te gebruiken.

Computers werken toevallig met het 2-tallige getallenstelsel, ook wel het binaire getallenstelsel genoemd (net zoals het 10-tallige getallenstelsel bekend staat als het decimale getallenstelsel). Ontdek waarom en hoe dat werkt in de volgende sectie.


## Waarom is dit belangrijk

TODO

## Waarom heb je bits en bytes nodig?

* Files (ascii, UTF)
* Netwerk (IP-)adressen
* RBG-codes

## Tallenstelsels

## Hardware & logischepoorten