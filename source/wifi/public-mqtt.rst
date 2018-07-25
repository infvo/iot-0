****************************
MQTT in het publieke netwerk
****************************

Als we de MQTT-broker in het publieke netwerk plaatsen,
dan kunnen we allerlei toepassingen in het internet aan deze broker koppelen.
In het bijzonder kunnen we via deze broker de IoT-knoop op afstand bedienen,
bijvoorbeeld via een web-toepassing.

We gebruiken de volgende configuratie:

.. figure:: IoT-nobridge-1.png
   :width: 600 px
   :align: center

   IoT-knopen met MQTT-broker in het publieke internet

De IoT-knoop publiceert de sensorwaarden naar de broker,
en deze stuurt deze berichten door naar de clients die zich op deze berichten geabonneerd hebben.

Omgekeerd stuurt een toepassing een bericht, bijvoorbeeld voor het aansturen van een LED,
naar de broker.
De broker stuurt dit door naar de IoT-knoop, die op deze actuator-berichten geabonneerd is.
