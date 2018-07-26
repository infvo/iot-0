**********
Opdrachten
**********

.. bij mqtt

.. admonition:: Wat heb je nodig?

  * MQTT broker in publieke internet;
      * de MQTT-broker fungeert ook als statische webserver, voor de app MQTT0
      * je moet de domeinnaam, het poortnummer, en eventueel een gebruikersnaam/wachtwoord kennen.
  * IoT knoop met software: MQTT-0; verbonden in het lokale WiFi-netwerk; en verbonden met de MQTT broker;
      * je moet de ID van de knoop kennen; dit bestaat uit de laatste 4 cijfers van het MAC-adres;
      * dit kan eventueel een gesimuleerde IoT-knoop zijn.

Gebruik van de app MQTT1
------------------------

[[Bestand:IoT-mqtt0-app.png|400px|right|mqtt test app]]

.. figure::IoT-mqtt0-app.png
   :width: 600 px
   :align: center

   MQTT test app

De  web-app kun je vinden via de broker: http://infvopedia.nl:1884/mqtt1.html.
Hierin is infvopedia.nl de domeinnaam van de broker.
1884 is het HTTP-poortnummer van de broker: de broker fungeert ook als (statische) webserver.
Het MQTT-protocol gebruikt poort 1883.

De app heeft de volgende invoervelden en -knoppen:

* invoer ``IoT-node``: de ID van de IoT-knoop die je wilt aansturen, en waarvan je de sensordata wilt ontvangen
    * de sensordata worden ontvangen via topic ``node/ID/sensors``
* knoppen On en Off: sturen bericht ``{"led0":1}`` respectievelijk ``{"led0":1}`` met ``topic node/ID/actuators``
* invoer ``subscribe to topic``: hier geef je het topic op waarvan je de berichten wilt ontvangen
    * in het venster daaronder verschijnen de ontvangen berichten met hun topic
    * in het topic kun je ook wildcard-tekens opnemen, zoals ``+`` en ``#``.
* invoer ``publish to topic`` - om een bericht met een topic te versturen;
    * het bericht zelf voer je daaronder in, en
    * met de "publish"-knop verstuur je het bericht.

Alternatief: MQTT-box
---------------------

Er zijn ook desktop-applicaties waarmee je deze opdrachten kunt uitvoeren.
Een voorbeeld is MQTTbox (http://workswithweb.com/mqttbox.html).

Opdracht 1: Chatten met MQTT
----------------------------

(stap 1) Je kunt met MQTT op een eenvoudige manier met een groep gebruikers chatten, via de MQTT0-app in de browser.

* Spreek met de groep af welk topic je gebruikt voor de communicatie, bijvoorbeeld <code>chat</code>
* Stel dit topic in bij de beide topic-vensters: voor het ontvangen (subscribe)
  en voor het verzenden (publish) van de berichten.
* Als je nu in de ene browser in de app een bericht invoert en publiceert,
  zul je dit in de andere browsers in de app zien verschijnen.

(stap 2) Een nadeel van deze aanpak is dat je niet kunt zien van wie een bericht afkomstig is.
Dit kun je oplossen door de naam van de afzender in het topic op te nemen:

* vul als subscribe-topic een wildcard-topic in, bijvoorbeeld <code>chat/+</code>.
  Deze <code>+</code> matcht met een willekeurige string.
* vul als publish-topic een topic in met daarin de naam van de afzender,
  bijvoorbeeld <code>chat/frans</code>.
* je ziet dan bij de ontvangen berichten steeds de naam van de afzender in het topic.

Je moet er in dit geval wel op vertrouwen dat iedereen zijn eigen naam invult:
je kunt de afzender in het topic niet authenticeren.

Opdracht 2: MQTT0 met een IoT-knoop
-----------------------------------

Voor het gebruik van MQTT0 met een IoT-knoop moet je de node-id van die knoop invullen:

* vul de node-id van je sensorknoop in;
* vul als subscribe-topic in: ``node/+/+``;

Voor de communicatie tussen de toepassing MQTT0 en de IoT-knoop gebruiken we de volgende MQTT-topics:

* ``node/nodeid/sensors`` - voor de sensorwaarden
* ``node/nodeid/actuators`` - voor de actuatorwaarden.

Met het wildcard-topic <code>node/+/+</code> ontvang je alle sensor/actuatorwaarden van alle knopen.
Er is geen standaard die deze afspraken afdwingt: het is een eigen, praktische keuze.
Deze keuze gebruiken we in onze IoT-knopen en in onze toepassingen (apps).

Vervolgens kun je de LEDs aansturen en de sensorwaarden uitlezen:

* controleer of de LED aan- en uitgaat bij het indrukken van de knoppen;
* controleer of je de sensorwaarden ontvangt: deze verschijnen in tabelvorm.
    * hierin kun je ook zien of de LED brandt.

Opdracht 3: Aansturen van actuatoren (LEDs) met JSON
----------------------------------------------------

Bekijk (met de MQTT0-app) de JSON-berichten zoals die door de IoT-knopen verstuurd worden.

* vul als subscribe-topic in: ``node/+/sensors`` (of, in plaats van ``+``, de nodeid)
* vergelijk de ontvangen berichten in JSON-formaat met de waarden in de tabel erboven.
* hoe weet je wat de eigenschap (het veld) ``"temp"`` voorstelt?

Om de actuators (LEDs) van de IoT-knoop aan te sturen,
moet je in het publish-message-venster een JSON-bericht invoeren,
en dit met de "publish"-knop versturen.

* stuur een JSON-bericht naar een IoT-knoop om LED 1 aan (of uit) uit te zetten.
* controleer of de waarde die je voor de LEDs ontvangt, kloppen met de bedoelde actie.
* zet in één JSON-bericht beide LEDs tegelijk aan (of uit).

Opdracht 4: Gebruik van JSON in het Internet of Things
------------------------------------------------------

Het JSON-formaat wordt in het Internet of Things op meerdere plaatsen gebruikt.
Bestudeer de volgende manieren van gebruik:

* importeren en exporteren van NodeRed-flows
* berichten van TTN gateways (via MQTT)
    * welke informatie voegt een TTN gateway toe aan de "payload" van een IoT-knoop?
