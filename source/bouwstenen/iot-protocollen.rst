Protocollen
===========

HTTP (web)
----------

HTTP is het basisprotocol voor het web.
De *client-server interactie* tussen de browser en de webserver gebruikt dit protocol.
Door middel van een URL (Uniform Resource Locator) adresseert de browser een element ("*resource*")" in het web.
Deze URL bevat het adres van de server, en het adres ("pad") van het eigenlijke web-element.
Als resultaat krijgt de browser een HTML-document, als "payload" in de response van de server.


Sommige IoT-knopen (of bridges) gebruiken het HTTP-protocol.
We bespreken dit in meer detail in het hoofdstuk over Webserver-knopen.

MQTT (IoT)
----------

Het Internet of Things stelt andere eisen aan de communicatie dan het web:
HTTP is voor het IoT niet bijzonder geschikt.
Een veelgebruikt IoT-protocol is MQTT (Message Queuing Telemetry Transport;
zie https://en.wikipedia.org/wiki/MQTT, http://mqtt.org):

* MQTT is een *publish/subscribe*-protocol, waarbij een *broker* als tussenschakel fungeert tussen de clients;
* een client "pusht" een bericht met een bepaald *topic* naar de broker,
* de broker "pusht" een ontvangen bericht naar alle clients die op dit topic geabonneerd zijn.

Door middel van een *topic* adresseert een client een stroom van berichten.
Als "payload" van de berichten wordt vaak JSON gebruikt.
JSON biedt een leesbaar en redelijk compact formaat, in het bijzonder geschikt voor data.

In onze IoT-voorbeelden gebruiken we een speciaal JSON-formaat, gebaseerd op het binaire LPP-formaat.
We bespreken MQTT en JSON in meer detail in het hoofdstuk over WiFi-MQTT-knopen.

.. topic JSON in het web

  * AJAX: JavaScript en JSON
  * websockets als symmetrisch "push" protocol

Protocol-stack
--------------

* end-to-end (toepassingen in de eindpunten)
* IP-protocol in het netwerk
* HTTP (web-protocol) -> webserver
* TCP/IP (internet basisprotocol) -> webserver
* MQTT (iot-protocol) -> WiFi-knopen

Adressering
-----------

* op de verschillende lagen
* HTTP: URL
* MQTT: topic
* IP: IP-adres
* TCP: poortnummer

Payload
-------

* HTTP: HTML-documenten
* MQTT: JSON (JSON-LPP)
* RFM69: LPP (binair)
* LoRa: binaire applicatie-payload

Protocol-conversie
------------------

* gateway (...toepassing in de gateway...)

Inleiding
=========

Concepten

* end-to-end protocol
* adressering (in de verschillende protocollen)
* protocol-stack (stapeling van protocollen)
* interactie: client-server, publish-subscribe


Bij de communicatie tussen de onderdelen van een IoT-keten hebben we te maken met verschillende *protocollen*.
op meerdere niveau's.

In het internet ziet de *protocolstack* er als volgt uit:

* toepassingenlaag - met protocollen als MQTT en HTTP, en de protocollen daarboven
* transportlaag - TCP (en TLS, voor veilge verbindingen), UDP
* netwerklaag - IP ("internet protocol")
* datalink-laag - (fysieke laag) op basis van Ethernet, WiFi, enz.

De onderste twee lagen vormen de verbinding in het netwerk.
De lagen daarboven zijn *end-to-end* protocollen: deze vind je in de *end devices*, niet in het (IP)netwerk.
Voorbeelden van end-devices: IoT-knoop, controller, server, browser.
Voorbeelden van network-devices: gateway, router, firewall,

Een bijzonder geval in deze context is een MQTT-broker:
vanuit de toepassingen bekenen is dit eigenlijk een network-device,
vanuit het internet-netwerk is dit een end-device.

We kunnen bovenop MQTT en HTTP onze eigen end-to-end toepassingsprotocol bedenken.
In de voorbeelden gebruiken we een protocol voor de sensor/actuatordata van de IoT-knopen.
Door hetzelfde protocol te gebruiken voor de verschillende IoT-ketens,
met verschillende radio's in de "edge", kunnen we de rest van de keten grotendeels gelijk houden.


Interactie
----------

Het webprotocol HTTP gebruikt een *client-server* interactie;
de browser is client van de webserver.
(Ook andere programma's kunnen als webclient optreden.)

Het IoT-protocol MQTT gebruikt een *publish-subscribe* interactie.
