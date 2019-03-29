***********
Protocollen
***********

.. topic JSON in het web

  * AJAX: JavaScript en JSON
  * websockets als symmetrisch "push" protocol


Inleiding
=========

.. admonition: Concepten en leerdoelen

  * protocol-stack (stapeling van protocollen)
  * end-to-end protocol
  * interactie: client-server, publish-subscribe
  * adressering in de verschillende protocollen
  * payload, payload-formaat, meta-data

Protocolstack
-------------

Bij de communicatie tussen de onderdelen van een IoT-keten hebben we te maken met verschillende *protocollen*.
op meerdere niveau's.

In het internet ziet de *protocolstack* er als volgt uit:

* toepassingenlaag - MQTT, HTTP enz., en de protocollen daarboven
* transportlaag - TCP (en TLS, voor veilige verbindingen), UDP
* netwerklaag - IP ("internet protocol")
* datalink-laag - (fysieke laag) op basis van Ethernet, WiFi, enz.

De onderste twee lagen vormen de verbinding in het netwerk.
De lagen daarboven zijn *end-to-end* protocollen: deze vind je in de *end devices*, niet in het (IP)netwerk.
Voorbeelden van end-devices: IoT-knoop, controller, server, browser.
Voorbeelden van network-devices: gateway, router, firewall.

Een bijzonder geval in deze context is een MQTT-broker:
vanuit de toepassingen bekenen is dit eigenlijk een network-device,
voor het internet is dit een end-device.

We kunnen bovenop MQTT en HTTP ons eigen end-to-end toepassingsprotocol bedenken.
In de voorbeelden gebruiken we een protocol voor de sensor/actuatordata van de IoT-knopen.
Door hetzelfde protocol te gebruiken voor de verschillende IoT-ketens,
met verschillende radio's in de "edge", kunnen we de rest van de keten grotendeels gelijk houden.

Interactie
----------

Het webprotocol HTTP gebruikt een *client-server* interactie;
de browser is client van de webserver.
Overigens kunnen ook andere programma's als webclient optreden.

Het IoT-protocol MQTT gebruikt een *publish-subscribe* interactie.
De IoT-knopen en de controllers zijn clients van de MQTT-broker.

Adressering
-----------

In een netwerk moeten de verschillende apparaten elkaar kunnen vinden:
*adressering* is een onderdeel van een netwerpprotocol.
Op elk niveau van de protocolstack komen we een passende vorm van adressering tegen:

* HTTP: URL; MQTT: topic;
* TCP: poortnummer
* IP: ip-adres (en domeinnaam)
* WiFi: MAC-adres; RFM69: node-nr; LoRa: device-EUI.

In de IoT-toepassingen gebruiken we (voorlopig) het nummer van de IoT-knoop als identificatie en adres.
Dit nummer leiden we af van het MAC-adres, of we wijzen dit zelf toe.

Payload
-------

Bij een protocol hoort ook een *payload*: de eigenlijke data.
Voor een toepassing moeten we vaak afspraken maken over het payload-formaat.
Voor onze IoT-toepassingen gebruiken we het JSON-formaat met daarin het Cayenne-LPP formaat voor de sensordata.

* HTTP: HTML als payload-formaat; MQTT: (JSON)
* TCP: bytestroom
* IP: payload-deel van pakket

Naast de payload hebben we te maken met de *meta-data* van het protocol.
Deze bestaat bijvoorbeeld uit het adres van de bestemming, het adres van de afzender,
een identificatie van het hoger-niveau protocol.
In het geval van radio-verbindingen kan bijvoorbeeld de signaalsterkte opgenomen zijn in de meta-data.

Protocollen
===========

HTTP (web)
----------

.. figure:: IoT-client-server-0.png
   :width: 500 px
   :align: center

   Client-server interactie (HTTP)

HTTP is het basisprotocol voor het web, met een *client-server interactie* tussen de browser en de webserver.
De browser stuurt een *request* naar de webserver,
met een URL (Uniform Resource Locator) voor het adresseren van een web-element (*resource*).
Als resultaat (payload) krijgt de browser een HTML-document in de response van de server.

Deze client-server interactie is een "pull"-interactie:
de browser (client) heeft het initiatief, en "trekt" de HTML-documenten van de server.
Bij de client-server interactie moet de server altijd bereikbaar zijn;
de client hoeft alleen gedurende de interactie verbonden te zijn.

Sommige IoT-knopen gebruiken het HTTP-protocol.
We bespreken dit in meer detail in het hoofdstuk over Webserver-knopen.

MQTT (IoT)
----------

Voor communicatie in het Internet of Things is HTTP minder geschikt.
Een veelgebruikt IoT-protocol is MQTT (Message Queuing Telemetry Transport;
zie https://en.wikipedia.org/wiki/MQTT, http://mqtt.org):

.. figure:: MQTT.png
   :width: 400 px
   :align: center

   Publish-subscribe interactie (MQTT)

MQTT is een *publish/subscribe*-protocol, waarbij een *broker* als tussenschakel fungeert tussen de clients:

* een client "pusht" een bericht met een bepaald *topic* naar de broker,
* de broker "pusht" een ontvangen bericht naar alle clients die op dit topic geabonneerd zijn.

De broker altijd bereikbaar moet zijn;
clients hoeven niet altijd verbonden te zijn.
De broker kan eventueel berichten voor clients bufferen.

Door middel van een *topic* adresseert een client een stroom van berichten.
Als *payload* van de berichten wordt vaak JSON gebruikt.
JSON biedt een leesbaar en redelijk compact formaat, in het bijzonder geschikt voor data.

In onze IoT-voorbeelden gebruiken we een speciaal JSON-formaat, gebaseerd op het binaire Cayenne-LPP-formaat.
We bespreken MQTT en JSON in meer detail in het hoofdstuk over WiFi-MQTT-knopen.

RFM69 en C-LPP
--------------

De RFM69-radio biedt een pakketcommunicatie, met kleine pakketten (max. 62 bytes payload).
De IoT-knopen in een RFM69-netwerk adresseren we met een *nodenr* (2..60);
nodenr=1 gebruiken we voor de gateway.
Als formaat voor de payload gebruiken we het Cayenne LPP formaat:
dit is als binair formaat veel compacter dan JSON.
*Meta-data* bij de communicatie is onder andere de signaalsterkte van de ontvangen berichten.

We bespreken C-LPP in het hoofdstuk over (???)

.. todo::

  * waar C-LPP? - meest logisch is in RFM69-hoofdstuk, maar "chronologisch" komt dat niet uit.

LoRaWan
-------

Het LoRaWan-protocol biedt een nog kleinere payload:
deze is in de praktijk beperkt tot ca. 25 bytes; kleiner is beter.

Voor de identificatie (adres) heeft elke IoT-knoop in het TTN/LoRaWan-netwerk een unieke EUI.
