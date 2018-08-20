**************
IoT-bouwstenen
**************

We beschrijven hier de verschillende bouwstenen van het Internet of Things.
Met deze bouwstenen kun je op verschillende manieren een *keten* vormen.
Een aantal mogelijke ketens vind je in het keten-gedeelte;
in de andere delen beschrijven we de interacties tussen deze bouwstenen,
en de protocollen die we daarbij gebruiken.

IoT-knoop
=========

Een *IoT-knoop* koppelt de fysieke wereld aan de virtuele wereld.
Zo'n knoop omvat gewoonlijk:

* sensoren, om in de fysieke wereld te meten;
* actuatoren, om in de fysieke wereld te sturen;
* een microcontroller ("computer op een chip") voor de besturing van de IoT-knoop;
* communicatie, vaak draadloos (radioverbinding)
* energie ("voeding"), voor draadloze knopen in de vorm van een batterij, soms aangevuld met *energy harvesting*.

.. figure:: IoT-knoop-0.png
  :width: 400px
  :align: center

  Een IoT-knoop

In de voorbeelden gebruiken we een IoT-knoop met de volgende sensoren/actuatoren:

* twee LEDs - als actuatoren
* twee drukknoppen - als "event" sensoren
* sensoren voor: temperatuur, luchtdruk en lichtniveau

Voor de draadloze communicatie gebruiken we verschillende protocollen (zoals MQTT en LoRaWan) en verschillende radio's (WiFi, RFM69, LoRa).
In deze laatste twee gevallen hebben we een *gateway* nodig voor de verbinding van de IoT-knoop met het lokale netwerk of met het publieke internet.
De eisen die het "ding" stelt aan bitrate, bereik, mobiliteit (draadloos?) en energieverbruik bepalen de keuze voor de radio.

Voor de microcontroller voor de besturing zijn er ook meerdere alternatieven, zoals Atmega AVR (Arduino), ESP8266, ESP32, ARM.
De keuze voor de microcontroller hangt meer af van de eigen voorkeur en omstandigheden dan van de eisen van de toepassing.

Radio
-----

Voor de verbinding tussen een draadloze IoT-knoop en het internet kun je kiezen uit verschillende radio's.
Deze radio's verschillen in hun energieverbruik (power), bereik en bitrate.
In deze module maken we kennis met enkele IoT-radio's.
De onderstaande tabel geeft de belangrijkste karakteristieken.

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

**Opmerking**: in deze lijst ontbreekt nog Bluetooth Low Energy (BLE).
We proberen deze in een toekomstige versie van dit materiaal toe te voegen.

Gateway
=======

Soms kunnen we de IoT-knopen niet rechstreeks in het internet verbinden,
bijvoorbeeld omdat deze knopen een ander (eenvoudiger) protocol gebruiken.
We gebruiken dan een *gateway* om het IoT-knoop-protocol om te zetten naar het IP-protocol, en omgekeerd.

.. figure:: IoT-0-stacks-gateway.png
   :width: 400 px
   :align: center

   IoT-gateway

Bij een dergelijke omzetting (protocolconversie) moeten we rekening houden met de complete protocolstack,
tot en met de toepassing.
In onze voorbeelden betekent dit dat de berichten van de IoT-knopen omgezet worden naar MQTT-berichten in het formaat van de toepassing.

.. admonition:: Gateway versus bridge

  We maken hier onderscheid tussen ''gateways'' en ''bridges'':
  een bridge verbindt netwerken met eenzelfde protocol(stack),
  een gateway verbindt netwerken met verschillende protocollen.
  De omzetting in een bridge is dan beperkt tot de gemeenschappelijke onderste laag van de protocollen.
  Bij een gateway moet je de hele protocolstack hierbij betrekken.
  Een gateway is  vaak (aanzienlijk) complexer dan een bridge.
  Bovendien hebben veranderingen in de toepassing mogelijk gevolgen voor de gateway.
  Voor een bridge is de toepassing niet van belang.
  Overigens wordt deze terminologie, met een duidelijk onderscheid tussen bridge en gateway,
  niet overal op dezelfde manier gebruikt.

Ook als de IoT-knoop zelf de internet-protocolstack gebruikt kan het zinvol zijn om een bridge te gebruiken,
om de lokale communicatie te scheiden van het publieke internet.
Deze bridge kan er bijvoorbeeld zorgen voor de versleuteling van het verkeer naar het publieke internet.

MQTT-broker
===========

We gebruiken het MQTT-protocol om IoT-knopen te koppelen aan de toepassing (web-app).
MQTT is een *Publish-Subscribe* protocol.
De MQTT-broker koppelt de verschillende soorten clients:
de IoT-knopen aan de ene kant, en de toepassingen en diensten aan de andere kant.

NodeRed-server
==============

.. figure:: Nodered-chat-flow.png
   :width: 500 px
   :align: center

   NodeRed Chat flow

IoT-toepassingen combineren vaak data uit verschillende bronnen:
vanuit IoT-knopen, maar ook uit databases of andere datastromen.
Om deze ruwe data bruikbaar te maken voor de gebruikerstoepassing, kun je deze eerst door externe diensten (Data Science, Artificial Intelligence, enz.) bewerken.
Deze databronnen, diensten en gebruikerstoepassingen gebruiken verschillende protocollen en formaten.
Met NodeRed knoop je deze verschillende onderdelen samen, op een grafische manier.
Een NodeRed-server is daarom in onze voorbeelden vrijwel altijd onderdeel van de IoT-keten.


Web(-app) server
================

.. figure:: Nodered-dashboard-display-0.png
   :width: 500 px
   :align: center

   Web-app voorbeeld: dashboard

Uiteindelijk komen deze data terecht bij een gebruikerstoepassing;
deze bereik je via een web(-app)server.
Een voorbeeld van een eenvoudige toepassing is een *dashboard*, met een samenvatting van de gegevens van de IoT-knopen.

In onze voorbeeld-toepassing werken we met een eenvoudig dashboard met de gegevens van één IoT-knoop.
Dit dashboard maken we met NodeRed: we gebruiken deze dan (ook) als webserver.
Bovendien kunnen we via dit dashboard de actuators van de IoT-knoop bedienen.
Dit gebruikersinterface heeft de vorm van een webtoepassing ("app"), beschikbaar via een server in het publieke web.
