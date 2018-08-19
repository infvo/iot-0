***********
WiFi-knopen
***********

In dit hoofdstuk behandelen we IoT-knopen gebaseerd op WiFi radio's.

In het vorige hoofdstuk hebben we een IoT-knoop met een webserver behandeld.
Deze aanpak heeft de volgende nadelen:

* de webserver moet altijd "online" zijn; voor IoT-knopen met batterijvoeding is dat lastig;
* de webserver is alleen lokaal toegankelijk: je kunt het apparaat niet op grote afstand bedienen;
* de client-server interactie tussen browser en webserver betekent dat de browser steeds om de sensorwaarden moet vragen ("pull");
  de webserver kan niet direct een bericht aan de browser sturen.

In dit hoofdstuk gebruiken we in plaats van HTTP het MQTT-protocol:
dit is een veelgebruikt protocol voor het Internet of Things.
Met dit protocol kunnen we de bovenstaande problemen oplossen.

.. toctree::
  :hidden:

  wifi/wifi-keten.rst
  wifi/wifi-protocol.rst 
  wifi/local-mqtt.rst
  wifi/public-mqtt.rst
  wifi/opdrachten.rst
  wifi/toetsvragen.rst
