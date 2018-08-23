NodeRed-opdrachten
==================

.. todo::

  * Inleiding NodeRed-hoofdstuk: vooral opdrachten, geen theorie
  * focus in eerste instantie op web/http (webserver-knopen)
  * opdracht: dashboard voor webserver-knoop
      * andere presentatie van gegevens
      * automatisch opvragen van gegevens

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
--------------

.. figure:: Nodered-hello.png
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

.. |http-input-node| image:: nodered-http-input-node.png
.. |http-output-node| image:: nodered-http-output-node.png
.. |template-node| image:: nodered-template-node.png

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
--------------

Met deze eerste flow kun je zien of alles werkt:

.. figure:: Nodered-flow1.png
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

.. |inject-node| image:: inject-node.png
.. |debug-node| image:: debug-node.png

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
-----------------------

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


Een NodeRed webserver
---------------------

In deze opdracht maak je een webserver met NodeRed,
met dezelfde opzet als de webserver.

Als je NodeRed gebruikt op de Raspberry Pi,
dan kun je deze webserver eenvoudig aanpassen om een LED aan te sturen,
via een GPIO-poort.

.. figure:: IoT-webserver-flow.png
   :width: 600 px
   :align: center

   Webserver-flow met 3 URLs

Deze flow bevat 3 http-input-nodes: voor elke URL een node.
Elk van deze nodes wordt gevolgd door een functie-node,
waarin de parameters voor de response op het URL-request ingevuld worden.
Deze parameters worden vervolgens gecombineerd met het HTML-template,
en als response teruggestuurd, via de HTTP-output-node.

**(1)** Kopieer de onderstaande flow naar een nieuwe NodeRed flow-tab:

.. code-block:: JSON

  [{"id":"24edc609.6dc6da","type":"http in","z":"b888923a.1b7b88","name":"",
  "url":"/node","method":"get","upload":false,"swaggerDoc":"","x":120,"y":120,
  "wires":[["18f4426b.2abe56"]]},{"id":"e25a9256.eab99","type":"http in",
  "z":"b888923a.1b7b88","name":"","url":"/ledon","method":"get","upload":false,
  "swaggerDoc":"","x":130,"y":180,"wires":[["f2a0db0e.53519"]]},
  {"id":"88f42a11.0e20a8","type":"http in","z":"b888923a.1b7b88","name":"",
  "url":"/ledoff","method":"get","upload":false,"swaggerDoc":"","x":130,"y":240,
  "wires":[["88a7405c.8558"]]},{"id":"f8934fcf.cda0b","type":"http response",
  "z":"b888923a.1b7b88","name":"","statusCode":"","headers":{},"x":770,"y":120,
  "wires":[]},{"id":"52a74722.96bfb","type":"template","z":"b888923a.1b7b88",
  "name":"","field":"payload","fieldType":"msg","format":"handlebars",
  "syntax":"mustache","template":"<html>\n  <head> <title>ESP8266 Sensor server</title> </head>\n  <body> <h1>ESP8266 Sensor & Led control</h1>\n    <p>\n      <a href=\"/ledon\"> On </a> &gt;\n      <span style=\"font-weight:bold;color:{{color}};\"> [[LED]] </span> &lt;\n      <a href=\"/ledoff\"> Off </a>\n    </p>\n    <p>\n      Temperature: {{temp}} &deg;C <br>\n      Atm.pressure: {{press}} hPa\n    </p>\n  </body>\n</html>\n","output":"str","x":580,"y":120,"wires":[["f8934fcf.cda0b"]]},
  {"id":"18f4426b.2abe56","type":"function","z":"b888923a.1b7b88","name":"node",
  "func":"var led = flow.get(\"led\")||0;\nflow.set(\"led\", led);\nif (led == 1) {\n    msg.color = \"red\";    \n} else {\n    msg.color = \"black\";\n}\nnode.warn(\"/node \" + msg.color);\nmsg.temp = 21.3;\nmsg.press = 1019.12;\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":120,"wires":[["52a74722.96bfb"]]},{"id":"88a7405c.8558","type":"function","z":"b888923a.1b7b88","name":"led-off","func":"node.warn(\"/ledoff\");\nflow.set(\"led\", 0);\nmsg.color = \"black\";\nmsg.temp = 21.4;\nmsg.press = 1019.11;\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":240,
  "wires":[["52a74722.96bfb"]]},{"id":"f2a0db0e.53519","type":"function",
  "z":"b888923a.1b7b88","name":"led-on",
  "func":"node.warn(\"/ledon\");\nflow.set(\"led\", 1);\nmsg.color = \"red\";\nmsg.temp = 21.5;\nmsg.press = 1019.12;\nreturn msg;",
  "outputs":1,"noerr":0,"x":330,"y":180,"wires":[["52a74722.96bfb"]]}]

Het html-template in de template-node heeft 3 parameters: ``{{color}}``,
``{{temp}}``, en ``{{press}}`` - respectievelijk de kleur van de LED-tekst,
de temperatuur, en de luchtdruk.

.. code-block:: jinja

  <html>
    <head> <title>ESP8266 Sensor server</title> </head>
    <body> <h1>ESP8266 Sensor & Led control</h1>
      <p>
        <a href="/ledon"> On </a> &gt;
        <span style="font-weight:bold;color:{{color}};"> [[LED]] </span> &lt;
        <a href="/ledoff"> Off </a>
      </p>
      <p>
        Temperature: {{temp}} &deg;C <br>
        Atm.pressure: {{press}} hPa
      </p>
    </body>
  </html>

In de functienodes tussen de HTML-input-nodes en de template-node worden deze parameters ingevuld.
De waarden voor de temperatuur en de luchtdruk zijn fantasiewaarden:
we hebben in deze gesimuleerde knoop geen echte sensoren.

Als voorbeeld geven we de functie led-on: deze wordt uitgevoerd nadat een HTTP-request met de URL ``/ledon`` ontvangen is.

.. code-block:: javascript

  node.warn("/ledon");
  flow.set("led", 1);
  msg.color = "red";
  msg.temp = 21.5;
  msg.press = 1019.12;
  return msg;

**(2)** Plaats een debug-node aan de output van de http-input-node ``ledon``.
Gebruik deze om het ontvangen request te bekijken.
Stel de output van deze debug-node in als "complete msg object".

1. wat is de method van het request?
2. wat is de URL van het request?
3. wat is de "user agent" (d.w.z., de browser)?
4. welk soort resultaat wordt verwacht ("accept"-header)?

Knipperende LED
---------------

Nachtlamp
---------

.. todo::

  * gebruik van inject-node om led te laten knipperen
    (nb: we hebben dan wel een webserver-knoop in het publieke internet nodig, of tenminste een gesmuleerde versie daarvan).
  * gebruik van schedule-node om led via tijd te besturen
  * dashboard voor een webserver-knoop; gebaseerd op "polling" van deze webserver-knoop.
