**************
IoT-bouwstenen
**************

IoT-knoop
=========

Een *IoT-knoop* koppelt de fysieke wereld aan de virtuele wereld.
Zo'n knoop omvat:

* sensoren, om in de fysieke wereld te meten;
* actuatoren, om in de fysieke wereld te sturen;
* een microcontroller ("computer op een chip") voor de besturing van de IoT-knoop;
* communicatie, meestal draadloos (radio)
* energie ("voeding"), meestal in de vorm van een batterij, soms aangevuld met "energy harvesting".

.. rubric:: Voorbeeld van een IoT-knoop

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

Voor de microcontroller voor de besturing zijn er ook meerdere alternatieven, zoals Atmega AVR (Arduino), ESP8266, ESP32, ARM.
De keuze voor de microcontroller hangt af van de lokale omstandigheden, de gebruikte programmeertaal/programmeeromgeving, enz.

We voeden de draadloze IoT-knopen gewoonlijk met een batterij.

.. figure:: Iotnode-simulator-0.png
   :width: 400 px
   :align: center

   IoT-knoop simulator

In de opdrachten gebruiken we een gesimuleerde IoT-knoop.
Je moet dan zelf de sensorwaarden instellen.
Deze gesimuleerde IoT-knoop gebruikt hetzelfde (MQTT-)protocol als de hardware-IoT-knopen.

Radio
=====

Voor de verbinding tussen een draadloze IoT-knoop en het internet zijn verschillende radio's beschikbaar.
Deze radio's verschillen in hun energieverbruik (power), bereik en bitrate.
In deze module gebruiken we verschillende radio's, voor verschillende toepassingen.
De onderstaande tabel geeft de beangrijkste karakteristieken.

+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+
| **power** | **bereik**              | **bitrate**   | **radio** | **tussenstap**           | **protocol** (**) |
+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+
| medium    | lokaal bereik (10-50m)  | MBytes/s      | WiFi      | (rechtstreeks)           | HTTP              |
+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+
| medium    | lokaal bereik (10-50m)  | Mbytes/s      | WiFi      | lokale/publieke broker   | MQTT              |
+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+
| low       | lokaal bereik (50-200m) | 50kbits/s (*) | RFM69     | lokale gateway; broker   | MQTT              |
+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+
| low       | niet-lokaal (enkele km) | 1 kbit/s (*)  | LoRa      | publieke gateway; broker | MQTT              |
+-----------+-------------------------+---------------+-----------+--------------------------+-------------------+

(*) voor LoRa is de bitrate nog lager bij een groot bereik.
Bovendien mogen RFM69 en Lora-radio's max. 1% van de tijd zenden.

(**) protocol: IoT-knoop <-> NodeRed/web-app

Gateway
=======

.. figure:: IoT-0-stacks-gateway.png
   :width: 400 px
   :align: center

   IoT-gateway

Soms kunnen we de IoT-knopen niet zomaar in het internet verbinden,
bijvoorbeeld omdat deze een veel eenvoudiger protocol gebruiken.
We gebruiken dan een gateway om het IoT-knoop-protocol om te zetten naar het IP-protocol, en omgekeerd.

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

NodeRed
=======

.. figure:: Nodered-chat-flow.png
   :width: 500 px
   :align: center

   NodeRed Chat flow

IoT-toepassingen combineren vaak data uit verschillende bronnen:
vanuit verschillende netwerken met IoT-knopen, maar ook uit andere databases of datastromen.
Deze data kunnen door allerlei diensten (Data Science, Artificial Intelligence, enz.) verwerkt worden voordat deze bruikbaar zijn in een toepassing.
Deze databronnen, diensten en gebruikerstoepassingen gebruiken verschillende protocollen en formaten.
Met NodeRed knoop je deze verschillende onderdelen samen, op een grafische manier.


Dashboard
=========

.. figure:: Nodered-dashboard-display-0.png
   :width: 500 px
   :align: center

   Voorbeeld van een dashboard

Een IoT-toepassing heeft vaak een gebruikersinterface,
bijvoorbeeld in de vorm van een *dashboard* met een samenvatting van de gegevens van de IoT-knopen.
In onze voorbeeld-toepassing werken we met een eenvoudig dashboard met de gegevens van één IoT-knoop.
Dit dashboard maken we met NodeRed: we gebruiken deze dan als web-server.
Bovendien kunnen we via dit dashboard de actuators van de IoT-knoop bedienen.
Dit gebruikersinterface heeft de vorm van een webtoepassing ("app"), beschikbaar via een server in het publieke web.
