**************
(MQTT en JSON)
**************

..todo::

  De stof van dit hoofdstuk wordt verdeeld over de andere hoofdstukken.
  Als dat compleet is, kan dit weg.

.. admonition:: Leerdoelen en concepten

 * PubSub interactie: client-broker (versus client-server)
 * push versus pull; polling;
 * MQTT protocol; publish; subscribe; message; topic; wildcard;
 * JSON formaat


.. rubric:: Inleiding

De aanpak met een webserver als interface van de IoT-knoop heeft een aantal nadelen:

* de IoT-knoop moet altijd beschikbaar (online) zijn;
  voor een draadloze IoT-knoop kost een permanente radioverbinding veel batterij-energie;
* de client-server interactie via het HTTP-protocol is een "pull" van de gegevens van de IoT-knoop;
  veranderingen in de sensorgegevens detecteer je door deze regelmatig bij de server op te vragen ("polling").
  Dit kost veel onnodige communicatie (bandbreedte) en daarmee energie.
* de IoT-knoop-webserver is zonder verdere maatregelen alleen in het lokale netwerk te benaderen.
* het http-protocol is minder geschikt voor grote aantallen kleine berichten, zoals we die voor de IoT-knopen nodig hebben.

We gebruiken in dit hoofdstuk een ander protocol: MQTT (http://mqtt.org/).
Dit protocol is speciaal ontwikkeld voor IoT-toepassingen.
Bij dit protocol is er sprake van een MQTT-broker en MQTT-clients:
de broker fungeert als tussenstation voor de communicatie tussen de clients.

MQTT is een ''publish/subscribe protocol'': de clients van een MQTT-broker kunnen zowel berichten publiceren als ontvangen.
Een IoT-knoop publiceert berichten met sensorwaarden naar de MQTT-broker;
deze zorgt voor de verdere distributie ("push") naar de clients die in die waarden geïnteresseerd zijn.

In dit hoofstuk gebruiken we een ''publieke MQTT-broker''.
Deze vormt de schakel tussen de IoT-knopen en de toepassing.
Hiermee kunnen we de sensordata van de IoT-knopen ontvangen en de actuatoren op afstand besturen.

.. rubric:: Links

* https://mosquitto.org/man/mosquitto-conf-5.html
* https://www.cloudmqtt.com/docs-bridge.html
* https://www.cloudmqtt.com/

.. figure:: mqtt/MQTT.png
   :width: 600 px
   :align: center

   MQTT voorbeeld

.. rubric:: Inhoud

.. toctree::

  mqtt/mqtt-pubsub.rst
  mqtt/json.rst
  mqtt/opdrachten.rst
  mqtt/toetsvragen.rst
