**********
Opdrachten
**********

.. voor lora-knopen


LoRa-Dashboard
==============

Met NodeRed kun je de data van een TTN-toepassing gebruiken, bijvoorbeeld in een dashboard.
In de onderstaande flow maak je de elementaire koppelingen.
Daarmee maken we een dashboard voor een LoRa-knoop.

Voor de koppeling van NodeRed aan de TTN-server zijn de volgende knopen beschikbaar:

+--------------------+------------------+----------------+------------------------+
| **figuur**         | **naam**         | **soort**      | **betekenis**          |
+--------------------+------------------+----------------+------------------------+
| |ttn-uplink|       | ttn-uplink       |  input         | data van ttn-knopen    |
+--------------------+------------------+----------------+------------------------+
| |ttn-event|        | ttn-event        |  input         | events(*)              |
+--------------------+------------------+----------------+------------------------+
| |ttn-downlink|     | ttn-downlink     |  output        | data naar ttn-knopen   |
+--------------------+------------------+----------------+------------------------+

.. |ttn-uplink| image:: nodered-ttn-uplink-node.png
.. |ttn-downlink| image:: nodered-ttn-downlink-node.png
.. |ttn-event| image:: nodered-ttn-event-node.png

Deze knopen gebruiken het MQTT-interface van de TTN-server.
Dit interface is beschreven in: https://www.thethingsnetwork.org/docs/applications/mqtt/quick-start.html

* de *uplink*-node ontvangt de data van een TTN-toepassing of van een enkel device (IoT-knoop).
* via de *downlink*-node kun je data naar een device (IoT-knoop) sturen.
  (Dit is binnen een LoRaWan-netwerk maar heel beperkt mogelijk.)
* de *event*-node ontvangt de meta-data van de TTN-server over een TTN-toepassing of een device (IoT-knoop).
  Een typisch voorbeeld van een event is de registratie van een device bij een gateway.

(De structuur van een topic is: ``<app-id>/devices/<dev-id>/up`` etc.; de id-s zijn unieke namen.)

* https://www.youtube.com/watch?v=JrNjY-pGuno&list=PLM8eOeiKY7JVwrBYRHxsf9p0VM_dVapXl&index=1

Alternatief: http integration ("push based...")

* https://www.youtube.com/watch?v=Uebcq7xmI1M&list=PLM8eOeiKY7JVwrBYRHxsf9p0VM_dVapXl&index=2

.. rubric:: installeren van de TTN-nodes in NodeRed

Voordat je de TTN-nodes kunt gebruiken moet je deze (eenmalig) installeren in NodeRed.
Dit doe je op de volgende manier:

* selecteer in het NodeRed UI in het "hamburger menu" rechts boven: "Manage Palette".
*

(Als "Manage Palette" niet beschikbaar is, moet de NodeRed-installatie aangepast worden.)
