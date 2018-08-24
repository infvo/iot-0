*********
Overzicht
*********

.. bij de inleiding; overzicht van het materiaal van deze module.

Het Internet of Things koppelt de fysieke wereld aan de virtuele wereld van het internet.
De eerste stap in deze koppeling zijn de *sensoren en actuatoren* verbonden met een "ding" in de fysieke wereld.
Deze sensoren en actuatoren zijn onderdeel van een *IoT-knoop*;
de andere onderdelen van deze knoop zorgen voor de verwerking van de data en voor de verbinding met het internet.

.. figure:: IoT-knoop-0.png
  :width: 350px
  :align: center

  IoT-knoop, met sensoren en actuatoren, en radio voor de verbinding

Voor de koppeling tussen een IoT-knoop en het internet kunnen we kiezen uit verschillende technologische oplossingen,
bijvoorbeeld voor verschillende soorten radio's.
De keuze voor een bepaalde technologie hangt af van de gebruikerseisen.

In deze module maak je kennis met een aantal mogelijkheden voor de koppeling tussen een IoT-knoop en het internet.
Met deze kennis kun je voor een bepaalde IoT-toepassing een passende technologische oplossing kiezen.

.. figure:: IoT-keten-0.png
  :width: 500px
  :align: center

  IoT-keten: van IoT-knoop via "???" naar het internet, en via *mqtt/json* naar de web-toepassing

Een ander belangrijk thema in deze module is de koppeling tussen de gegevens van de IoT-knoop en de uiteindelijke toepassing ("app").
We laten zien hoe je deze koppeling zo kunt maken dat deze onafhankelijk is van de koppeling tussen de IoT-knoop en het internet.

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

.. todo::

  NB: er ontbreekt één belangrijke technologie: BLE.
