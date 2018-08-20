MQTT-ketens
===========

Publieke broker
---------------
Met een MQTT-broker in het publieke netwerk kunnen we toepassingen in het internet koppelen aan lokale IoT-knopen.
Via deze broker kunnen we de IoT-knoop op afstand bedienen, bijvoorbeeld via een web-toepassing.

.. figure:: IoT-nobridge-1.png
   :width: 600 px
   :align: center

   IoT-knopen met MQTT-broker in het publieke internet

De IoT-knoop publiceert de sensorwaarden naar de broker,
en deze stuurt deze berichten door naar de clients die zich op deze berichten geabonneerd hebben.

Deze keten is eenvoudig op te zetten.
Er kleven wel enkele nadelen aan: (i) de latency kan te groot worden om lokale apparaten te bedienen;
en (ii) de communicatie verloopt via het publieke internet, met meer veiligheidsrisico's dan het lokale netwerk.

Lokale broker
-------------

.. figure:: IoT-lokale-gateway-1.png
   :width: 600 px
   :align: center

   Lokale MQTT-broker als bridge naar publieke broker

Door het gebruik van een lokale broker kunnen we de lokale interacties binnen het lokale netwerk houden:
dit komt de latency en de veiligheid ten goede.

Een lokale broker kun je ook gebruiken als *bridge* naar een publieke broker:
op die manier kun je de voordelen van lokale en publieke brokers combineren.
