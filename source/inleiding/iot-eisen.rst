*************************
Eisen aan de communicatie
*************************

We hebben hiervoor gezien welke functionele elementen je tegenkomt in een IoT-keten.
We proberen hier de eisen op te stellen voor de communicatie tussen deze elementen, voor een bepaalde toepassing.
Aan de hand van deze eisen bepaal je later welke (radio)verbinding je kunt gebruiken voor de rand ("edge") van de IoT-keten.

  Zoals we in de volgende hoofdstukken zullen zien,
  verschillen deze radio's in de grootte van de berichten,
  de snelheid waarmee deze verstuurd worden,
  het bereik van de radio, en de betrouwbaarheid van de communicatie.

  In deze module beperken we ons tot IoT-toepassingen met korte berichten.
  Voor toepassingen met "streaming" beeld of geluid heb je andere middelen nodig.

.. admonition:: Over signalen en events

  Een analoog signaal is continu: dit heeft op elk moment een waarde.
  Je kunt een analoog signaal bemonsteren (*sampling*) met vaste tussenpozen:
  als je deze tussenpozen klein genoeg kiest kun je met deze monster-waarden (*samples*) het oorspronkelijke signaal voldoende goed benaderen.
  Wat "klein genoeg" is hangt af van de snelheid van de veranderingen in het oorspronkelijke signaal.

    Voor muziek kies je als bemonsteringsfrequentie vaak 44,1kHz (Audio CD) of 48kHz (DVD);
    voor een ECG (electrocardiogram): 0,5-1kHz.

  Vaak ben je niet in de details van een signaal geïnteresseerd,
  maar alleen in bepaalde karakteristieke patronen of *events* die je in het signaal kunt herkennen.

  *Voorbeeld*: een cardioloog wil graag een volledig ECG-signal (electrocardiogram);
  een sporter heeft voldoende aan het aantal hartslagen per minuut.

  Een *event* is in dit verband een gebeurtenis of toestandsverandering die op een bepaald tijdstip plaatsvindt;
  de duur van deze gebeurtenis is niet van belang.
  Als de duur wel relevant is, kun je altijd gebruik maken van begin- en eind-events.

  In onze IoT-toepassingen gaat het vooral om events:
  gebeurtenissen waar je op moet reageren.
  *Voorbeeld*: je wilt graag weten wanneer de eerste persoon een kamer binnenkomt,
  en wanneer de laatste deze kamer verlaat (*events*).
  Dat zijn momenten dat de verlichting in- of uitgeschakeld moet worden.
  Het aantal personen in de kamer op een willekeurig moment (*signaal*) is daarvoor niet van belang.

  Uit de IoT-voorbeelden:

  - de bodemvochtigheid is een *signaal* dat we elke 5 minuten bemonsteren:
    snellere veranderingen komen niet voor of hebben geen betekenis.
  - het indrukken van een knop om een lamp in te schakelen is een *event*.

  Andere voorbeelden, buiten het bestek van deze IoT-module:

  * internet-telefonie: signaal
  * streaming video (internet tv, video on demand): signaal
  * streaming audio (internet radio, muziekdienst): signaal

Inputs en outputs
=================

Als eerste stap gaan we na wat de *inputs* (signalen en events) en *outputs* van de toepassing zijn.
Een input kan een enkele meetwaarde zijn van een enkele sensor;
maar een input kan ook het resultaat zijn van patroonherkenning op een reeks meetwaarden,
mogelijk van meerdere sensoren.
In dit stadium doen we daarover geen uitspraken.

  Het heeft voordelen om de patroonherkenning op de meetwaarden dicht bij de bron (sensoren) te doen.
  Door slimme *edge computing* of *fog computing* kun je voorkomen dat je grote hoeveelheden data naar de "cloud" moet communiceren.
  Bovendien kun je hierdoor vaker lokaal -en dus sneller en betrouwbaarder- reageren.

Inputs
======

Voor het nadenken over de inputs zijn de volgende vragen van belang:

- wat wil je meten? hoe vaak? wat is het waardenbereik? welke precisie is nodig?
- welke gebeurtenissen (events) zijn van belang? waar moet het systeem op reageren?

Het waardenbereik en de precisie hebben we nodig voor de grootte van de berichten.

We maken ons hier nog niet druk over de sensoren en de berekeningen voor deze signalen en events.

Voorbeelden:

- (event) indrukken van de "aan" knop van de lamp in de woonkamer;
- (event) uitschakelen van de lamp in de woonkamer op afstand;
- (signaal) meten van de kamertemperatuur: elke 2 minuten, met een nauwkeurigheid van 0,5 graden.

De temperatuur van de kamer verandert niet sneller, en tijdelijke veranderingen,
bijvoorbeeld door een deur die even open en dicht gaat, zijn niet van belang.

Outputs
=======

Ook bij outputs hebben we te maken met signalen en events.
Wij proberen hier vooral met *events* te werken, zoals het in- of uitschakelen van een lamp of van een pomp.

  Signalen gebruik je soms voor een *proportionele* regeling,
  bijvoorbeeld het dimmen van een lamp afhankelijk van het tijdstip en/of het niveau van het omgevingslicht.
  Dit soort regelingen probeer je dicht bij de outputs (actuatoren) te plaatsen,
  om snel te kunnen reageren, en om de hoeveelheid communicatie met de "cloud" te beperken.
  Ook dat is een voorbeeld van "fog" computing.

Voorbeelden:

* (event) het in- en uitschakelen van een lamp;
* (event) het instellen van de kleur en lichtsterkte van een lamp;
* (event) het in- en uitschakelen van een sproeicyclus.

Opdrachten
==========

 Werk dit uit voor een IoT-voorbeeld; kies hiervoor een eigen voorbeeld, of een voorbeeld uit de lijst (volgende sectie).

Communicatie-eisen
==================

 * wat is het bereik en de precisie van de waarden van de inputs en outputs? (Hiermee kunnen we de grootte van de berichten uitrekenen.)
 * wat is de maximale vertraging (latency) van een bericht, de maximale reactietijd op een event?
 * hoe betrouwbaar moet de communicatie van het bericht zijn? ("best effort" vs. "bericht van ontvangst" (ACK) en opnieuw verzenden)

Tijd: vertraging en reactietijd
===============================

Meten, communiceren, rekenen en beslissen kost tijd.
Elke toepassing heeft zijn eigen tijdseisen:

* wat is de maximale reactietijd op een bepaalde input-event?
* wat is de maximale vertraging (latency) tussen een meting en de aankomst van het bericht met de meetwaarde bij de controller?

*Opmerking*: de communicatie naar een lokale controller heeft veel minder latency dan naar een controller "in the cloud".

Voorbeelden:

- (event) indrukken van de "aan" knop in de woonkamer: lamp moet binnen 0,5 seconde aan gaan.
	- als de lamp niet "direct" aangaat denkt de gebruiker dat er iets kapot is; hij drukt dan bijvoorbeeld de schakelaar nog een aantal keren in.
- (event) lamp uitschakelen op afstand (internet-app): lamp moet binnen 15 seconden uitgaan.
	- de gebruiker is niet in dezelfde ruimte als de lamp, een directe reactie is dan niet nodig.
- (signaal): de gemeten kamertemperatuur moet binnen 60 seconden bij de lokale controller zijn die ook de verwarming aanstuurt.
	- de controller moet voldoende snel *feedback* hebben op het resultaat van de aansturing van de verwarming.

Betrouwbaarheid
===============

Communicatie is niet altijd betrouwbaar.
Dit geldt in het bijzonder voor radiocommunicatie:
deze kan op allerlei manieren (tijdelijk) verstoord raken.
Er zijn (ruwweg) twee manieren om berichten te versturen:

- *best effort*: het bericht wordt verstuurd in de verwachting dat dit *meestal* aankomt;
- *versturen met ontvangstbevestiging* (ACK): de ontvanger stuurt een ontvangstbevestiging;
  als de zender deze niet binnen een bepaalde tijd ontvangen heeft, verstuurt deze het bericht opnieuw.

Voor veel IoT-toepassingen is *best effort* voldoende,
(i) omdat een enkele meting wel gemist kan worden;
of (ii) omdat het resultaat van het bericht op andere manieren bevestigd wordt.

Voorbeeld: als de controller voor de watersproeier nog steeds dezelfde bodemvochtigheid meet,
kan deze de watersproeier nogmaals inschakelen.

.. admonition:: Idempotente opdrachten

  Een *idempotente* opdracht geeft hetzelfde resultaat als je deze eenmaal uitvoert of meerdere keren.
  Bij veel IoT-berichten is het wenselijk dat deze *idempotent* zijn:
  het maakt dan niet uit of deze eenmaal of vaker verstuurd, ontvangen en uitgevoerd worden.
  Bijvoorbeeld: het in- of uitschakelen van een lamp is een idempotente opdracht.
  Het instellen van het absolute lichtniveau van een lamp is dat ook.
  Maar het halveren van het lichtniveau is geen idempotente operatie.

  Vraag: welke opdrachten (knoppen) op de afstandsbediening van een TV zijn idempotent? Waarom? Welke zijn niet idempotent? Waarom is dat geen probleem?

  Vraag: is het opnieuw opvragen van een webpagina idempotent? Is het opnieuw versturen van een webformulier altijd idempotent?

  Vraag: is een tuimerschakelaar voor een lamp idempotent?

  Vraag: welke knoppen zitten er op een Hue dimmer? Welke zijn daarvan idempotent (en waarom)?

Voorbeelden:

* (signaal) bodemvochtigheid: best effort;
* (event) indrukken van de "aan"knop van de lamp in de woonkamer: best effort (waarom?);
* (event) op afstand uitzetten van de lamp in de woonkamer: bevestiging (ACK) vereist (waarom)?
* (signaal) temperatuur in de woonkamer: best effort.

Grootte van berichten
=====================

Sommige IoT-radio's, zoals LoRa, werken met erg kleine berichten: ca. 10-20 bytes.
Je moet dan wel weten of je berichten daarin passen.
Om uit te rekenen hoeveel ruimte een waarde voor een sensor of actuator nodig heeft,
in een binair formaat, kun je de volgende regels gebruiken:

* reken het waardenbereik om naar het geheeltallige (integer) bereik 0..x, waarbij elke stap relevant is.
  Dit doe je door schaling (vermenigvuldigen/delen) en verschuiven (optellen/aftrekken);
* voor 0..255 is 1 byte voldoende; voor 0..65536 2 bytes. (Je hebt vrijwel nooit meer nodig.)
* eventueel kun je ook in bits rekenen, als elke bit telt, maar dat laten we voorlopig buiten beschouwing.

Voorbeeld:

* temperatuur 0..50 graden Celsius, in stappen van 0,5 graad:
	* *zender*: vermenigvuldig met 2: verstuur waarde 0..100 (1 byte);
	* *ontvanger*: deel ontvangen waarde door 2.
* luchtdruk 950..1050, in 0,5 mbar (hPa) stappen:
	* *zender*: trek 950 af, en vermenigvuldig met 2: 0..200 (1 byte);
	* ontvanger: deel door 2, en tel er 950 bij.

Daarnaast heb je nog wat extra gegevens nodig, bijvoorbeeld om aan te geven over welke sensor of actuator het gaat,
en om welk soort waarde; reken op 2 bytes extra per sensor/actuator.

In sommige gevallen werken we met berichten in een tekstformaat (JSON):
de berichten worden dan ca. 10 maal zo groot.

Bereik en vermogen
====================

Afhankelijk van het soort toepassing heb je een radio met groter bereik nodig, of is een kleiner bereik voldoende.

IoT-radio's hebben in de regel een beperkt zendvermogen.
De gebruikte radioband eist vaak een (vrij klein) maximum zendvermogen.
En voor een mobiele IoT-knoop is "low power" van belang voor een grote levensduur van de batterij/accu.
Vooral voor mobiele IoT-knopen is dit van belang: de batterij is het grootste en zwaarste onderdeel.

Bij eenzelfde zendvermogen moet je kiezen tussen een groot bereik of een grote bitsnelheid (bitrate).

De radio's die wij in dit materiaal gebruiken hebben de volgende eigenschappen:


+-----------+-----------+-------------------------+---------------+
| **radio** | **power** | **bereik**              | **bitrate**   |
+-----------+-----------+-------------------------+---------------+
| WiFi      | medium    | lokaal bereik (10-50m)  | MBytes/s      |
+-----------+-----------+-------------------------+---------------+
| WiFi      | medium    | lokaal bereik (10-50m)  | Mbytes/s      |
+-----------+-----------+-------------------------+---------------+
| RFM69     | low       | lokaal bereik (50-200m) | 50kbits/s (*) |
+-----------+-----------+-------------------------+---------------+
| LoRa      | low       | niet-lokaal (enkele km) | 1 kbit/s (*)  |
+-----------+-----------+-------------------------+---------------+

(*) voor LoRa is de bitrate nog lager bij een groot bereik.
Bovendien mogen RFM69 en Lora-radio's max. 1% van de tijd zenden.


Veiligheid en privacy
=====================

IoT-toepassingen zijn vaak erg gevoelig:
je wilt niet dat anderen je sensorgegevens gebruiken,
of dat anderen je actuatoren kunnen aansturen.
De veiligheid van de verbindingen is dan van groot belang.
Deze kun je op verschillende niveaus waarborgen:
encryptie kan onderdeel zijn van de fysieke (radio)verbinding,
maar je kunt ook end-to-end encryptie toepassen,
op het niveau van de toepassing.

Samenvatting
============

Je kunt de eisen aan de communicatie voor een toepassing samenvatten in een tabel:

+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
| **input/output** | **signaal/ event** | **periode**| **latency**  | **bereik**   | **precisie** | **best effort?** |
+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
| I: temperatuur   | signaal            |      120 s |      60 s    | 0..50'C      | 0,5'C        | best effort      |
+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
| I: aan/uit knop  | event              |            |      0,5 s   | -            | -            | best effort      |
+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
| I: aan/uit (app) | event              |            |      120 s   | -            | -            | ACK              |
+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
|                  |                    |            |              |              |              |                  |
+------------------+--------------------+------------+--------------+--------------+--------------+------------------+
