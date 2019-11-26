MQTT-ketens
===========

Publieke broker
---------------
Met een MQTT-broker in het publieke internet kun je toepassingen in het internet koppelen aan lokale IoT-knopen.
Via deze broker kun je de IoT-knoop op afstand bedienen, bijvoorbeeld via een web-toepassing.

.. figure:: IoT-keten-WiFi-MQTT.png
   :width: 600 px
   :align: center

   IoT-knopen met MQTT-broker in het publieke internet

De IoT-knoop publiceert de sensorwaarden naar de broker,
en deze stuurt deze berichten door naar de clients die zich op deze berichten geabonneerd hebben.
Omgekeerd stuurt de web-toepassing instellingen voor de actuatoren naar de broker;
deze stuurt de berichten door naar de clients (IoT-knopen) die op deze berichten geabonneerd zijn.
De communicatie tussen de IoT-knopen en de web-toepassing via de MQTT-broker verloopt *symmetrisch*.

Deze keten is eenvoudig op te zetten.
Er kleven wel enkele nadelen aan: (i) de latency via het publieke internet kan te groot worden om lokale apparaten te bedienen;
en (ii) de communicatie verloopt via het publieke internet, met meer veiligheidsrisico's dan het lokale netwerk.

Lokale broker
-------------

.. figure:: IoT-keten-local-broker.png
   :width: 600 px
   :align: center

   Lokale MQTT-broker en lokale controller

Door het gebruik van een (extra) lokale broker houd je de lokale interacties binnen het lokale netwerk:
dit komt de latency en de veiligheid ten goede.

De lokale broker kun je ook gebruiken als *bridge* naar een publieke broker:
op die manier combineer je de voordelen van lokale en publieke brokers.
