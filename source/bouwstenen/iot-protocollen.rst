***********
Protocollen
***********

.. topic JSON in het web

  * AJAX: JavaScript en JSON
  * websockets als symmetrisch "push" protocol

.. admonition: Concepten en leerdoelen

  * protocol-stack (stapeling van protocollen)
  * end-to-end protocol
  * interactie: client-server, publish-subscribe
  * adressering in de verschillende protocollen
  * payload, payload-formaat, meta-data

Protocolstack
=============

Bij de communicatie tussen de onderdelen van een IoT-keten hebben we te maken met verschillende *protocollen*.
op meerdere niveau's.
De *protocolstack* van het internet er als volgt uit:

* toepassingenlaag - MQTT, HTTP enz., en de protocollen daarboven
* transportlaag - TCP (en TLS, voor veilige verbindingen), UDP
* netwerklaag - IP ("internet protocol")
* datalink-laag - (fysieke laag) op basis van Ethernet, WiFi, enz.

De onderste twee lagen vormen de verbinding *in het netwerk*.
De lagen daarboven zijn *end-to-end* protocollen: deze vind je in de *end devices*, niet in het (IP)netwerk.
Voorbeelden van end-devices: IoT-knoop, controller, server, browser;
ook een MQTT-broker is voor het internet een end-device.
Voorbeelden van network-devices: gateway, router, firewall.

.. figure:: HTTP-IP-stack.png
   :width: 600 px
   :align: center

   Protocol-stacks in HTTP client-server verbinding

HTTP en MQTT zijn voorbeelden van end-to-end protocollen:
deze vind je alleen in de eindpunten (end devices).
Het IP-protocol geeft de ingepakte HTTP-data ongewijzigd door:
"het heeft geen boodschap aan de boodschap",
net als bijvoorbeeld de traditionele post of telefonie.

Je kunt bovenop MQTT en HTTP je eigen end-to-end toepassingsprotocollen bedenken.
In de voorbeelden gebruiken we een protocol voor de sensor/actuatordata van de IoT-knopen.
Door hetzelfde protocol te gebruiken voor de verschillende IoT-ketens,
met verschillende radio's in de "edge", kunnen we de rest van de keten grotendeels gelijk houden.

.. admonition:: End-to-end

  Het *end-to-end* principe is één van de belangrijke bouwstenen van het internet.
  Het IP-protocol is het enige protocol *in het netwerk*.
  Dit is een uniform en universeel communicatieprotocol, gebaseerd op best-effort pakketcommunicatie.
  Alle aanpassingen aan de toepassingen gebeuren in de eindpunten (*end devices*).
  Dank zij dit end-to-end principe is het internet universeel en uitbreidbaar:
  iedereen kan een eigen end-to-end protocol gebruiken voor de eigen toepassingen.

Interactie
==========

.. figure:: IoT-client-server-0.png
   :width: 300 px
   :align: right

   Client-server interactie (HTTP)

Het webprotocol HTTP gebruikt een *client-server* interactie:
de browser (client) stuurt een verzoek (met een URL) naar de webserver,
en krijgt als response van de webserver het bijbehorende HTML-document.
Overigens kunnen ook andere programma's als webclient optreden.

.. figure:: MQTT.png
   :width: 300 px
   :align: right

   Publish-subscribe interactie (MQTT)

Het IoT-protocol MQTT gebruikt een *publish-subscribe* interactie,
waarbij een *broker* als tussenschakel fungeert tussen de clients:

* een client "pusht" een bericht met een bepaald *topic* naar de broker,
* de broker "pusht" een ontvangen bericht naar alle clients die op dit topic geabonneerd zijn.

Zowel de IoT-knopen en de controllers zijn clients van de MQTT-broker:
voor de broker zijn deze gelijkwaardig.

Adressering
===========

In een netwerk moeten de verschillende apparaten elkaar kunnen vinden:
*adressering* is een onderdeel van een netwerpprotocol.
Op elk niveau van de protocolstack komen we een passende vorm van adressering tegen:

* HTTP: URL; MQTT: topic (adres van de IoT-knoop);
* TCP: poortnummer (adres van de toepassing)
* IP: ip-adres en domeinnaam (adres van de "host": end-device)
* WiFi: MAC-adres; RFM69: node-nr; LoRa: device-EUI (hardware-adres)

In de IoT-toepassingen gebruiken we het nummer van de IoT-knoop als identificatie en adres.
Dit nummer leiden we af van het MAC-adres, of we wijzen dit zelf toe.

Payload
=======

Bij een protocol hoort ook een *payload*: de eigenlijke data.
Voor een toepassing maken we afspraken over het payload-formaat.
Voor onze IoT-toepassingen gebruiken we het JSON-formaat met daarin het Cayenne-LPP formaat voor de sensordata.

* HTTP: HTML als payload-formaat; MQTT: (JSON)
* TCP: bytestroom
* IP: payload-deel van pakket, met (o.a.) fragmenten van de TCP bytestroom.
* Ethernet, WiFi: frames (pakketten op fysiek niveau) met IP-pakketten als payload.

Naast de payload hebben we te maken met de *meta-data* van het protocol.
Deze bestaat bijvoorbeeld uit het adres van de bestemming, het adres van de afzender,
een identificatie van het hoger-niveau protocol.
In het geval van radio-verbindingen kan bijvoorbeeld de signaalsterkte opgenomen zijn in de meta-data.

Overzicht
=========

.. rubric:: HTTP (web)

HTTP is het protocol van het web.
In het gedeelte over IoT-knopen als webserver gaan we dieper op dit protocol in.

.. rubric:: MQTT (IoT)

MQTT is een basisprotocol voor het Internet of Things.
We gebruiken dit protocol voor het verkeer tussen de WiFi IoT-knoop en de controller(s),
tussen de RFM69 gateway en de controller, tussen de LoRa server en de controller,
en tussen de controllers onderling.
In het deel over WiFi-MQTT knopen behandelen we dit protocol.

.. rubric:: RFM69

RFM69 is één van de vele pakketradio's voor het IoT,
met een eigen protocol.
In de kleine pakketten (max. 60 bytes) past alleen een binaire payload.
Hiervoor gebruiken we het Cayenne Low Power Payload-formaat (C-LPP).
In het deel over RFM69 werken we dit verder uit;
we gaan daar ook in op de protocolconversie in de gateway tussen het RFM69 netwerk met de IoT-knopen,
en het IP-netwerk.

.. rubric:: LoRa(Wan)

De LoRa radio's in het LoRaWan gebruiken ook een eigen pakketformaat.
Dit wordt in de TTN gateways/servers omgezet in een MQTT-formaat.
Ook in dit geval gebruiken we een binaire payload:
de pakketten zijn meest 10-20 bytes groot (of liever: klein).
