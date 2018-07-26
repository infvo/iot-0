MQTT: publish-subscribe
=======================

MQTT is een zogenaamd publish-subscribe protocol.
De communicatie vindt in dit geval plaats tussen clients, met tussenkomst van een broker (makelaar).
De broker heeft alleen als taak om berichten door te sturen:

* een client kan zich bij de broker abonneren ("subscribe") op de berichten van een bepaald onderwerp ("topic");
* een client kan een bericht van een bepaald onderwerp publiceren ("publish").
  De broker stuurt dit bericht dan door naar alle clients zich op dit onderwerp geabonneerd hebben.
* een client kan zowel "publisher" als "subscriber" zijn.

.. figure:: mqtt/MQTT.png
   :width: 600 px
   :align: center

   MQTT voorbeeld

Dit laatste betekent dat de broker op een willekeurig moment een bericht naar een client kan "pushen":
deze client hoeft niet steeds bij de broker te vragen of er een nieuw bericht aangekomen is.

  *Dit is een verschil met het HTTP-protocol: in dat geval ligt het initiatief altijd bij de client,
  deze "trekt" ("pull") de documenten van de server.*

In de figuur publiceert client A bericht M met topic T.
De broker stuurt dit bericht door ("push") naar clients B en D,
die zich eerder met "subscribe(T)" op topic T geabonneerd hebben.
Client C heeft zich niet geabonneerd op topic T, en ontvangt bericht M dus niet.

Op een volgend moment kan client B een bericht publiceren met een topic waar bijvoorbeeld A, C en D op geabonneerd zijn. Elke client kan dus zowel "publisher" als "subscriber" zijn.

Deze publish/subscribe-aanpak heeft de volgende eigenschappen:

* ''losse koppeling'' tussen de clients: de clients communiceren via een topic, en weten niet welke clients aan dat topic gekoppeld zijn.
    * In het bijzonder weet een publisher niet welke clients op het topic geabonneerd zijn.
* "push" van berichten van de client naar de broker, en van de broker naar de clients.

Gebruik van MQTT in het Internet of Things
==========================================

In de context van het Internet of Things zijn zowel IoT-knopen als IoT-apps clients van de MQTT-broker.

Een typisch gebruik van MQTT voor IoT-toepassingen:

* een IoT-knoop publiceert zijn sensordata naar de broker
    * als de sensordata veranderd zijn, en/of met een vaste regelmaat
    * met een topic dat gekoppeld is aan de identiteit van de IoT-knoop
* een IoT-knoop abonneert zich op de actuatordata voor de eigen actuatoren
    * met een topic dat gekoppeld is aan de identiteit van de knoop
* een IoT-app abonneert zich op de sensordata van de relevante knopen
* een IoT-app stuurt een actuator in een IoT-knoop aan door een "publish"
    * van een bericht met het actuator-topic van de IoT-knoop.

Voorbeeld
=========

.. figure:: mqtt/MQTT-IoT.png
   :width: 600 px
   :align: center

   MQTT in het Internet of Things

Uitleg:

* IoT-knoop A publiceert zijn sensordata onder topic <code>A/sensors</code>
* IoT-knoop A (na "subscribe(A/led)") ontvangt de berichten voor het aansturen van de eigen LED
* analoog voor IoT-knoop B
* toepassing (app) C ontvangt (na "subscribe(+/sensors)") de sensordata van A en van B
    * "+" is hierin een wildcard-teken: dit past op alle strings (node-id's)
* toepassing C stuurt de LED van de node A aan.
* analoog voor app D
    * deze communiceert alleen met node B.
* de broker stuurt de sensordata door naar de apps, en de led-aansturing naar de IoT-knopen.

Structuur van een topic
=======================

Een MQTT-topic bestaat uit een aantal strings gekoppeld door ``/``,
bijvoorbeeld ``abd``, ``abc/def``, ``abc/123/def``.
Dit lijkt op de padnaam in een URL.

Bij een *subscribe* kun je in de topic-string ook wildcards opnemen:
``+`` staat voor een willekeurige string zonder ``/``;
``#`` voor een willekeurige string waarin ook het koppelteken ``/`` mag voorkomen.
* voorbeeld: ``node/+/sensors`` matcht met ``node/12/sensors`` en ``node/432/sensors``.
* voorbeeld: ``node/#`` matcht met ``node/12/sensors`` en ``node/432/led``

JSON
====

.. sidebar:: JSON in het web

  Ook het web gebruikt het JSON-formaat, als onderdeel van AJAX.
  JavaScript-functies in een app communiceren vanuit de browser met de server,
  met (naar verhouding) kleine hoeveelheden data.
  Deze communicatie gebruikt het compacte en leesbare JSON-formaat, in plaats van complete HTML-documenten.

In dit hoofdstuk gebruiken we MQTT voor het bewaken en besturen van een IoT-knoop.
De communicatie tussen de IoT-knoop en de toepassing (app), via de MQTT-broker,
vindt plaats in de vorm van JSON-berichten.
Het JSON-formaat is voor het IoT wat HTML is voor het web.
We leggen hieronder eerst de principes van JSON uit.
In de opdracht gebruik je JSON voor het aansturen van de IoT-knoop.

JSON staat voor "JavaScript Object Notatie".
Een JSON-document is een tekstdocument: een leesbare vorm van een JavaScript-object.
Een object bestaat uit een aantal <code>"naam": waarde</code>-paren.
Een waarde kan een enkelvoudige waarde zijn, bijvoorbeeld een getal, een string, of een boolean.
Het kan ook een samengestelde waarde zijn: een object of een array.
In JavaScript gebruiken we de notatie: ``naam: waarde``.
In JSON staat de naam tussen dubbele quotes: ``"naam": waarde``.

We geven in de onderstaande voorbeelden de JavaScript-objecten en de bijbehorende JSON-notatie.

.. code-block:: javascript

  {temp: 21, press: 1015, id: "e4c7"}

.. code-block:: json

  {"temp":123,"press":1012, "id": "e4c7"}

.. todo::

  * meer JSON-voorbeelden

Je kunt een JSON-document eenvoudig omzetten in een JavaScript-object, en omgekeerd:

* ``obj = JSON.parse(str);``
* ``str = JSON.stringify(obj);``

Niet alle JavaScript-objecten kun je in JSON omzetten:

* objecten met functie-waarden
* objecten met onderlinge verwijzingen die een lus (cykel) vormen.

Veel programmeertalen hebben functies om JSON-objecten te verwerken.

* Python: https://docs.python.org/3/library/json.html
* Arduino: https://arduinojson.org/

Links
=====

* referentie: https://www.json.org/
* referentie: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
* tutorial: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON

Opdrachten
==========

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

.. figure:: mqtt/IoT-mqtt0-app.png
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
