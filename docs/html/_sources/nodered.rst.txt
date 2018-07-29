*******
NodeRed
*******

.. admonition:: Leerdoelen en concepten

  * Kennismaking met NodeRed, voor het configureren van datastromen, koppelen van data, acties en diensten;
  * NodeRed voorbeeld: dashboard van sensordata
  * NodeRed voorbeeld: bediening van de actuatoren (LED)

De module "web voor makers" geeft een uitgebreidere handleiding in het gebruik van NodeRed.

.. admonition:: Wat heb je nodig?

  * NodeRed-installatie met daarin geïnstalleerd:
      * UI-nodes (voor dashboard)
      * TTN-nodes
  * sensordata
      * via MQTT van een publieke broker, of
      * via MQTT van een lokale broker (alleen met een lokale NodeRed-installatie)
  * IoT-knoop
      * een eigen hardware IoT-knoop; of
      * een gesimuleerde IoT-knoop; of
      * sensordata van een IoT-knoop elders.

Nodes en flows
==============

.. figure:: nodered/Nodered-hello.png
   :width: 600 px
   :align: center

   NodeRed http flow-voorbeeld

Een *flow* in NodeRed bestaat uit een netwerk van *nodes* en *verbindingen*.
Het aansluitpunt (bolletje) aan de linkerkant van een node is de input.
Een node zonder aansluiting links is een *input-node*, met een externe input, bijvoorbeeld een http-request.
De outputs staan aan de rechterzijde van de node.
Een node zonder aansluiting rechts is een *output-node*, met een externe output, bijvoorbeeld een http-response.

Een NodeRed-toepassing kan uit meerdere flows bestaan: elke flow heeft een eigen pagina (tab).

+--------------------+------------------+------------------+
| **figuur**         | **naam**         | **soort node**   |
+--------------------+------------------+------------------+
| |http-input-node|  | http-input-node  |  input           |
+--------------------+------------------+------------------+
| |http-output-node| | http-output-node |  output          |
+--------------------+------------------+------------------+
| |template-node|    | template-node    |  in-out          |
+--------------------+------------------+------------------+

.. |http-input-node| image:: nodered/nodered-http-input-node.png
.. |http-output-node| image:: nodered/nodered-http-output-node.png
.. |template-node| image:: nodered/nodered-template-node.png

**Hoe werkt een flow?**
Als een node een bericht (message) krijgt via de input,
dan voert deze node daarop een bewerking uit,
en genereert één of meer messages naar de output(s).
Deze output is weer verbonden  met de input van een andere node;
of de node is een output-node, met een externe output.

*Voor het bovenstaande flow-voorbeeld*: (i) de http-input-node ontvangt een http-request als
de http-method gelijk is aan ``get`` en het URL-pad gelijk is aan ``/hello``.
Deze http-input-node stuurt dan een message met dit request naar
(ii) de template-node ``hello.html``.
Deze genereert de bijbehorende output: een html-document,
en stuurt een message met dit document naar
(iii) de http-output-node, die uit de message de bijbehorende response samenstelt.
Deze node stuurt de response naar de afzender van het http-request.


Je eerste flow
==============

Met deze eerste flow kun je zien of alles werkt:

.. figure:: nodered/Nodered-flow1.png
   :width: 500 px
   :align: center

   NodeRed: eerste flow

Hiervoor gebruik je de volgende nodes:

+----------------+---------------+------------------+
| **figuur**     | **naam**      | **soort node**   |
+----------------+---------------+------------------+
| |inject-node|  | inject-node   |  input           |
+----------------+---------------+------------------+
| |debug-node|   | debug-node    |  output          |
+----------------+---------------+------------------+

.. |inject-node| image:: nodered/inject-node.png
.. |debug-node| image:: nodered/debug-node.png

.. rubric:: Opdracht

Voer de onderstaande opdrachten uit in een lege (flow)tab in NodeRed.

* sleep een inject-node vanuit de lijst met nodes links naar het lege vlak in het midden
* plaats op dezelfde manier de debug-node;
* verbind de output (rechts) van de inject-node met de input (links) van de debug-node.
* activeer deze flow (rechts boven: Deploy)
* test deze flow, door op het knopje links op de input-node ("timestamp") te klikken.

Als het goed is, krijg je in het debug-venster rechts nu de output van deze flow te zien. Je maakt het debug-venster zichtbaar via de debug-tab.

* verander de configuratie van de inject-knoop: zorg ervoor dat deze elke minuut een timestamp oplevert.
    * de configuratie van een knoop krijg je te zien door een dubbel-klik op die knoop.
* verander de configuratie van de inject-knoop: zorg ervoor dat deze een tekst levert als payload.
* verbind meerdere inject-knopen met dezelfde debug-knoop.

Importeren van een flow
=======================

Bij de praktische opdrachten gebruik je flows die eerder gemaakt zijn.

Op de volgende manier importeer je een flow vanuit een JSON-vorm:

* selecteer en kopieer de flow in JSON-vorm naar het clipboard
    * met de "Copy" van je host-Operating System;
* selecteer in het hamburger-menu->Import->Clipboard (rechts);
* kopieer ("Paste") de inhoud van het clipboard in het venster;
* klik op "Import"

.. rubric:: Opdracht

1. Importeer de onderstaande flow in NodeRed:

.. code-block:: json

  [{"id":"678b8c4c.974984","type":"inject","z":"b7f5ac90.8cf17","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":146,"y":80,"wires":[["654b6309.c742ec","d272daf8.c48e38"]]},{"id":"65beec84.75ffe4","type":"debug","z":"b7f5ac90.8cf17","name":"","active":true,"console":"false","complete":"false","x":502,"y":81,"wires":[]},{"id":"654b6309.c742ec","type":"delay","z":"b7f5ac90.8cf17","name":"","pauseType":"delay","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":323.5,"y":82,"wires":[["65beec84.75ffe4"]]},{"id":"d272daf8.c48e38","type":"debug","z":"b7f5ac90.8cf17","name":"","active":true,"console":"false","complete":"false","x":323.5,"y":134,"wires":[]}]


2. test deze flow.

MQTT-nodes
==========

+--------------------+------------------+------------------+
| **figuur**         | **naam**         | **soort node**   |
+--------------------+------------------+------------------+
| |mqtt-input-node|  | mqtt-input-node  |  input           |
+--------------------+------------------+------------------+
| |mqtt-output-node| | mqtt-output-node |  output          |
+--------------------+------------------+------------------+
| |mqtt-broker-node| | mqtt-broker-node |  configuration   |
+--------------------+------------------+------------------+

.. |mqtt-input-node| image:: nodered/nodered-mqtt-input-node.png
.. |mqtt-output-node| image:: nodered/nodered-mqtt-output-node.png
.. |mqtt-broker-node| image:: nodered/nodered-mqtt-broker-node.png

In de volgende voorbeelden gebruiken we de MQTT-input- en output-nodes.
Deze nodes configureer je met de gebruikte mqtt-broker en het topic.

* de MQTT-input-node heeft het ontvangen bericht als resultaat (payload);
* de MQTT-output-node stuurt het bericht (``msg.payload``) naar de broker, met het genoemde topic.
* er is een aparte *configuratie-node* voor de MQTT-broker.
  Deze gebruik je indirect bij het configureren van de MQTT-input- of output-node.
  Je kunt deze broker-node ook vinden via het rechter "hamburger" menu: Configuration Nodes.

MQTT sensor flow
================

In dit eerste voorbeeld gebruik je de MQTT-input-node om de sensorwaarden van een IoT-knoop te ontvangen.
De ontvangen waarden vind je in het debug-venster.
Om deze flow te gebruiken moet je eerst de verschillende nodes configureren (dubbelklik op de node):

* de  *nodeid* in het topic van de MQTT-input-node verander je in de *nodeid* van je eigen IoT-knoop;
  voorbeeld: ``node/e0f1/sensors``
* de MQTT-broker van de MQTT-input-node configureer je met de gegevens van je MQTT-broker;
* "deploy" de aangepaste flow. De MQTT-node moet nu melden dat deze "connected" is.
  We gebruiken in dit voorbeeld een JSON-node:
  deze zet een JSON-string-vorm om in een JavaScript-object.
  In het debug-venster vind je dan beide vormen terug.
* schakel één van de beide debug-nodes uit met de knop aan de rechterkant (en **deploy**!).
  Je ziet dan maar één van beide vormen in het debug-venster.

.. figure:: nodered/mqtt-sensor-flow-0.png
   :width: 600 px
   :align: center

   MQTT sensor flow


Flow:

.. code-block:: json

  [{"id":"db8775b0.d5ebf8","type":"mqtt in","z":"ffc7967f.8cd98","name":"","topic":"node/ec54/sensors","qos":"2","broker":"f4b28537.29eb48","x":190,"y":120,"wires":[["553ee431.775ac4","d065b3dd.226998"]]},{"id":"553ee431.775ac4","type":"debug","z":"ffc7967f.8cd98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":430,"y":120,"wires":[]},{"id":"8675f8e8.eb7ff8","type":"debug","z":"ffc7967f.8cd98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":200,"wires":[]},{"id":"d065b3dd.226998","type":"json","z":"ffc7967f.8cd98","name":"","property":"payload","action":"","pretty":false,"x":410,"y":200,"wires":[["8675f8e8.eb7ff8"]]},{"id":"f4b28537.29eb48","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]


MQTT actuator flow
==================

In dit voorbeeld gebruik je de MQTT-output-node.
Hiermee stuur je JSON berichten naar een IoT-knoop.

* configureer de MQTT-output-node voor je IoT-knoop (nodeid) en voor je MQTT-broker.
* bij de Inject-node selecteer je voor de payload het JSON-alternatief: <code>{}</code>.
* voor het in- en uitschakelen gebruik je de teksten <code>{"led0":1}</code> en <code>{"led0":0}</code>
* controleer bij de IoT-knoop of de LED aan- en uitgaat;
* controleer eventueel de MQTT-berichten met de MQTT0-app.

.. figure:: nodered/mqtt-actuator-flow-0.png
   :width: 600 px
   :align: center

   MQTT actuator flow

.. code-block:: json

  [{"id":"c9b1e2b5.78e5e","type":"inject","z":"ffc7967f.8cd98","name":"","topic":"","payload":"{\"led0\": 1}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":180,"y":300,"wires":[["ab359f6d.215e78"]]},{"id":"ab359f6d.215e78","type":"mqtt out","z":"ffc7967f.8cd98","name":"","topic":"node/ec54/actuators","qos":"","retain":"","broker":"f4b28537.29eb48","x":460,"y":300,"wires":[]},{"id":"93df0f8c.4a27","type":"inject","z":"ffc7967f.8cd98","name":"","topic":"","payload":"{\"led0\": 0}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":180,"y":360,"wires":[["ab359f6d.215e78"]]},{"id":"f4b28537.29eb48","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]


Sensor dashboard
================

Met een sensor-dashboard kun je de waarden van de sensoren via een browser bekijken.

.. figure:: nodered/Nodered-dashboard-display-0.png
   :width: 600 px
   :align: center

   NodeRed dashboard display

NodeRed biedt de bouwstenen voor het maken van een eenvoudig dashboard. We gebruiken in het voorbeeld de volgende knopen:

+--------------------+------------------+----------------+------------------------+
| **figuur**         | **naam**         | **soort**      | **betekenis**          |
+--------------------+------------------+----------------+------------------------+
| |dashboard-gauge|  | dashboard-gauge  |  output        | meter (actuele waarde) |
+--------------------+------------------+----------------+------------------------+
| |dashboard-chart|  | dashboard-chart  |  output        | grafiek (verloop)      |
+--------------------+------------------+----------------+------------------------+

.. |dashboard-gauge| image:: nodered/nodered-dashboard-gauge.png
.. |dashboard-chart| image:: nodered/nodered-dashboard-chart.png

Het sensor-dashboard gebruikt de sensorwaarden die de IoT-knoop verstuurt, in JSON-formaat.
Deze sensorwaarden selecteren we uit het JSON-bericht.
Sommige waarden schalen we om deze op de gebruikelijke manier te presenteren (bijvoorbeeld: hPa voor de luchtdruk).

De flow voor een dashboard voor een enkele sensor:

.. figure:: nodered/Nodered-dashboard-0.png
   :width: 600 px
   :align: center

   NodeRed dashboard flow

Uitleg bij deze flow:

* de MQTT-input-node ontvangt de sensor-berichten van de IoT-knoop via de MQTT-broker;
* de JSON-node zet de JSON-string-vorm om in een JavaScript object
* de Change-nodes gebruiken we voor het selecteren van de "temp" respectievelijk "pres"-eigenschap in dit JavaScript object
* in het geval van "temp" gebruiken we deze waarde voor de meter (Gauge) en de grafiek (Cart).
* in het geval van "pres" moeten we de waarde eerst aanpassen, van Pascal naar hectoPascal (delen door 100). Dit doen we met de range-node.
* de aangepaste luchtdruk-waarde gebruiken we voor de luchtdrukmeter- en de grafiek-nodes.

Hieronder staat de flow in JSON-notatie.
Deze kun je met Copy-Paste overbrengen en vervolgens importeren in je NodeRed-editor.

.. code-block:: json

  [{"id":"46ecec97.f7e234","type":"ui_gauge","z":"338f5858.dee25","name":"","group":"52cd25cb.3136fc","order":0,"width":0,"height":0,"gtype":"gage","title":"Temperatuur","label":"'C","format":"{{payload}}","min":0,"max":"50","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":722,"y":174,"wires":[]},{"id":"1bca6974.5a9e1f","type":"ui_chart","z":"338f5858.dee25","name":"","group":"437f7191.421f08","order":0,"width":0,"height":0,"label":"Temperatuur","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","dot":false,"ymin":"0","ymax":"50","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":726,"y":231,"wires":[[],[]]},{"id":"23c29a4e.f82bf6","type":"json","z":"338f5858.dee25","name":"","property":"payload","action":"","pretty":false,"x":161,"y":238,"wires":[["87b7c751.9b976","b955a49e.bc3438"]]},{"id":"d32397f5.9883c8","type":"mqtt in","z":"338f5858.dee25","name":"","topic":"node/ec54/sensors","qos":"2","broker":"f4b28537.29eb48","x":167,"y":128,"wires":[["23c29a4e.f82bf6"]]},{"id":"87b7c751.9b976","type":"change","z":"338f5858.dee25","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.temp","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":363,"y":200,"wires":[["46ecec97.f7e234","1bca6974.5a9e1f"]]},{"id":"b955a49e.bc3438","type":"change","z":"338f5858.dee25","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.pres","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":364,"y":286,"wires":[["f855069e.76ba58"]]},{"id":"f855069e.76ba58","type":"range","z":"338f5858.dee25","minin":"0","maxin":"110000","minout":"0","maxout":"1100","action":"scale","round":true,"property":"payload","name":"","x":523,"y":286,"wires":[["e52eb6da.33bfc","b6fab314.d75778"]]},{"id":"e52eb6da.33bfc","type":"ui_gauge","z":"338f5858.dee25","name":"","group":"52cd25cb.3136fc","order":0,"width":0,"height":0,"gtype":"gage","title":"Luchtdruk","label":"hPascal","format":"{{payload}}","min":"970","max":"1050","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":718,"y":291,"wires":[]},{"id":"b6fab314.d75778","type":"ui_chart","z":"338f5858.dee25","name":"","group":"437f7191.421f08","order":0,"width":0,"height":0,"label":"Luchtdruk","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","dot":false,"ymin":"970","ymax":"1050","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":719,"y":349,"wires":[[],[]]},{"id":"52cd25cb.3136fc","type":"ui_group","z":"","name":"ec54-meters","tab":"14d08d75.13f933","disp":true,"width":"6","collapse":false},{"id":"437f7191.421f08","type":"ui_group","z":"","name":"ec54-grafieken","tab":"14d08d75.13f933","disp":true,"width":"6","collapse":false},{"id":"f4b28537.29eb48","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""},{"id":"14d08d75.13f933","type":"ui_tab","z":"","name":"ec54","icon":"dashboard"}]


Afstandsbediening van de LED
============================

Op eenzelfde manier als het dashboard kun je een gebruikersinterface maken om de LED aan- en uit te zetten.

.. figure:: nodered/Nodered-remote-led-0.png
   :width: 600 px
   :align: center

   NodeRed remote led control

Uitleg bij deze flow:

* we gebruiken de function-node om de achtergrondkleur van de knoppen aan te passen:
  rood als de led brandt, blauw als deze niet brandt.

.. code-block:: JavaScript

  if (msg.payload.led0 == 1) {
      msg.background = "red";
  } else {
      msg.background = "blue";
  }

* de on-button maakt (bij indrukken van de knop) een JSON-bericht aan: <code>{"led0": 1}</code>
* de off-button maakt een JSON-bericht aan:  <code>{"led0": 0}</code>
* de MQTT-output-node verstuurt dit bericht via de broker naar de IoT-knoop.

De flow:

.. code-block:: json

  [{"id":"3d3028df.0ce25","type":"mqtt out","z":"78f60f5d.4f998","name":"","topic":"node/ec54/actuators","qos":"","retain":"","broker":"f4b28537.29eb48","x":600,"y":300,"wires":[]},{"id":"950c2019.86d02","type":"ui_button","z":"78f60f5d.4f998","name":"On-button","group":"b677c80b.4e221","order":0,"width":0,"height":0,"passthru":false,"label":"On","color":"","bgcolor":"{{msg.background}}","icon":"","payload":"{\"led0\": 1}","payloadType":"json","topic":"node/ec54/actuators","x":350,"y":300,"wires":[["3d3028df.0ce25"]]},{"id":"e1a44b59.fca13","type":"ui_button","z":"78f60f5d.4f998","name":"Off-button","group":"b677c80b.4e221","order":0,"width":0,"height":0,"passthru":false,"label":"Off","color":"","bgcolor":"{{msg.background}}","icon":"","payload":"{\"led0\": 0}","payloadType":"json","topic":"node/ec54/actuators","x":350,"y":340,"wires":[["3d3028df.0ce25"]]},{"id":"bef6e85d.1cacb","type":"mqtt in","z":"78f60f5d.4f998","name":"","topic":"node/ec54/sensors","qos":"2","broker":"f4b28537.29eb48","x":130,"y":180,"wires":[["1ea39d7c.b75ef3"]]},{"id":"1ea39d7c.b75ef3","type":"json","z":"78f60f5d.4f998","name":"","property":"payload","action":"","pretty":false,"x":330,"y":180,"wires":[["2f59316.3ca1f4e","d95ec723.a4ce2"]]},{"id":"2f59316.3ca1f4e","type":"function","z":"78f60f5d.4f998","name":"Set background","func":"if (msg.payload.led0 == 1) {\n    msg.background = \"red\";\n} else {\n    msg.background = \"blue\";\n}\nreturn msg;","outputs":1,"noerr":0,"x":140,"y":300,"wires":[["950c2019.86d02","e1a44b59.fca13"]]},{"id":"d95ec723.a4ce2","type":"debug","z":"78f60f5d.4f998","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":570,"y":180,"wires":[]},{"id":"f4b28537.29eb48","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""},{"id":"b677c80b.4e221","type":"ui_group","z":"","name":"ec54-LED","tab":"14d08d75.13f933","disp":true,"width":"6","collapse":false},{"id":"14d08d75.13f933","type":"ui_tab","z":"","name":"ec54","icon":"dashboard"}]




MQTT koppelen aan actie/server
==============================

* ontvangen MQTT-bericht omzetten in Twitter-bericht?
