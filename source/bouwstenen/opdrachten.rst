**********
Opdrachten
**********

.. voor IoT-bouwstenen


.. rubric:: MQTT-broker in het publieke internet

We gebruiken in deze opdracht een publieke MQTT-broker.
Deze broker fungeert ook als webserver voor de statische webtoepassing ``mqttt``.

.. topic:: Statische webtoepassing

  Bij een statische webtoepassing levert de server alleen de bestanden;
  alle actie en interactie vindt plaats in de browser, via JavaScript.
  De server gebruikt geen server-scripting (zoals PHP of Python) en geen server-database.

.. _MQTTT:

.. rubric:: De app MQTTT

Bij veel opdrachten gebruiken we de app MQTTT:
dit is een mqtt-client waarmee we mqtt-berichten kunnen sturen en ontvangen.
Door de aard van het mqtt-protocol kunnen we daarmee ook *topics* "afluisteren".

.. figure:: IoT-mqtt0-app.png
   :width: 400 px
   :align: center

   MQTTTest app

De  web-app kun je vinden via de broker: http://infvopedia.nl:1884/mqttt.html.
Hierin is ``infvopedia.nl`` de domeinnaam van de broker.
``1884`` is het HTTP-poortnummer van de broker: de broker fungeert ook als (statische) webserver.
MQTTT communiceert met de MQTT-broker via het *websockets* protocol (via poort 1884).
Het MQTT-protocol gebruikt poort ``1883``.

De app heeft de volgende invoervelden en knoppen:

* ``IoT-node``: de ID van de IoT-knoop waarvan je de sensordata wilt ontvangen
    * knoppen On en Off: publiceren berichten met topic ``node/ID/actuators``,
      om led0 aan- en uit te schakelen
    * de sensordata worden ontvangen via topic ``node/ID/sensors``
    * de ontvangen sensordata worden in tabelvorm weergegeven
* ``subscribe to topic``: het topic waarvan je de berichten wilt ontvangen
    * daaronder verschijnen de ontvangen berichten
    * in het topic kun je ook wildcard-tekens opnemen, zoals ``+`` en ``#``.
* ``publish to topic``: het topic waarvoor je berichten wilt versturen:
    * het bericht voer je daaronder in, en
    * met de "publish"-knop verstuur je het bericht.

.. admonition:: Let op

  Een nieuwe waarde in een invoerveld wordt actief zodra de cursor dat invoerveld verlaat.
  Je ziet dan geen blauwe rand meer om het veld.

  Als je een nieuw topic invult of een nieuwe IoT-node ID,
  vindt er automatisch een *subscribe* op dit nieuwe topic plaats,
  en een *unsubscribe* op het vorige topic.
  Na het invullen verschijnen dan automatisch de ontvangen berichten voor dat nieuwe topic.


.. rubric:: Alternatief: MQTT-box

Er zijn ook desktop-applicaties waarmee je deze opdrachten kunt uitvoeren.
Een voorbeeld is MQTTbox (http://workswithweb.com/mqttbox.html).


1. Een eerste IoT-keten
=======================

Een eerste voorbeeld van een IoT-keten, van IoT-knoop tot toepassings ("app"), bestaat uit de volgende onderdelen:

* een gesimuleerde IoT-knoop
    * http://infvopedia.nl:1884/iotnode-app.html
* een MQTT-broker in het publieke internet;
* het programma MQTTT, waarmee je het mqtt-verkeer van de sensor kunt bekijken.
    * http://infvopedia.nl:1884/mqttt.html

.. figure:: Iotnode-simulator-0.png
   :width: 400 px
   :align: center

   IoT-knoop simulator

.. rubric:: Opdracht 1.1 

Voer de onderstaande stappen uit:

1. open de gesimuleerde IoT-knoop op in een browservenster
2. open het programma MQTTT in een ander browservenster
    * deze opzet werkt het best met twee browservensters naast elkaar.
3. voer in het "IoT-node"-venster van MQTTT de nodeID in van de gesimuleerde IoT-knoop
4. druk in MQTTT op de knop om de LED (led0) aan (of uit) te zetten
    * in MQTTT zie je de waarden van de sensoren in tabelvorm verschijnen
    * je ziet in de gesimuleerde IoT-knoop de linker LED aan (of uit) gaan.
5. verander één van de sliders van de IoT-knoop
    * je ziet nu (na verloop van tijd) de berichten met de nieuwe waarde langskomen.
6. druk één van de knoppen op de (gesimuleerde) IoT-knoop in
    * wat gebeurt er?

Opmerkingen:

* Mogelijk zie je ook berichten van andere IoT-knopen langskomen:
  die gebruiken dezelfde MQTT-broker,
  en via ``subscribe: +/+/+`` ontvangt MQTTT de berichten van alle knopen.
* De IoT-knoop-simulator verstuurt ca. elke 60 seconden de waarden van de lokale sensoren;
  dit zullen we later ook bij de hardware-IoT-knopen zien.
* je kunt meerdere (gesimuleerde) IoT-knopen hebben met dezelfde node-ID:
  deze zijn op het MQTT-niveau niet van elkaar te onderscheiden.

2. een tweede IoT-keten
=======================

.. todo::

  Verder uitwerken - ook de infrastructuur.

  * gebruik van een IoT-dashboard
      * NB: uiteindelijk moeten we een dashboard zien te bieden waarin meerdere sensoren/IoT-knopen gecombineerd worden?
      * gebruik van IoT-dashboard voor gegeven knopen (elders)
      * gebruik van IoT-dashboard voor gesimuleerde knopen
