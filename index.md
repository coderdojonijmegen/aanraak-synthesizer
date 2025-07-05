---
title: "Aanraakgevoelige synthesizer"
date: 2025-07-05T15:51:01+02:00
draft: false
toc: true
headercolor: "teal-background"
onderwerp: CircuitPython 
---

Maak een synthesizer die geluid maakt als je een potloottekening aanraakt (met een Raspberry Pi Pico en CircuitPython).

<!--more-->

Om dingen aanraakgevoelig te maken, gebruiken we dit groene printplaatje, een Pico-microcontroller. Laten we beginnen met het ingebouwde LED-lampje van de Pico te laten knipperen.

![[images/pico.jpg]]

De Raspberry Pi Pico is een **microcontroller**, wat betekent dat het een heel eenvoudige computer is. Je kunt er inputs op aansluiten, zoals lichtsensoren, en outputs zoals motoren en lampen.

Dit schema toont de 'pinout' van de Pico. Het vertelt je welke pinnen je kunt gebruiken en wat hun nummers zijn.

![[images/pinout.png]]


### Software installeren

De Pico die je voor je hebt draait op Circuitpython. Dat is een smaakje Python voor microcontrollers. 

Thonny is een programma op je computer waarmee je kunt programmeren in de programmeertaal Python.

![[images/thonny.png]]

Download het op [thonny.org](http://thonny.org): kies de juiste versie voor jouw computer door in de rechterbovenhoek Windows, Mac of Linux te selecteren. Download het bestand, installeer het en open vervolgens het programma.

Typ deze regel in het bovenste venster van Thonny:

```Python
print('Hallo wereld!')
```

Klik vervolgens op de groene afspeelknop:
![[images/groene-knop.png]]

Nu wordt de code uitgevoerd. Het resultaat verschijnt in het onderste deel van het Thonny-venster.


### Je eerste Pico-programma

Nu gaan we met behulp van Thonny code op het Pico-bordje uitvoeren in plaats van op je computer.

Sluit de Pico aan op je computer via een USB-kabel.

Selecteer 'CircuitPython (Raspberry Pi Pico)' in de rechteronderhoek van het Thonny-scherm. (Als je die optie niet ziet in de lijst, controleer dan of je Raspberry Pi Pico goed is aangesloten.)

Wanneer je nu code uitvoert, zal het op de Pico worden uitgevoerd. Kopieer deze code naar Thonny en klik op de groene afspeelknop:

```Python
import board
import digitalio
led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT
led.value = True
```

Het lampje op de Pico zou nu aan moeten gaan. Tenzij het al aan was! In dat geval kun je het uitzetten door de laatste regel te veranderen:
```Python
led.value = False
```

Je kunt ook de LED steeds laten knipperen zonder te wachten tot je het programma opnieuw uitvoert. Voeg deze regels toe aan de vorige code en voer het uit:
```Python
import time
while True:
	led.value = True
	time.sleep(2)
	led.value = False
	time.sleep(2)
```

Zorg ervoor dat je **ook de witruimte** overneemt (4 spaties), want dat is belangrijk in Python.

Kun je het LEDje ook tien keer zo snel laten knipperen?


### Een pin van de Pico aanraakgevoelig maken

Nu gaan we kijken of we de LED aan kunnen laten gaan als we een tekening aanraken. 

Aan de zijkanten van de Pico zitten veel pinnetjes die we kunnen gebruiken om allerlei elektronische onderdelen aan te sluiten, zoals sensoren of motortjes.

De Pico is vastgeklikt in wat een breadboard heet (het witte ding met allemaal kleine gaatjes).

De gaatjes op het breadboard zijn onderling verbonden in rijen van vijf, zoals de oranje rechthoek in het schema aangeeft.

Als we draadjes en onderdelen met elkaar willen verbinden, hoeven we ze alleen maar in gaatjes op dezelfde rij te steken.

De gaatjes langs de rode en blauwe lijnen zijn verticaal met elkaar verbonden. Rood staat voor plus en blauw voor min, net als bij een batterij.

Haal de USB-kabel uit je computer voordat je verder gaat.

In de volgende stappen maak je alle verbindingen precies zoals in het schema:

![[images/Pico_synth_GP15.png]]


Pak een kabeltje met een krokodillenklem aan het eind. Steek het pinnetje in het breadboard, vlak naast het laatste pinnetje aan de linkerkant van de Pico.

Steek één pootje van een weerstand (dat gestreepte dingetje) in dezelfde rij.

Steek het andere pootje van de weerstand in een van de gaatjes naast de **blauwe** lijn.

Gebruik een draadje om beide blauwe kolommen van het breadboard met elkaar te verbinden. Zorg ervoor dat je het NIET in een gaatje naast de rode lijn steekt!

### 🖍️ Maak je interactieve tekening!

Maak nu een tekening. Het mag een simpele vorm zijn. Zorg dat de tekening tot aan de rand van het papier gaat, zodat je er een krokodillenklem op kunt aansluiten, zoals in dit voorbeeld:

We gebruiken een 9B grafietpotlood, dat is zacht en geleidt goed.
Breng een dikke laag grafiet aan. Als je lijnen te dun zijn, maakt de tekening geen goede verbinding met de microcontroller.

Als je klaar bent, klem dan de krokodillenklem die uit je breadboard komt vast op het papier.

### 🔌 Start je programma – Zet het LEDje aan met aanraking

Open je bestand in Thonny en zet daar onderstaande code in (de uitleg met # ervoor hoef je niet over te nemen): 

```Python
import time
import board
import touchio
import digitalio
from adafruit_debouncer import Debouncer

# Wacht tot alles gestabiliseerd is
time.sleep(1.0)

# Stel GP15 in als aanraakpin
touch_pin = touchio.TouchIn(board.GP15)
button = Debouncer(touch_pin)

# Zet het ingebouwde LEDje klaar
led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

while True:
    button.update()
    
    if button.rose:
        led.value = True
        
    if button.fell:
        led.value = False

```

Start deze code op de Pico door in Thonny op de start-knop te drukken.

Als hij stroom krijgt, zou het LED-lampje op de Pico een paar keer moeten knipperen. Als dat niet gebeurt, controleer dan of alle draden in de juiste rijen zitten.

Je zou nu je tekening moeten kunnen aanraken en zien dat het LED-lampje op het bordje verandert!

✅ **Test het**: Als je je tekening aanraakt, gaat het ingebouwde LEDje aan!

  

### Meer aanraakgevoelige tekeningen toevoegen

Laten we twee extra aanraakgevoelige gebieden aan je kunstwerk toevoegen.

Voeg eerst twee sets weerstanden en kabels toe aan het breadboard, zoals in dit schema.

De nieuwe weerstanden verbinden de 7e (GP5) en de 12e pin (GP9) met de blauwe lijn. (Die nummers staan ook op het breadboard.)

Teken nu de extra ‘knoppen’. Je tekening mag alles zijn, zolang er maar vier aparte delen zijn in totaal. Elk deel moet een verbinding hebben met de rand zodat je er een krokodillenklem op kunt aansluiten (het makkelijkst is als ze allemaal aan dezelfde rand beginnen). Je mag de lijnen tekenen zoals je wilt, zolang er maar minstens een vingerbreedte tussen de vier delen zit.

Als je de tekeningen hebt gemaakt, maak je de krokodillenklemmen eraan vast.



### 🎵 Nu mèt geluid!

We kunnen je interactieve kunstwerk interessanter maken door een extra uitgang toe te voegen: geluid!

We gaan een kleine versterkte speaker op de Pico aansluiten.

Neem twee kabels met aan de ene kant krokodillenklemmen en aan de andere kant male connectors.

Verbind één krokodillenklem met de bovenkant van de stekker van de speaker en één met het onderste deel, zoals dit >>

Neem het andere uiteinde van de kabel die aan de tip van de stekker zit en steek die in het breadboard, in de rij naast de onderste rechterpin van de Pico.

Steek de andere kabel in een gaatje naast de blauwe lijn.

Zet de speaker aan.

Voeg onderstaande code toe aan je script. Zet eerst dit stuk bovenaan:

```Python
import audiopwmio
import synthio
import asyncio

audio = audiopwmio.PWMAudioOut(board.GP16)
synth = synthio.Synthesizer(sample_rate=22050)
audio.play(synth)

async def threeArpeggios():
    notes = [196, 294, 392, 494, 587, 220, 330, 440, 523, 659, 262, 392, 523, 659, 784]
    for i in range(len(notes)):
        note = synthio.Note(notes[i])
        synth.press(note)
        await asyncio.sleep(0.03)
        synth.release(note)
        await asyncio.sleep(0.01)
        
        if i % 5 == 4:
            await asyncio.sleep(0.2)
```

Pas dan je `while True:` aan:

```Python
while True:
    button.update()

    if button.rose:
        led.value = True
        asyncio.run(threeArpeggios())

    if button.fell:
        led.value = False

```

✅ **Test**: Raak je tekening aan → hoor een melodie én zie het LEDje branden!

## 🎵 Nog meer interessante geluiden

Voeg deze regels toe bovenaan in je script:

```Python
touch_pin2 = touchio.TouchIn(board.GP9)
touch_pin3 = touchio.TouchIn(board.GP5)

button2 = Debouncer(touch_pin2)
button3 = Debouncer(touch_pin3)
```

Voeg deze functies toe:

```Python
async def arpeggio():
    for j in range(10, 124, 3):
        synth.press(j)
        await asyncio.sleep(0.02)
        synth.release(j)
    for j in range(124, 10, -3):
        synth.press(j)
        await asyncio.sleep(0.02)
        synth.release(j)

async def bleepUp():
    for i in range(12, 120, 12):
        synth.press(i)
        await asyncio.sleep(0.02)
        synth.release(i)
```

Vervang de `while`-loop in je script door deze versie:

```Python
while True:
    button.update()
    button2.update()
    button3.update()

    if button.rose:
        led.value = True
        asyncio.run(threeArpeggios())
    if button.fell:
        led.value = False

    if button2.rose:
        asyncio.run(arpeggio())

    if button3.rose:
        asyncio.run(bleepUp())
```

Je zou nu verschillende geluiden moeten horen als je je tekening aanraakt!


## 🌈 Voeg kleur toe met de Neopixel LED

Laten we een andere output proberen: een LED met verschillende kleuren! Die kan reageren met verschillende kleuren op elk apart deel van je tekening!

De Neopixel LED heeft vier pootjes. Je moet ze misschien een beetje buigen om ze in het breadboard te krijgen.

Haal één van de draden van de batterij uit het breadboard (zorg dat ze tijdens het werken niks raken, ook de Pico niet).

Houd de LED voor je zodat **het langste pootje het derde is van links**. Dit is belangrijk want als je de LED verkeerd in het breadboard steekt dan kan hij stukgaan.

Steek de LED nu in het breadboard zodat:

- het eerste (linker) pootje in dezelfde rij zit als pin 14 van de Pico (GP21)
- het tweede pootje in de rode kolom zit
- het derde (langste) pootje in de blauwe kolom zit
- het laatste pootje gebruiken we niet. Buig dat gewoon om zodat het uitsteekt over de rand.

Controleer goed dat de pootjes van de LED elkaar niet raken.

Sluit de USB-kabel weer aan.

Zet bovenaan in je script deze regels:
```Python
import neopixel
from rainbowio import colorwheel

neopixel_led = neopixel.NeoPixel(board.GP21, 1, brightness=0.4)

class Colours:
    def __init__(self, initial_colours):
        self.values = initial_colours
```

Voeg Neopixel-code toe (ook boven `while True:`):

```Python
colours = Colours([0, 0, 0, 0])

async def changeNeopixel(np, colours):
    while True:
        if colours.values[3] != 0:
            neopixel_led.fill( colorwheel((time.monotonic()*90)%255) )
        else:
            neopixel_led.fill((colours.values[0], colours.values[1], colours.values[2]))
        await asyncio.sleep(0)

```

Voeg kleuren toe aan je knoppen:
```Python
    if button.rose:
        led.value = True
        colours.values[3] = 1
        asyncio.run(threeArpeggios())

    if button.fell:
        led.value = False
        colours.values[3] = 0

    if button2.rose:
        colours.values[0] = 30
        asyncio.run(arpeggio())

    if button3.rose:
        colours.values[2] = 50
        asyncio.run(bleepUp())
```

✅ **Laatste stap hier**: Voeg aan het eind van je script toe:

```Python
asyncio.run(changeNeopixel(neopixel_led, colours))
```

Als je dit script draait en dan je aanraakgevoelige tekening aanraakt, zou je kleuren moeten zien!


## ✅ Klaar!

Vond je het leuk om je tekening aanraakgevoelig te maken? Dan zul je blij zijn te horen dat je nog veel meer dingen interactief kunt maken door ze aanraakgevoelig te maken.

Je kunt je eigen geleidende verf maken en op van alles aanbrengen. Je kunt ook kopertape, metalen draad, aluminiumfolie of zelfs geleidende stof gebruiken.

Water en metalen voorwerpen geleiden ook elektriciteit, dus die werken ook goed.
Planten en bloemen bevatten veel water, dus zelfs die kun je aanraakgevoelig maken!

Naast al deze mogelijke inputs, is er ook een wereld aan mogelijke outputs. Je kunt een animatie op een tekening of schilderij projecteren, zoals in het eerdergenoemde kunstwerk Awake.

Of je kunt een video laten starten en stoppen als mensen iets aanraken, of lampen aan- of uitzetten, of iets mechanisch laten bewegen, of een rookmachine starten…


{{< licentie rel="http://creativecommons.org/licenses/by-nc-sa/4.0/">}}
