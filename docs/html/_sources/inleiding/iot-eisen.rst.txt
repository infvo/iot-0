*************************
Eisen aan de communicatie
*************************

.. bij de Inleiding

.. rubric:: Het Internet of Things stelt eigen eisen aan de internet-communicatie

Het Internet of Things verbindt de fysieke wereld van de "dingen" met de virtuele wereld van het internet.
Voor de verbinding met de fysieke wereld van een "ding" zorgen sensoren en actuatoren,
als onderdeel van een IoT-knoop gekoppeld aan dat "ding".
Deze IoT-knoop is vaak draadloos verbonden met het internet, bijvoorbeeld via een *gateway*.
De IoT-knopen en gateways vormen de rand (*edge*) van het Internet of Things.

In deze module ligt de focus op deze edge: de IoT-knopen en gateways,
en de communicatie daartussen.
De eisen aan de IoT-knopen en de bijbehorende communicatie volgen voor een deel uit de fysieke eigenschappen van de toepassing op het niveau van de gebruiker.
Met andere woorden: je kunt als gebruiker deze eisen beschrijven, zonder kennis van de IoT-technologie.

We beschrijven in dit gedeelte de soorten eisen die van belang zijn.
Later behandelen we hoe je op grond van deze eisen de passende IoT-technologie kunt kiezen.

.. rubric:: snelheid: bitrate

De eerste soort eis heeft te maken met de snelheid van communicatie, gerekend in bits per seconde (bitrate).
Veel sensoren en actuatoren, bijvoorbeeld voor temperatuur of lichtniveau, werken met waarden van 1 of 2 bytes.
Bovendien hoef je die waarden vaak maar enkele keren per uur te versturen.

Op basis van de sensoren en actuatoren die je gebruikt,
en van de eigenschappen van het "ding" (hoe snel verandert dit?),
kun je dan een inschatting maken van het aantal berichten dat je per uur nodig hebt,
en van hun grootte (in bytes).

  Er zijn ook sensoren en actuatoren die een veel grotere bandbreedte nodig hebben.
  Voorbeelden daarvan zijn camera's en microfoons, displays en luidsprekers.
  In deze module laten we dergelijke sensoren en actuatoren buiten beschouwing:
  "streaming" audio en video vormen een aparte categorie toepassingen.

.. rubric:: latency

De tweede soort eis heeft ook met snelheid te maken.
Hierbij gaat het om de latency: de tijd dat "iets onder water is".
Bij communicatie is de latency de tijd tussen het versturen en het ontvangen van een bericht.
Meer in het algemeen gaat het bij het IoT-toepassingen om de totale reactietijd.
Wat is de minimale reactietijd voor een reactie op een gebeurtenis?
Denk bijvoorbeeld aan een IoT-toepassing voor het besturen van de verlichting in je huis:
als je op de knop drukt om de verlichting in te schakelen,
dan wil je "bijna onmiddellijk" (binnen 200 ms) de lampen zien reageren.
Maar als je de verlichting automatisch of op afstand in- of uitschakelt,
dan mag die reactie misschien wel na enkele seconden plaatsvinden.

Voor IoT-toepassingen met alleen sensoren ("monitoring") is vaak een latency van enkele seconden acceptabel.
Als er actuatoren bij betrokken zijn, maakt het vaak een groot verschil of deze lokaal of op afstand bestuurd worden;
en of deze besturing automatisch of door mensen gebeurt.

.. rubric:: betrouwbaarheid

Sommige fysische processen verlopen langzaam: denk bijvoorbeeld aan de verandering van de temperatuur in een woonkamer.
Dit betekent dat je de sensoren die dit proces bewaken niet zo vaak hoeft uit te lezen.
Bovendien mag je best eens een meting missen: hooguit heeft dat tot gevolg dat de verwarming of de airconditioning een paar minuten later aan gaat.
We kunnen dan voor de communicatie volstaan met "best effort".
Het is niet nodig om de ontvangst van elk bericht te bevestigen.

Eén van de vragen die je als gebruiker moet onderzoeken is dan ook:
hoe betrouwbaar moet de communicatie zijn?
Hoe erg is het als er eens een meting of een aansturing verloren gaat?

.. rubric:: energie en mobiliteit

De "dingen" waar we in het IoT over spreken verschillen sterk.
Soms hebben we te maken met grote apparaten met een permanente stroomvoorziening.
In zo'n geval speelt de grootte, het gewicht en het energieverbruik van de IoT-knoop niet zo'n grote rol.
In andere gevallen hebben we te maken met kleine en mobiele "dingen" (bijvoorbeeld een huisdier):
dan spelen deze factoren wel een grote rol.

Bij mobiele "dingen" speelt de energiezoorziening een grote rol:
hiervoor gebruiken we meestal een batterij.
Bij voorkeur gebruiken we een kleine batterij of accu,
die we vaak hoeven te vervangen of op te laden.
Dit laatste is vooral van belang als we met veel IoT-knopen te maken hebben:
het is niet handig als je regelmatig honderden batterijen in de IoT-knopen in je huis moet vervangen.

Dit geeft de volgende vragen voor de gebruiker:

* hoe groot en zwaar mag de IoT-knoop zijn?
* hoe mobiel moet deze knoop zijn?
* over welk gebied is deze knoop mobiel?
  Hoe groot moet het bereik van een radio zijn om dit "ding" te kunnen volgen?
* hoe groot en zwaar mag een batterij zijn?
* wat moet de minimale levensduur van een batterijlading zijn?
* hoe vaak mag je die batterij opladen of omwisselen?

.. rubric:: veiligheid

.. todo::

  * eisen aan de security (en privacy)

* veiligheid is essentieel - zowel vanwege de veiligheid van de "dingen" als van de privacy van mensen in hun omgeving.
    * vooral bij besturingstoepassingen is dit van belang
    * (opmerking: het IoT kan ook bijdragen aan de fysieke veiligheid...)

Een ander soort eisen heeft te maken met veiligheid (security) en privacy.
Het Internet of Things kan erg "invasief" zijn in de omgeving van mensen.
Aan de hand van informatie over de "dingen" is veel op te maken over het gedrag van mensen in de omgeving daarvan.
Sommige IoT-toepassingen stellen daarom hoge eisen aan de *privacy*.

In andere gevallen hebben we te maken met veiligheideisen:
een apparaat mag alleen bediend worden door personen (of programma's) met de juiste autorisatie.

Beveiligde verbindingen vormen een essentieel middel voor zowel veiligheid als privacy.
Daarnaast zijn er nog aanvullende maatregelen nodig: zowel veiligheid als privacy zijn systeem-issues,
die niet alleen op een technologische manier opgelost kunnen worden.

Als gebruiker of opdrachtgever moet je van te voren nadenken over de eisen op het gebied van privacy en veiligheid,
zowel van jezelf als van anderen.
Hierbij moet je bedenken dat de nadelen van een IoT-toepassing soms andere mensen treffen dan de voordelen.

.. rubric:: Voorbeeld(en)

Eisen aan de communicatie
-------------------------

We gaan in een volgend hoofdstuk dieper in op de manier waarop we de verschillende onderdelen kunnen verbinden.
Op basis van kennis van de toepassing kunnen we al wel de eisen aan de communicatie formuleren.
Enkele voorbeelden van eisen:

* bandbreedte (bitrate): voor de toepassing kunnen we bepalen hoeveel berichten we in een bepaalde tijd willen sturen,
  en wat de omvang van die berichten is (in bits of in bytes);
* latency (vertraging): in het bijzonder bij een besturingstoepassing moeten we ervoor zorgen
  dat de tijd tussen het signaleren van een *event* (gebeurtenis) door een sensor en
  het aansturen van de actuator(en) niet te groot is:
  de maximale reactietijd wordt bepaald door de snelheid van het (fysieke) proces dat je aanstuurt.
* draadloos of bedraad: veel toepassingen eisen dat de verbinding tussen de sensoren/actuatoren en de controller draadloos is:
  draden belemmeren bijvoorbeeld de plaatsing of de beweging van de sensoren, en daarmee van het "ding" waaraan deze sensoren gekoppeld zijn.
* andere eisen, bijvoorbeeld beveiliging en privacy;
  je wilt bijvoorbeeld niet dat een besturing door anderen overgenomen kan worden.

In het voorbeeld van de sproeier hebben we de volgende eisen:

* bandbreedte (bitrate):
    * bodemvochtigheid: 1 bericht per 5-10 minuten, 1 byte per bericht;
    * actuator: 1 bericht per 30 minuten(?), 1 byte per bericht;
    * temperatuursensor: 1 bericht per 5-10 minuten, 1 byte per bericht;
    * regensensor: 1 bericht per 5-10 minuten, 1 byte per bericht;
* latency: deze is niet kritisch (minuten), het fysische proces van besproeien van een grasveld is langzaam.
* draadloos of bedraad: de verbinding tussen de controller, de actuator(s) en de sensoren is bij voorkeur draadloos.

.. admonition:: Hoeveel bits heb je nodig?

  Bij het bepalen van het aantal bits (of bytes) voor een sensormeting of een actuator-aansturing
  moet je weten (i) wat het *bereik* is, en (2) wat de vereiste *precisie* is.

  Bijvoorbeeld: je wilt temperatuur meten in het bereik -20..50 (Celcius),
  met een precisie van 0,5 graad.
  Je gebruik dan het bereik -40..100, waarbij je dit getal later door 2 deelt.

  Voor een getal in het bereik 0..255 heb je 8 bits nodig (1 byte).
  Je kunt dit bereik ook verschuiven, bijvoorbeeld -128..127, of -100..155.
  Het aantal bits blijft dan gelijk.
  Je kunt het bereik ook schalen (de komma verschuiven), bijvoorbeeld 0,0..25,5 (Celsius).
  Vaak is het bij deze schaling handiger om door een macht van 2 te delen,
  dan door een macht van 10.

  Je kunt ook met grotere getallen werken: 0..1023 (10 bits), 0..1000000 (20 bits), enz.

*klimaatbeheersing*

* per vertrek 1 of 2 IoT-knopen met sensoren voor de temperatuur en luchtvochtichtigheid.
* *bitrate* Voor de temperatuur zijn 2 bytes voldoende (0..500, in 1/10 graad Celcius: 234 staat dan voor 23,4 graad Celcius).
  Voor de luchtvochtigheid is 1 byte genoeg (0-200, in 1/2%).
* *bitrate* We hoeven de sensoren niet vaker dan eens in de 5 minuten uit te lezen.
* *latency* Een sensoruitlezing moet binnen 60 seconden na de meting beschikbaar zijn voor de toepassing
  (de besturing van de verwarming of airconditioning).
* *betrouwbaarheid* Het is niet erg als 1 of 2 opeenvolgende sensor-uitlezingen ontbreken.
* *veiligheid* Sensoruitlezingen mogen niet door derden gelezen kunnen worden;
  een sensoruitlezing mag ook niet vervalst kunnen worden.
* Er zijn geen aanvullende privacy-eisen.

*Opmerking* we werken in het geval van sensorwaarden vaak met gehele getallen,
waarbij een vaste schaalfactor gebruikt wordt om tot de gebruikelijke eenheid te komen.

*verlichting*

* elke lamp bevat een IoT-knoop om deze aan te sturen;
* daarnaast zijn er sensoren die als automatische en handbediende lichtschakelaars werken.
* *bitrate*: voor het instellen van een lamp zijn 4 bytes nodig: voor de lichtintensiteit en voor de kleuren.
* *bitrate*: voor het instellen van een lamp zijn soms meerdere berichten nodig, bijvoorbeeld 10 berichten in 5 seconden.
* *bitrate*: gewoonlijk wordt een lamp niet vaker dan eens in de 30 minuten bediend.
* *latency*: een lamp moet binnen 0,5 seconde reageren op het bedienen van een schakelaar.
* *veiligheid*: sensoruitlezingen en besturingsberichten mogen niet door derden gelezen kunnen worden;
  deze mogen ook niet vervalst kunnen worden.
* er zijn geen aanvullende privacy-eisen.

*Opmerking*: voor de latency werken we hier met een *end-to-end* eis:
voor de gebruiker maakt het niet welk onderdeel voor de vertraging verantwoordelijk is,
het gaat alleen om de totale vertraging.


----

.. csv-table:: Sproeier-eisen
  :header: "Soort eis", "aa", "bb"
  :widths:  15, 10, 30

  "#berichten/uur", 10
  "#bytes/bericht", 10
  "betrouwbaarheid", "best effort"
  "max. latency",  15 min.
  "mobiliteit", ""
  "privacy", ""
  "veiligheid", ""

.. csv-table::
  :header: "Soort eis", "aa", "bb"
  :widths:  15, 10, 30

  "#berichten/uur", 10
  "#bytes/bericht", 10
  "betrouwbaarheid", "best effort"
  "max. latency",  15 min.
  "privacy", ""
  "veiligheid", ""

NB:

- verschillende eisen voor sensoren en actuatoren;
  bijv. sproeier: sensoren draadloos, actuatoren bedraad (waterkleppen).

----

Eisen aan de communicatie
=========================

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

  Vaak zijn we niet in de details van een signaal geïnteresseerd,
  maar alleen in bepaalde karakteristieke patronen of *events* die je in het signaal kunt herkennen.

  *Voorbeeld*: een cardioloog wil graag een volledig ECG-signal (electrocardiogram);
  een sporter heeft voldoende aan het aantal hartslagen per minuut.

  Een *event* is in dit verband een gebeurtenis of toestandsverandering die op een bepaald tijdstip plaatsvindt;
  de duur van deze gebeurtenis is niet van belang.
  Als de duur wel relevant is, kun je altijd gebruik maken van begin- en eind-events.

  In onze IoT-toepassingen gaat het vooral om events:
  gebeurtenissen waar we op moeten reageren.
  *Voorbeeld*: we willen graag weten wanneer de eerste persoon een kamer binnenkomt, en wanneer de laatste deze kamer verlaat (*events*).
  Dat zijn momenten dat de verlichting in- of uitgeschakeld moet worden.
  Het aantal personen in de kamer op een willekeurig moment (*signaal*) is daarvoor niet van belang.

  Uit de IoT-voorbeelden:

  - de bodemvochtigheid is een *signaal* dat we elke 5 minuten bemonsteren: snellere veranderingen komen niet voor of hebben geen betekenis.
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
