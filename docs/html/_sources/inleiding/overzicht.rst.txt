*********
Overzicht
*********

.. bij de inleiding; overzicht van het materiaal van deze module.

Het Internet of Things koppelt de fysieke wereld aan de virtuele wereld van het internet.
De eerste stap in deze koppeling zijn de *sensoren en actuatoren* verbonden met een "ding" in de fysieke wereld.
Deze sensoren en actuatoren zijn onderdeel van een *IoT-knoop*;
de andere onderdelen van deze knoop zorgen voor de verwerking van de data en voor de verbinding met het internet.
Voor de koppeling tussen een IoT-knoop en het internet kunnen we kiezen uit verschillende technologische oplossingen,
bijvoorbeeld voor verschillende soorten radio's.
De keuze voor een bepaalde technologie hangt af van de gebruikerseisen.

In deze module maak je kennis met een aantal mogelijkheden voor de koppeling tussen een IoT-knoop en het internet.
Met deze kennis kun je voor een bepaalde IoT-toepassing een passende technologische oplossing kiezen.

  We behandelen een aantal representatieve mogelijkheden, waarbij verschillende problemen aan de orde komen.
  Het is in het kader van deze module niet mogelijk om alle beschikbare oplossingen te behandelen.

Een ander belangrijk thema in deze module is de koppeling tussen de gegevens van de IoT-knoop en de uiteindelijke toepassing ("app").
We laten zien hoe je deze koppeling zo kunt maken dat deze onafhankelijk is van de koppeling tussen de IoT-knoop en het internet.

.. todo::

  Inleiding-overzicht: <<fig met beide koppelingen>>

.. todo::

  NB: er ontbreekt één belangrijke technologie: BLE.

.. rubric:: Bouwstenen

In het volgende hoofdstuk geven we een overzicht van de bouwstenen van het Internet of Things.
Hierbij gaat het vooral om de koppeling tussen de IoT-knoop en het internet.
We kunnen daarvoor kiezen uit verschillende radio's, interacties en protocollen.
Deze bouwstenen combineren we tot een complete "IoT-keten", van IoT-knoop tot web-toepassing ("app").

.. note::

  Dit hoofdstuk is bedoeld als globaal overzicht.
  Hierbij komen veel nieuwe begrippen aan de orde: dat kan intimiderend zijn.
  Het is geen probleem als je die eerst niet begrijpt: in de volgende hoofdstukken komen die in detail terug.
  Je kunt dan later dit overzicht gebruiken om te begrijpen hoe die details in het geheel passen.

In de opdrachten gebruik je een IoT-keten met een gesimuleerde IoT-knoop.
Dit is ook een handig hulpmiddel voor de volgende hoofdstukken.

.. rubric:: IoT-knoop met webserver

Soms bevat een IoT-knoop een webserver:
deze levert de toepassing ("app") waarmee je het "ding" kunt besturen.
Een voorbeeld hiervan is een netwerkprinter.
In dit geval gebruikt de koppeling tussen de IoT-knoop en de "app" het HTTP-protocol,
voor een client-server interactie tussen de app op de browser en de webserver op de IoT-knooop.

Je gaat met een IoT-knoop met een webserver aan de slag.
Vanuit je eigen browser bestuur je enkele LEDs, en meet je de temperatuur, luchtdruk en lichtniveau.
Met behulp van de browser-ontwikkelaarstools bestudeer je het HTTP-protocol met de IoT-knoop.
In NodeRed maak je een eigen webserver, om het HTTP-protocol vanuit de server te bestuderen.

  NodeRed is een toepassing waarmee je op een grafische manier allerlei protocollen en diensten kunt koppelen.
  Hiermee kun je op een eenvoudige manier experimenteren, en snel een eigen toepassing maken.

.. rubric:: WiFi/MQTT-IoT-knoop

Een webserver moet altijd beschikbaar (online) zijn: dat is niet handig als je een IoT energiezuinig wilt maken.
Met een Publish-Subscribe interactie kunnen zowel de IoT-knoop als de toepassing ("app") *client* zijn van een gemeenschappelijk *broker*.
Het MQTT-protocol biedt dit, in een vorm die goed past bij het Internet of Things.

In dit hoofdstuk maak je kennis met het MQTT-protocol.
Je communiceert met een aparte toepassing via MQTT en de MQTT-broker met je IoT-knoop.
Daarmee kun je weer de sensoren van de IoT-knoop uitlezen en de LEDs besturen.

In NodeRed gebruik je de MQTT-nodes om met je IoT-knoop de communiceren.
Je kunt daarmee een eigen "app" maken, bijvoorbeeld een dashboard voor de IoT-knoop.

.. rubric:: RFM69

Een WiFi-radio is niet echt energiezuinig, en heeft naar verhouding een klein bereik.
Een RFM69-radio is zuiniger en heeft een redelijk groot bereik (tot enkele honderden meters).
Dit gaat ten koste van de *bitrate*.
Dit betekent dat je voor deze radio een eenvoudig protocol gebruikt.
Voor de aansluiting op het internet is dan een *gateway*: deze zet het RFM69-protocol om in MQTT.

In dit gedeelte gebruik je een IoT-knoop met een RFM69-radio, en een gateway met zowel een RFM69-radio als een WiFi-radio.
Deze gateway communiceert via MQTT (over WiFi) met de MQTT-broker.
Voor het uitlezen en aansturen van de sensoren en actuatoren van de IoT-knoop kun je dan dezelfde aanpak gebruiken als hiervoor.

In NodeRed koppel je de verschillende IoT-knopen aan elkaar.

.. rubric:: LoRa(Wan)

De LoRa-radio (Long Range) heeft een groter bereik dan de RFM69-radio: tot enkele kilometers.
Dit grote bereik gaat ten koste van de bitrate: je kunt maar een tiental kleine berichten per uur versturen.
Een LoRaWan-netwerk, op basis van deze LoRa radio, gebruikt *gateways* voor de verbinding tussen de IoT-knoop en het internet.
Er zijn enkele LoRaWan-netwerken met landelijke dekking (of groter), onder andere van KPN.

In de opdrachten gebruik je een LoRaWan-IoT-knoop met het TTN-netwerk.
Dit is een netwerk opgezet door een wereldwijde community, in plaats van door een bedrijf of een overheid.
Je gebruikt een *TTN-application* via één van de TTN-servers/brokers.
Daarbij bestudeer je de gevolgen van de lage bitrate.
Via MQTT communiceer je met de IoT-node(s) (*device*) in deze application: je kunt hiervoor bijvoorbeeld je eigen dashboard maken.

-------

.. rubric:: Bouwstenen

Als eerste stap beschrijven we de bouwstenen van de internet of things.
Deze bouwstenen kunnen we op verschillende manieren combineren tot een IoT-keten:
van "ding" tot toepassing.

* Een IoT-knoop koppelt een fysiek "ding" aan de virtuele wereld.
* Het MQTT-protocol koppelt een IoT-knoop aan de toepassingen in het internet.
* We gebruiken een uniform JSON-formaat voor de verschillende soorten IoT-knopen:
  op het niveau van de toepassingen kun je deze dan gelijk behandelen.
* Met NodeRed kun je IoT-knopen, diensten en toepassingen op een grafische manier aan elkaar verbinden.
* Een IoT-knoop is vaak draadloos:
  er zijn verschillende soorten radio's om de verbinding met het internet te maken,
  afhankelijk van de eisen die het "ding" en de toepassing stellen.

De gebruikerseisen zoals we die hiervoor gezien hebben, bepalen voor een belangrijk deel welke technologie je kunt gebruiken.
Allereeerst maak je een keuze voor de verbinding tussen de IoT-knoop en het internet.
De keuze van de protocollen volgt daarna: deze wordt voor een deel bepaald door de communicatiemogelijkheden.

.. rubric:: MQTT

MQTT is een veelgebruikt protocol voor het internet of things.
MQTT is een publish-subscribe-protocol: dit maakt het mogelijk om gegevens te *pushen* naar de bestemming.
(In de client-server interactie in het web, via het HTTP-protocol haalt de browser de gegevens van de server (*pull*).)

De oefeningen in dit deel zijn gericht op het leren werken met MQTT.
dit vormt de basis van de IoT-ketens die later aan de orde komen.)
Bij deze oefeningen gebruik je een paar eenvoudige hulpprogramma's die later ook van pas komen.

.. todo::

  <<JSON>>

.. rubric:: NodeRed

Met NodeRed kun je IoT-knopen, diensten en toepassingen op een grafische manier aan elkaar verbinden.

Dit onderdeel vormt een tutorial in het gebruik van NodeRed, gericht op het gebruik in deze module.
Dit tutorial behandelt een aantal NodeRed-flows voor het koppelen van IoT-knopen en toepassingen.
Een eenvoudige toepassing is een *dashboard* waarin je de sensorgegevens van één of meer IoT-knopen kunt volgen.

.. rubric: Webserver-knopen

Sommige IoT-knopen beschikken over een webserver.
Zo'n knoop kun je bewaken en besturen via een browser, via het HTTP-protocol.
Deze oplossing kun je gebruiken als de IoT-knoop verbonden is via Ethernet (bedraad) of via WiFi (draadloos).


.. rubric:: WiFi-knopen

.. todo::

  * MQTT; MQTT-broker (publiek - op afstand besturen; eventueel lokaal);
  * JSON (JSON-formaat voor sensorknopen)

Een veel gebruikt protocol voor het Internet of Things is MQTT.
Dit past beter dan HTTP bij de eisen

.. rubric:: RFM69-knopen

We gebruiken de RFM69-radio als voorbeeld van *pakketcommunicatie*.
Deze radio gebruikt een vrije ISM-band (868MHz).
Voor het gebruik van deze radioband gelden enkele beperkingen.
Het bereik van deze radio is 20-200 meter, afhankelijk van het zendvermogen, de antenne en de bitrate.
Net als vergelijkbare radio's zoals ZigBee en Z-wave is deze radio goed te gebruiken voor toepassingen in en rond het huis,
zoals *domotica*.

Voor het aansluiten van het RFM69-netwerk op het internet gebruiken we een gateway.
Deze zorgt onder andere voor de protocol-conversie, van het binaire LPP-formaat naar het tekst-gebaseerde JSON-formaat.

.. rubric:: LoRa-knopen

De LoRa-radio heeft een bereik van enkele kilometers:
deze is dan ook vooral geschikt voor IoT-knopen die mobiel zijn in een groot gebied.
Dit grote bereik gaat wel ten koste van de bitrate:
een IoT-knoop kan maar een tiental keren per uur een klein bericht versturen.

In dit onderdeel gebruiken we het publieke netwerk van TheThingsNetwork (TTN).
Je koppelt IoT-knopen in een gegeven TTN-toepassing aan een eigen dashboard.
Met hardware-knopen met LoRaWan/TTN-software kun je een eigen TTN-toepassing maken.
