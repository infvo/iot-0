NodeRed-opdrachten
==================

.. bij WiFi/MQTT-keten

Bij de volgende opdrachten gebruiken we NodeRed-flows met MQTT-nodes.
Met de MQTT-input- en output-nodes sluiten we deze flows aan op de IoT-knopen.

.. admonition:: Je hebt nodig

  * (gratis) FRED-account voor NodeRed (in het publieke internet)
  * MQTT-broker in het publieke netwerk, bijvoorbeeld: ``infvopedia.nl:1883``;
    Vraag de toegangsgegevens hiervoor aan je docent;
  * mqt3 web-app: http://infvopedia.nl/mqt3.html;
  * zo mogelijk: IoT-knoop (ESP8266) met toepassing: ``mqtt-node-x``;
  * alternatief: IoT-knoop-simulator: http://infvopedia.nl/iotnode-app.html

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

* de MQTT-input-node heeft als resultaat (``msg.payload``) het ontvangen bericht.
* de MQTT-output-node stuurt het bericht (``msg.payload``) naar de broker, met het genoemde topic.
* er is een aparte *configuratie-node* voor de MQTT-broker.
  Deze gebruik je indirect bij het configureren van de MQTT-input- of output-node.
  Je kunt deze broker-node ook vinden via het rechter "hamburger" menu: Configuration Nodes.

.. admonition:: Let op

  Bij het configureren van een mqtt-node moet je ook de gebruikersnaam en het wachtwoord invullen.
  Deze gegevens worden bij het kopiëren en plakken van een flow *niet meegekopieerd*
  (in verband met de veiligheid).

(1) MQTT sensor-flow
--------------------

In deze sensor-flow gebruik je een MQTT-input-node om de sensorwaarden van een IoT-knoop te ontvangen.
De ontvangen waarden vind je in het debug-venster.
Om deze flow te gebruiken moet je eerst de verschillende nodes configureren (dubbelklik op de node):

* de  *nodeid* in het topic van de MQTT-input-node verander je in de *nodeid* van je eigen IoT-knoop;
  voorbeeld: ``node/e0f1/sensors``
* de MQTT-broker van de MQTT-input-node configureer je met de gegevens van je MQTT-broker;
  vergeet niet de gebruikersnaam en het wachtwoord;
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


(2) MQTT actuator-flow
----------------------

In de actuator-flow gebruik je de MQTT-output-node.
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

(3) Sensor-dashboard
--------------------

Je maakt met NodeRed een sensor-dashboard voor de IoT-knoop.

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

  [{"id":"97c52af7.83a448","type":"mqtt in","z":"d22f8dde.af3ae8","name":"","topic":"node/8f12/sensors","qos":"2","broker":"d7c8ba29.2d4508","x":170,"y":120,"wires":[["2a7496ff.f65fb2"]]},{"id":"2a7496ff.f65fb2","type":"json","z":"d22f8dde.af3ae8","name":"","pretty":false,"x":150,"y":200,"wires":[["1e9d97ab.35c888"]]},{"id":"14f7b979.cab697","type":"ui_gauge","z":"d22f8dde.af3ae8","name":"","group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage","title":"Temperatuur","label":"'C","format":"{{payload}}","min":0,"max":"50","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":630,"y":180,"wires":[]},{"id":"6c6dc3e3.2ae914","type":"ui_chart","z":"d22f8dde.af3ae8","name":"","group":"b7537500.9e9de","order":0,"width":0,"height":0,"label":"Temperatuur","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"0","ymax":"50","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":630,"y":220,"wires":[[],[]]},{"id":"98955d0f.426088","type":"change","z":"d22f8dde.af3ae8","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.temperature","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":200,"wires":[["6c6dc3e3.2ae914","14f7b979.cab697"]]},{"id":"2837dd7b.950f42","type":"ui_gauge","z":"d22f8dde.af3ae8","name":"","group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage","title":"Luchtdruk","label":"units","format":"{{payload}}","min":"950","max":"1050","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":620,"y":280,"wires":[]},{"id":"19137cd2.ee77ab","type":"ui_chart","z":"d22f8dde.af3ae8","name":"","group":"b7537500.9e9de","order":0,"width":0,"height":0,"label":"Luchtdruk","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"950","ymax":"1050","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":620,"y":320,"wires":[[],[]]},{"id":"a21da591.9e0878","type":"change","z":"d22f8dde.af3ae8","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.barometer","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":300,"wires":[["2837dd7b.950f42","19137cd2.ee77ab"]]},{"id":"1e9d97ab.35c888","type":"function","z":"d22f8dde.af3ae8","name":"convert-LLP-sensors","func":"msg.payload.temperature = msg.payload.payload[4].temperature / 10;\nmsg.payload.barometer = msg.payload.payload[5].barometer / 10;\nmsg.payload.humidity = msg.payload.payload[6].humidity;\nmsg.payload.lightlevel = msg.payload.payload[8].aIn;\nmsg.payload.led0 = msg.payload.payload[0].dOut;\nmsg.payload.led1 = msg.payload.payload[1].dOut;\n//msg.payload.button0 = msg.payload.payload[2].dIn;\n//msg.payload.button1 = msg.payload.payload[3].dIn;\nreturn msg;","outputs":1,"noerr":0,"x":190,"y":300,"wires":[["a21da591.9e0878","98955d0f.426088","c8bda563.74bfa"]]},{"id":"52abcdc9.226a74","type":"ui_gauge","z":"d22f8dde.af3ae8","name":"","group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage","title":"Licht","label":"units","format":"{{payload}}","min":"0","max":"1023","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":610,"y":380,"wires":[]},{"id":"c3fd28d5.22ee","type":"ui_chart","z":"d22f8dde.af3ae8","name":"","group":"b7537500.9e9de","order":0,"width":0,"height":0,"label":"Licht","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"0","ymax":"1023","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":610,"y":420,"wires":[[],[]]},{"id":"c8bda563.74bfa","type":"change","z":"d22f8dde.af3ae8","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.lightlevel","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":400,"wires":[["52abcdc9.226a74","c3fd28d5.22ee"]]},{"id":"d7c8ba29.2d4508","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","willTopic":"","willQos":"0","willPayload":""},{"id":"a4643fc8.e80d68","type":"ui_group","z":"","name":"My-meters","tab":"4e75c8d2.40f86","disp":true,"width":"6","collapse":false},{"id":"b7537500.9e9de","type":"ui_group","z":"","name":"My-graphs","tab":"4e75c8d2.40f86","disp":true,"width":"6","collapse":false},{"id":"4e75c8d2.40f86","type":"ui_tab","z":"","name":"My-node 8f12","icon":"dashboard"}]

(a) Pas in deze flow de parameters van de MQTT-input-node aan, en bekijk het dashboard.

* configureer de MQTT-input-node voor je IoT-knoop (nodeid) en voor je MQTT-broker;
* ga naar de webpagina van het dashboard via de tab "dasboard", bij het debug-venster rechts.
  In deze tab klik je op het vierkantje met de uitgaande pijl (rechtsboven).
* in het dashboard vind je de gegevens van je eigen knoop onder "My Node".

(b) Breid het dashboard uit met een weergave van de lichtsensor.

* kopieer de deelflow met 3 knopen: *set msg.payload*, Luchtdruk(meter) en Luchtdruk (grafiek),
* en plak deze in dezelfde flow;
* pas de knoop *msg.payload* aan: ``set msg.payload to msg.payload.lightlevel``
* pas de knopen Luchtdruk (meter) en Luchtdruk (grafiek) aan: vervang "Luchtdruk" door "Licht",
  en stel de minima en maxima in op 0 en 1023.
* **deploy**
* controleer het dashboard; het kan even duren voordat de IoT-knoop de sensorwaarden verstuurd heeft.
  (Je kunt dit versnellen door een bericht naar de IoT-knoop te sturen, bijvoorbeeld om de LED aan te zetten.)

(4) LED-afstandsbediening
-------------------------

Met de NodeRed UI-nodes maak je een web-interface om de LED van een IoT-knoop aan- en uit te zetten.

.. figure:: Nodered-remote-led-0.png
   :width: 600 px
   :align: center

   NodeRed remote led control

Uitleg bij deze flow:

* we gebruiken de function-node om de achtergrondkleur van de knoppen aan te passen:
  rood als de led brandt, blauw als deze niet brandt.

.. code-block:: JavaScript

  if (msg.payload.payload[0].dOut == 1) {
      msg.background = "red";
  } else {
      msg.background = "blue";
  }
  return msg;

* ``msg.payload`` is hier het MQTT-JSON-bericht;
  dit is een object met als ``payload`` de sensorgegevens in LPP-formaat;
* ``payload[0]`` geeft de toestand van LED0 weer;
* de on-button maakt (bij indrukken van de knop) een JSON-bericht aan: ``{"0":{"dOut":1}}``
* de off-button maakt een JSON-bericht aan:  ``{"0":{"dOut":0}}``
* de MQTT-output-node verstuurt dit bericht via de broker naar de IoT-knoop.

De flow:

.. code-block:: json

  [{"id":"c86ee84d.ba74e8","type":"mqtt out","z":"c5998a3b.3a8cd8","name":"","topic":"node/ec54/actuators","qos":"","retain":"","broker":"a71c1cf9.dcd1c","x":680,"y":240,"wires":[]},{"id":"d6461459.c6e21","type":"ui_button","z":"c5998a3b.3a8cd8","name":"On-button","group":"ac1fc815.5c1368","order":0,"width":0,"height":0,"passthru":false,"label":"On","color":"","bgcolor":"{{msg.background}}","icon":"","payload":"{\"0\":{\"dOut\":1}}","payloadType":"json","topic":"node/ec54/actuators","x":430,"y":240,"wires":[["c86ee84d.ba74e8"]]},{"id":"29557457.4d4734","type":"ui_button","z":"c5998a3b.3a8cd8","name":"Off-button","group":"ac1fc815.5c1368","order":0,"width":0,"height":0,"passthru":false,"label":"Off","color":"","bgcolor":"{{msg.background}}","icon":"","payload":"{\"0\":{\"dOut\":0}}","payloadType":"json","topic":"node/ec54/actuators","x":430,"y":280,"wires":[["c86ee84d.ba74e8"]]},{"id":"6e55f1d8.49d9d","type":"mqtt in","z":"c5998a3b.3a8cd8","name":"","topic":"node/ec54/sensors","qos":"2","broker":"a71c1cf9.dcd1c","x":210,"y":120,"wires":[["5971a318.cee434"]]},{"id":"5971a318.cee434","type":"json","z":"c5998a3b.3a8cd8","name":"","property":"payload","action":"","pretty":false,"x":410,"y":120,"wires":[["cb9d5ef5.0ac258","ef6cae81.13708"]]},{"id":"cb9d5ef5.0ac258","type":"function","z":"c5998a3b.3a8cd8","name":"Set background","func":"if (msg.payload.payload[0].dOut == 1) {\n    msg.background = \"red\";\n} else {\n    msg.background = \"blue\";\n}\nreturn msg;","outputs":1,"noerr":0,"x":220,"y":240,"wires":[["d6461459.c6e21","29557457.4d4734"]]},{"id":"ef6cae81.13708","type":"debug","z":"c5998a3b.3a8cd8","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":650,"y":120,"wires":[]},{"id":"a71c1cf9.dcd1c","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","willTopic":"","willQos":"0","willPayload":""},{"id":"ac1fc815.5c1368","type":"ui_group","z":"","name":"ec54-LED","tab":"82f77921.e05dc","disp":true,"width":"6","collapse":false},{"id":"82f77921.e05dc","type":"ui_tab","z":"","name":"ec54","icon":"dashboard"}]


(5) Koppelen van knopen
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

Bij deze opdracht heb je twee IoT-knopen nodig, nodeA en nodeB.
Met de buttons van knoop nodeA bedien je een LED van knoop nodeB.
Je kunt hiervoor ook gesimuleerde knopen gebruiken.

(a) Importeer de flow, pas deze aan, en test deze:

  * importeer in een nieuw flow-venster de flow-code die hieronder staat;
  * configureer de MQTT-input-node met de gegevens van nodeA en van je broker;
  * configureer de MQTT-output-node met de gegevens van nodeB en van je broker;
  * "Deploy" de aangepaste flow
  * Test de flow:
      * Button0 van NodeA schakelt Led0 van NodeB aan
      * Button1 van NodeA schakelt Led0 van NodeB uit

(b) Deze flow heeft als nadeel dat je bij nodeA niet ziet of de led bij nodeB brandt.

  * pas de flow aan zodat Button0 ook Led0 van nodeA aanzet, en Button1 deze led uitzet.
      * hint: je hoeft maar 1 output-node toe te voegen.

(c) Wat lastiger is de volgende variant:

  * pas de flow aan zodat Button0 *Led1* van nodeA aanzet, en Button1 deze led uitzet.
      * (deze buttons schakelen nog steeds Led0 van nodeB)
      * tip: binnen NodeRed kun je nodes of hele flows kopiëren en plakken met Copy/Paste.

(d) De volgende stap ligt nu voor de hand:

  * pas de flow aan zodat Button0 van nodeB Led0 van nodeA en Led1 van nodeB aan zet;
    Button1 van nodeB zet deze leds uit.

.. rubric:: Flow voor de koppeling van schakelaars en LEDs

.. [[Bestand:IoT-node-switch-flow.png|IoT node - switch flow]]

.. figure:: IoT-node-switch-flow.png
   :width: 600 px
   :align: center

   IoT node - switch flow

Uitleg bij deze flow:

* de mqtt-input-node ontvangt (via "subscribe") de berichten van het Topic ``node/[IDa]/sensors``
* de JSON-node zet de JSON-tekst van het mqtt-bericht om in een JavaScript-object (met LPP-payload);
* de node *convertLPPsensors* zet de LPP-payload om naar ``msg.payload``.
* de switch-node splitst de berichten in:
    * berichten met ``button0 = 1`` en
    * berichten met ``button1 = 1``;
    * NB: dit zijn JSONata-expressies: ``=`` staat voor de test op gelijkheid.
* deze verschijnen op de twee uitgangen, en op de ingangen van de template-nodes:
    * de eerste template-node geeft als resultaat {"0": {"dOut":1} (in JSON)
    * de tweede template-node geeft: {"0": {"dOut":0} (idem)
* de mqtt-output-node verstuurt ("publish") het JSON-bericht onder Topic ``node/[IDb]/actuators``
* controleer de berichten met het mqtt-hulpprogramma
* controleer de berichten door debug-nodes aan de flow toe te voegen (vergeet "Deploy" niet!).

NodeRed-code van deze flow:

.. code-block:: JSON

  [{"id":"2137b92e.cd0ef6","type":"mqtt in","z":"ba282e2d.b99a98","name":"","topic":"node/e0f1/sensors","qos":"2","broker":"d7c8ba29.2d4508","x":190,"y":140,"wires":[["e506dde5.324f9"]]},{"id":"e506dde5.324f9","type":"json","z":"ba282e2d.b99a98","name":"","property":"payload","action":"","pretty":false,"x":380,"y":140,"wires":[["e15ea67c.34b19","76e0b51.23c68cc"]]},{"id":"da4f8e21.b2655","type":"switch","z":"ba282e2d.b99a98","name":"button0/1 split","property":"payload","propertyType":"msg","rules":[{"t":"jsonata_exp","v":"payload.button0 = 1","vt":"jsonata"},{"t":"jsonata_exp","v":"payload.button1 = 1","vt":"jsonata"}],"checkall":"true","repair":false,"outputs":2,"x":310,"y":260,"wires":[["7c0ee5f.25ca49c"],["17069d90.2e51fa"]]},{"id":"7c0ee5f.25ca49c","type":"template","z":"ba282e2d.b99a98","name":"led0-1","field":"payload","fieldType":"msg","format":"json","syntax":"mustache","template":"{\"0\":{\"dOut\":1}}","output":"str","x":510,"y":240,"wires":[["bffac5ea.3b6d98","279e4ee7.53c692"]]},{"id":"17069d90.2e51fa","type":"template","z":"ba282e2d.b99a98","name":"led0-0","field":"payload","fieldType":"msg","format":"json","syntax":"mustache","template":"{\"0\":{\"dOut\":0}}","output":"str","x":510,"y":280,"wires":[["bffac5ea.3b6d98","279e4ee7.53c692"]]},{"id":"bffac5ea.3b6d98","type":"mqtt out","z":"ba282e2d.b99a98","name":"","topic":"node/ec54/actuators","qos":"","retain":"","broker":"d7c8ba29.2d4508","x":770,"y":260,"wires":[]},{"id":"e15ea67c.34b19","type":"function","z":"ba282e2d.b99a98","name":"convert-LLP-sensors","func":"if (msg.payload.payload.hasOwnProperty(\"2\")) {\n  msg.payload.button0 = msg.payload.payload[2].dIn;\n} else {\n    button0 = 0;\n}\nif (msg.payload.payload.hasOwnProperty(\"3\")) {\n  msg.payload.button1 = msg.payload.payload[3].dIn;\n} else {\n    button1 = 0;\n}\n\nreturn msg;","outputs":1,"noerr":0,"x":600,"y":140,"wires":[["da4f8e21.b2655"]]},{"id":"279e4ee7.53c692","type":"debug","z":"ba282e2d.b99a98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":720,"y":340,"wires":[]},{"id":"76e0b51.23c68cc","type":"debug","z":"ba282e2d.b99a98","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":520,"y":80,"wires":[]},{"id":"d7c8ba29.2d4508","type":"mqtt-broker","z":"","name":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","closeTopic":"","closePayload":"","willTopic":"","willQos":"0","willPayload":""}]

De code van *convert-LPP-sensors*:

.. code-block:: JavaScript

  if (msg.payload.payload.hasOwnProperty("2")) {
      msg.payload.button0 = msg.payload.payload[2].dIn;
  } else {
      button0 = 0;
  }
  if (msg.payload.payload.hasOwnProperty("3")) {
      msg.payload.button1 = msg.payload.payload[3].dIn;
  } else {
      button1 = 0;
  }

  return msg;
