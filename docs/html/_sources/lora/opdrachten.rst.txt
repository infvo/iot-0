**********
Opdrachten
**********

.. voor lora-knopen

TTN-application
===============

.. admonition:: Wat heb je nodig?

  * een (gratis) TTN-account

In deze opdracht bestudeer


LoRa-Dashboard
==============

.. admonition:: Wat heb je nodig?

  * een (gratis) FRED-account, of andere NodeRed-server
  *

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

.. rubric:: Installeren van de TTN-nodes in NodeRed

Voordat je de TTN-nodes kunt gebruiken moet je deze (eenmalig) installeren in NodeRed.
Dit doe je op de volgende manier:

* selecteer in het NodeRed UI in het "hamburger menu" rechts boven: "Manage Palette".
* selecteer de tab "install", en type in het zoekveld: ``ttn``
* selecteer het item ``node-red-contrib-ttn``, en klik op "install".
* als het goed is, worden nu de knopen geïnstalleerd.

(Als "Manage Palette" niet beschikbaar is, moet de NodeRed-installatie aangepast worden.)

In het geval van een FRED-account (https://fred.sensetecnic.com) voor gebruik je de volgende stappen:

* selecteer in het FRED-menu links onder "Tools": "Add or remove nodes";
* type in het zoekveld: ``ttn``
* vink aan: TTN - TheThingsNetwork NodeRed application

Dashboard-flow
--------------

.. figure:: iot-ttn-dashboard-flow.png
  :align: center
  :width: 400px

  Dashboard-flow voor TTN

* kopieer de onderstaande NodeRed-flow naar een leeg tabblad.

.. code-block:: json

  [{"id":"36a23e12.6d3342","type":"ttn uplink","z":"ca97d5d6.8e9618","name":"infvo-iot-22feb","app":"91e8325e.8b12b8","dev_id":"mini-node-1","field":"","x":140,"y":140,"wires":[["e6534f3f.0c4088","f449ffb1.c115e","3ffbf776.9a3db","541347a.fc367b8","4f17a475.9264d4"]]},{"id":"e6534f3f.0c4088","type":"debug","z":"ca97d5d6.8e9618","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","x":410,"y":140,"wires":[]},{"id":"f449ffb1.c115e","type":"ui_gauge","z":"ca97d5d6.8e9618","name":"Temperature","group":"397ba453.3b0e0c","order":0,"width":0,"height":0,"gtype":"gage","title":"Temp","label":"'C","format":"{{payload.celcius}}","min":0,"max":"50","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":450,"y":220,"wires":[]},{"id":"3ffbf776.9a3db","type":"ui_gauge","z":"ca97d5d6.8e9618","name":"","group":"febde8db.f65de8","order":0,"width":0,"height":0,"gtype":"gage","title":"Barometer","label":"hPa","format":"{{payload.mbar}}","min":"970","max":"1040","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":450,"y":260,"wires":[]},{"id":"e4c72a9.9f78458","type":"ui_chart","z":"ca97d5d6.8e9618","name":"Temperature","group":"397ba453.3b0e0c","order":0,"width":0,"height":0,"label":"Temperature","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","dot":false,"ymin":"0","ymax":"50","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":450,"y":320,"wires":[[],[]]},{"id":"b3522a84.fddf48","type":"ui_chart","z":"ca97d5d6.8e9618","name":"Barometer","group":"febde8db.f65de8","order":0,"width":0,"height":0,"label":"Barometer","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","dot":false,"ymin":"990","ymax":"1030","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":450,"y":360,"wires":[[],[]]},{"id":"541347a.fc367b8","type":"change","z":"ca97d5d6.8e9618","name":"select payload.celcius","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.celcius","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":240,"y":320,"wires":[["e4c72a9.9f78458"]]},{"id":"4f17a475.9264d4","type":"change","z":"ca97d5d6.8e9618","name":"select payload.mbar","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.mbar","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":240,"y":360,"wires":[["b3522a84.fddf48"]]},{"id":"91e8325e.8b12b8","type":"ttn app","z":"","appId":"infvo-iot-22feb","accessKey":"ttn-account-v2.EaP_sntUikm8s1jdZ9Z5gTYegxUrFYdcpLVEUhAjy-A","discovery":"discovery.thethingsnetwork.org:1900"},{"id":"397ba453.3b0e0c","type":"ui_group","z":"","name":"TTN-device-1-temperature","tab":"18f86ddf.f7110a","disp":true,"width":"6","collapse":false},{"id":"febde8db.f65de8","type":"ui_group","z":"","name":"TTN-device-1-barometer","tab":"18f86ddf.f7110a","disp":true,"width":"6","collapse":false},{"id":"18f86ddf.f7110a","type":"ui_tab","z":"","name":"TTN-device-1","icon":"dashboard"}]

* configureer de TTN-uplink-node:
    * vul de ID van de TTN-application in
    * vul de ID van het device (IoT-knoop) in
* configureer de debug-node, met output: complete msg object
* "deploy"

Je ziet nu (als het goed is) in het dashboard de gegevens van de IoT-knoop verschijnen.

Via de debug-node kun je de metadata van de communicatie tussen de IoT-knoop en de gateway volgen.
Een voorbeeld hiervan zie je hieronder:

.. figure:: lora-metadata.png
  :width: 300px
  :align: center

  Metadata voor TTN-LoRaWan-communicatie

* welke gateway(s) ontvangen de berichten van deze IoT-knoop?
* welke SF wordt gebruikt?
* wat is de (geschatte) *air time* van het bericht?

(NB: deze gegevens kun je ook bestuderen in het TTN-console; waarom zou je dat hier doen?)

LoRaWan-toepassingen
====================

* zoek een aantal voorbeelden van LoRaWan-toepassingen
* ga voor één van deze toepassingen na:
    * welke sensordata moeten verstuurd worden?
    * hoe vaak zouden deze verstuurd moeten worden?
    * wat zijn de voordelen van deze toepassing? voor wie?
    * wat zijn de mogelijke nadelen? voor wie?

Zie ook:

* https://www.thethingsnetwork.org/labs/
* https://www.thethingsnetwork.org/marketplace
    * o.a. "a better mousetrap"
* https://www.kpn.com/zakelijk/grootzakelijk/internet-of-things/lora-netwerk.htm
