****************
WiFi-MQTT-knopen
****************

In dit hoofdstuk behandelen we IoT-knopen gebaseerd op WiFi radio's.
Deze knopen communiceren via het MQTT-protocol met de IoT-toepassing (app).
MQTT is een veel gebruikt protocol voor het het Internet of Things.
Dit protocol heeft een aantal voordelen ten opzichte van HTTP:

* de interactie via MQTT is *publish-subscribe* tussen MQTT-clients en de MQTT-broker,
  in plaats van *client-server* zoals bij HTTP;
* dit maakt het "pushen" van berichten in beide richtingen mogelijk:
  van IoT-knoop naar IoT-toepassing, en van toepassing naar IoT-knoop;
* MQTT is daarmee geschikt voor het bewaken van sensoren (monitoring) en voor het besturen van actuatoren (control).
* alleen de MQTT-broker moet altijd online zijn;
  clients zoals IoT-knopen of toepassingen hoeven dat niet.
  Dit heeft grote voordelen voor draadloze IoT-knopen.

Als payload voor MQTT gebruiken we in onze voorbeelden JSON,
meestal in een variant afgeleid van het Cayenne Low Power Payload (LPP) formaat.

.. toctree::
  :hidden:

  wifi/wifi-keten.rst
  wifi/mqtt-interactie.rst
  wifi/wifi-protocol.rst
  wifi/json-lpp.rst
  wifi/opdrachten.rst
  wifi/nodered-opdrachten.rst
  wifi/toetsvragen.rst
