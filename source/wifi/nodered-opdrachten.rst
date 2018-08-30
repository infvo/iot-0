NodeRed-opdrachten
==================

Bij de volgende opdrachten gebruiken we NodeRed-flows met MQTT-knopen.
Met deze MQTT-knopen sluiten we deze flows aan op de IoT-knopen.

.. admonition:: Je hebt nodig

  * (gratis) FRED-account voor NodeRed (in het publieke internet)
  * MQTT-broker in het publieke netwerk, bijvoorbeeld: ``infvopedia.nl:1884``
  * mqttt web-app: http://infvopedia.nl:1884/mqttt.html
  * zo mogelijk: IoT-knoop (ESP8266) met toepassing: ``mqtt-node-0``,
  * alternatief: IoT-knoop-simulator: http://infvopedia.nl:1884/iotnode-app.html

Je hebt de toegangsgegevens van deze broker nodig voor het de onderstaande opdrachten.
De IoT-knoop moet geconfigureerd zijn voor WiFi-toegang tot een lokaal netwerk,
en voor MQTT-toegang tot de genoemde publieke MQTT-broker.
Je kunt werken met een voorgeconfigureerde IoT-knoop.
Voor het configureren van de IoT-knoop, zie IoT-1.

.. rubric:: MQTT-nodes

+--------------------+------------------+------------------+
| **figuur**         | **naam**         | **soort node**   |
+--------------------+------------------+------------------+
| |mqtt-input-node|  | mqtt-input-node  |  input           |
+--------------------+------------------+------------------+
| |mqtt-output-node| | mqtt-output-node |  output          |
+--------------------+------------------+------------------+
| |mqtt-broker-node| | mqtt-broker-node |  configuration   |
+--------------------+------------------+------------------+

.. |mqtt-input-node| image:: nodered-mqtt-input-node.png
.. |mqtt-output-node| image:: nodered-mqtt-output-node.png
.. |mqtt-broker-node| image:: nodered-mqtt-broker-node.png

In de volgende voorbeelden gebruiken we de MQTT-input- en output-nodes.
Deze nodes configureer je met de gebruikte mqtt-broker en het topic.

* de MQTT-input-node heeft het ontvangen bericht als resultaat (payload);
* de MQTT-output-node stuurt het bericht (``msg.payload``) naar de broker, met het genoemde topic.
* er is een aparte *configuratie-node* voor de MQTT-broker.
  Deze gebruik je indirect bij het configureren van de MQTT-input- of output-node.
  Je kunt deze broker-node ook vinden via het rechter "hamburger" menu: Configuration Nodes.

(5) MQTT sensor flow
--------------------

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

.. figure:: mqtt-sensor-flow-0.png
   :width: 600 px
   :align: center

   MQTT sensor flow


Flow:

.. code-block:: json

  [{"id":"db8775b0.d5ebf8","type":"mqtt in","z":"ffc7967f.8cd98","name":"","topic":"node/ec54/sensors","qos":"2","broker":"f4b28537.29eb48","x":190,"y":120,"wires":[["553ee431.775ac4","d065b3dd.226998"]]},{"id":"553ee431.775ac4","type":"debug","z":"ffc7967f.8cd98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":430,"y":120,"wires":[]},{"id":"8675f8e8.eb7ff8","type":"debug","z":"ffc7967f.8cd98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":200,"wires":[]},{"id":"d065b3dd.226998","type":"json","z":"ffc7967f.8cd98","name":"","property":"payload","action":"","pretty":false,"x":410,"y":200,"wires":[["8675f8e8.eb7ff8"]]},{"id":"f4b28537.29eb48","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]


(6) MQTT actuator flow
----------------------

In dit voorbeeld gebruik je de MQTT-output-node.
Hiermee stuur je JSON berichten naar een IoT-knoop.

* configureer de MQTT-output-node voor je IoT-knoop (nodeid) en voor je MQTT-broker.
* bij de Inject-node selecteer je voor de payload het JSON-alternatief: ``{}``.
* voor het in- en uitschakelen gebruik je de teksten ``{"0":{"dOut":1}}`` en ``{"0":{"dOut":0}}``
* controleer bij de IoT-knoop of de LED aan- en uitgaat;
* controleer eventueel de MQTT-berichten met de MQTTT-app.

.. figure:: mqtt-actuator-flow-0.png
   :width: 500 px
   :align: center

   MQTT actuator flow

.. code-block:: json

  [{"id":"d5114a87.c3aa2","type":"inject","z":"fd9cc71d.7f5e1","name":"Led-on","topic":"","payload":"{\"0\":{\"dOut\":1}}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":210,"y":180,"wires":[["e0dcf3ba.5bdc68"]]},{"id":"e0dcf3ba.5bdc68","type":"mqtt out","z":"fd9cc71d.7f5e1","name":"","topic":"node/ec54/actuators","qos":"","retain":"","broker":"","x":500,"y":180,"wires":[]},{"id":"916570d.f38be9","type":"inject","z":"fd9cc71d.7f5e1","name":"Led-off","topic":"","payload":"{\"0\":{\"dOut\":0}}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":210,"y":240,"wires":[["e0dcf3ba.5bdc68"]]}]

(7) Een eigen dashboard
-----------------------

In deze opdracht maak je een eigen dashboard voor de IoT-knoop,
met behulp van NodeRed.

* Als je een NodeRed-server in het publieke internet gebruikt,
  dan is dit dashboard toegankelijke voor computers in het internet (lokaal en publiek).
* Als je een NodeRed-server in het lokale netwerk gebruikt,
  bijvoorbeeld op een Raspberry Pi,
  dan is het dashboard alleen toegankelijk voor computers in het lokale netwerk.

Gebruik voor het dashboard de volgende flow:

.. figure:: IoT-dashboard-flow.png
   :width: 600 px
   :align: center

   NodeRed-flow voor IoT-dashboard

Kopieer de onderstaande flow-code naar een lege flow-tab in NodeRed.

.. code-block:: JSON

  [{"id":"5e3f6502.43471c","type":"mqtt in","z":"c9819513.b5c408", "name":"",
  "topic":"node/e0f1/sensors", "qos":"2","broker":"d97b4423.5d2e78","x":170,"y":120,
  "wires":[["9a7d4d72.52a178"]]},{"id":"9a7d4d72.52a178","type":"json",
  "z":"c9819513.b5c408","name":"","pretty":false,"x":210,"y":220,
  "wires":[["4d0ca7b0.c72278","f844ca03.6d7bf8","c4605ff8.e68618"]]},
  {"id":"bd11d240.d3e168","type":"ui_gauge","z":"c9819513.b5c408","name":"",
  "group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage",
  "title":"Temperatuur","label":"'C","format":"{{payload}}","min":0,"max":"50",
  "colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":610,"y":200,
  "wires":[]},{"id":"eb10bb85.4c73b8","type":"ui_chart","z":"c9819513.b5c408",
  "name":"","group":"b7537500.9e9de","order":0,"width":0,"height":0,"label":"Temperatuur",
  "chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear",
  "nodata":"","dot":false,"ymin":"0","ymax":"50","removeOlder":1,"removeOlderPoints":"",
  "removeOlderUnit":"3600","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8",
  "#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],
  "useOldStyle":false,"x":610,"y":240,"wires":[[],[]]},{"id":"4d0ca7b0.c72278",
  "type":"change","z":"c9819513.b5c408","name":"","rules":[{"t":"set","p":"payload",
  "pt":"msg","to":"payload.temp","tot":"msg"}],"action":"","property":"","from":"",
  "to":"","reg":false,"x":400,"y":220,"wires":[["eb10bb85.4c73b8","bd11d240.d3e168"]]},
  {"id":"2881d11b.ee3086","type":"ui_gauge","z":"c9819513.b5c408","name":"",
  "group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage",
  "title":"Luchtdruk","label":"units","format":"{{payload}}","min":"950","max":"1050",
  "colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":600,"y":300,
  "wires":[]},{"id":"96937342.1bee2","type":"ui_chart","z":"c9819513.b5c408",
  "name":"","group":"b7537500.9e9de","order":0,"width":0,"height":0,"label":"Luchtdruk",
  "chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear",
  "nodata":"","dot":false,"ymin":"950","ymax":"1050","removeOlder":1,"removeOlderPoints":"",
  "removeOlderUnit":"3600","cutout":0,"useOneColor":false,"colors":["#1f77b4",
  "#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],
  "useOldStyle":false,"x":600,"y":340,"wires":[[],[]]},{"id":"f844ca03.6d7bf8",
  "type":"change","z":"c9819513.b5c408","name":"","rules":[{"t":"set","p":"payload",
  "pt":"msg","to":"payload.pres","tot":"msg"}],"action":"","property":"","from":"",
  "to":"","reg":false,"x":400,"y":320,"wires":[["2881d11b.ee3086","96937342.1bee2"]]},
  {"id":"c4605ff8.e68618","type":"debug","z":"c9819513.b5c408","name":"","active":true,
  "console":"false","complete":"false","x":390,"y":160,"wires":[]},{"id":"d97b4423.5d2e78",
  "type":"mqtt-broker","z":"","name":"","broker":"infvopedia.nl","port":"1883","clientid":"",
  "usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"birthTopic":"",
  "birthQos":"0","birthRetain":"false","birthPayload":"","closeTopic":"","closeQos":"0",
  "closeRetain":"false","closePayload":"","willTopic":"","willQos":"0","willRetain":"false",
  "willPayload":""},{"id":"a4643fc8.e80d68","type":"ui_group","z":"","name":"my-meters",
  "tab":"4e75c8d2.40f86","disp":true,"width":"6","collapse":false},{"id":"b7537500.9e9de",
  "type":"ui_group","z":"","name":"My-graphs","tab":"4e75c8d2.40f86","disp":true,
  "width":"6","collapse":false},{"id":"4e75c8d2.40f86","type":"ui_tab","z":"",
  "name":"My-node","icon":"dashboard"}]

Pas in deze flow de parameters van de MQTT-input-node aan, en bekijk je eigen dashboard.

* dubbel-klik op de MQTT-input-node;
* verander de node-ID in het topic naar de ID van je eigen knoop.
* je krijgt de webpagina met het dashboard via de tab "dasboard", bij het debug-venster rechts.
  In deze tab klik je op het vierkantje met de uitgaande pijl (rechtsboven).
* in het dashboard vind je de gegevens van je eigen knoop onder "My Node".

Breid het dashboard uit met een weergaven van de licht-sensor.

* kopieer de deelflow met 3 knopen: set msg.payload, Luchtdruk(meter) en Luchtdruk (grafiek),
* en plak deze in dezelfde flow;
* pas de knoop msg.payload aan: ``set msg.payload to msg.payload.light``
* pas de knopen Luchtdruk (meter) en Luchtdruk (grafiek) aan: vervang "Luchtdruk" door "Licht",
  en stel de minima en maxima in op 0 en 1023.
* **deploy**
* controleer het dashboard; het kan even duren voordat de IoT-knoop de sensorwaarden verstuurd heeft.


(8) Koppelen van knopen
-----------------------

Je kunt in NodeRed ook verschillende IoT-knopen aan elkaar koppelen.
We gebruiken dit om met de knoppen van de ene IoT-knoop een LED van een andere IoT-knoop aan- en uit te schakelen.

We gebruiken de ene knop om een LED aan te zetten, en de andere knop om deze uit te zetten.
Dit zorgt ervoor dat er geen vreemde dingen gebeuren als er een bericht verloren gaat.

.. topic:: Idempotente acties

  Bij een idempotente actie maakt het geen verschil of je deze 1 maal of vaker uitvoert.
  Deze aanpak gebruik je veel vaker bij communicatie, vooral als deze "best effort" is.
  Als je niet zeker bent of een bericht aangekomen is, kun je dit zonder risico nogmaals versturen.
  Een voorbeeld is de HTTP-GET opdracht: je kunt een webpagina een extra keer vernieuwen (reload) zonder dat dit gevolgen heeft (voor de server).
  De HTTP-POST opdracht is niet idempotent: de browser vraagt je dan of je het formulier nogmaals wilt versturen.

  In ons geval configureren we de knoppen op de IoT-knopen op een idempotente manier:
  we gebruiken de ene knop voor het aanzetten en de andere voor het uitschakelen van de LED.
  (Ga na wat er kan gebeuren als je één knop gebruikt voor het aan- en uitschakelen,
  in een situatie dat er berichten verloren kunnen gaan.)

Bij deze opdracht heb je twee IoT-knopen nodig: met de buttons van de ene knoop bedien je een LED van de andere knoop.
Je kunt hiervoor ook gesimuleerde knopen gebruiken.

(1) Importeer de flow, pas deze aan, en test deze:

* maak een IoT-knoop(simulator) aan, en geef deze een eigen (uniek) adres (nodeA);
* maak een tweede IoT-knoop aan, en geef deze een eigen (uniek) adres (nodeB);
* maak een nieuw flow-venster aan in NodeRed (via de "+"-tab);
* importeer hierin de flow-code die hieronder staat;
* pas de volgende knopen aan:
    * ``node/e0f1/sensors`` (mqtt input-node)
        * verander het Topic in ``node/[nodeA]/sensors``, waarin ``[nodeA]`` de ID is van nodeA
        * "Save"
    * ``node/e0f2/actuators`` (mqtt output node)
        * verander het Topic in ``node/[nodeB]/sensors``, waarin ``[nodeB]`` de ID is van nodeB
        * "Save"
* "Deploy" de aangepaste flow
* Test de flow:
    * Button0 van NodeA schakelt Led0 van NodeB aan
    * Button1 van NodeA schakelt Led0 van NodeB uit

(2) Deze flow heeft als nadeel dat je bij NodeA niet ziet of Led0 bij NodeB brandt.

* pas de flow aan zodat Button0 ook Led0 van NodeA aanzet, en Led0 van NodeA uitzet
    * hint: je hoeft maar 1 output-node toe te voegen.

(3) Wat lastiger is de volgende variant:

* pas de flow aan zodat Button0 ''Led1''(!) van NodeA aanzet, en Led1 van NodeA uitzet (in plaats van Led0).
    * (en nog steeds Led0 van NodeB schakelt)
    * tip: binnen NodeRed kun je nodes of hele flows kopiëren en plakken met Copy/Paste.

(4) De volgende stap ligt nu voor de hand:

    * pas de flow aan zodat Button0 van NodeB Led0 van NodeA en Led1 van NodeB aan zet; Button1 van NodeB zet deze leds uit.

Flow voor de koppeling van schakelaars en LEDs
----------------------------------------------

.. [[Bestand:IoT-node-switch-flow.png|IoT node - switch flow]]

Uitleg bij deze flow:

* de mqtt-input-node ontvangt (via "subscribe") de berichten van het Topic ``node/[IDa]/sensors``
* de JSON-node zet de JSON-tekst van het mqtt-bericht om in een JavaScript-object
* de switch-node splitst de berichten in:
    *  berichten met ``button0: 1`` en
    * berichten met ``button1: 1``;
* deze verschijnen op de twee uitgangen, en op de ingangen van de template-nodes:
    * de eerste template-node geeft als resultaat {"0": {"dOut":1} (in JSON)
    * de tweede template-node geeft: {"0": {"dOut":0} (idem)
* de mqtt-output-node verstuurt ("publish") het JSON-bericht onder Topic ``node/[IDb]/actuators``
* controleer de berichten met het mqtt-hulpprogramma
* controleer de berichten door debug-nodes aan de flow toe te voegen (vergeet "Deploy" niet!).

NodeRed-code van deze flow:

.. code-block:: JSON

  [{"id":"7bd76101.769c6","type":"mqtt in","z":"21e77168.c3a01e","name":"","topic":"node/e0f1/sensors","qos":"2","broker":"d97b4423.5d2e78","x":210,"y":120,"wires":[["61316186.c89fa8"]]},{"id":"61316186.c89fa8","type":"json","z":"21e77168.c3a01e","name":"","property":"payload","action":"","pretty":false,"x":400,"y":120,"wires":[["65a7b0ed.8e5d"]]},{"id":"65a7b0ed.8e5d","type":"switch","z":"21e77168.c3a01e","name":"button0/1 split","property":"payload","propertyType":"msg","rules":[{"t":"jsonata_exp","v":"payload.button0 = 1","vt":"jsonata"},{"t":"jsonata_exp","v":"payload.button1 = 1","vt":"jsonata"}],"checkall":"true","repair":false,"outputs":2,"x":330,"y":240,"wires":[["441083fd.53bc6c"],["54723040.784cf"]]},{"id":"441083fd.53bc6c","type":"template","z":"21e77168.c3a01e","name":"led0-1","field":"payload","fieldType":"msg","format":"json","syntax":"mustache","template":"{\"led0\": 1}","output":"str","x":540,"y":220,"wires":[["e12e623.940f0a"]]},{"id":"54723040.784cf","type":"template","z":"21e77168.c3a01e","name":"led0-0","field":"payload","fieldType":"msg","format":"json","syntax":"mustache","template":"{\"led0\": 0}","output":"str","x":540,"y":260,"wires":[["e12e623.940f0a"]]},{"id":"e12e623.940f0a","type":"mqtt out","z":"21e77168.c3a01e","name":"","topic":"node/e0f2/actuators","qos":"","retain":"","broker":"d97b4423.5d2e78","x":790,"y":240,"wires":[]},{"id":"d97b4423.5d2e78","type":"mqtt-broker","z":"","name":"","broker":"infvopedia.nl","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","willTopic":"","willQos":"0","willRetain":"false","willPayload":""}]
