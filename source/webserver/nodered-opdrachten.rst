NodeRed-opdrachten
==================

.. admonition:: Leerdoelen en concepten

  * Kennismaking met NodeRed, voor het configureren van datastromen, koppelen van data, acties en diensten;
  * NodeRed voorbeeld: dashboard van sensordata
  * NodeRed voorbeeld: bediening van de actuatoren (LED)

De module "web voor makers" geeft een uitgebreidere handleiding in het gebruik van NodeRed.

.. admonition:: Wat heb je nodig?

  * NodeRed-installatie
      * bijvoorbeeld: FRED - https://fred.sensetecnic.com (gratis versie)

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

   Webserver-flow met 2 URLs

Deze flow bevat 2 http-input-nodes: voor elke URL een node.
Elk van deze nodes wordt gevolgd door een functie-node,
waarin de parameters voor de response op het URL-request ingevuld worden.
Deze parameters worden vervolgens gecombineerd met het HTML-template,
en als response teruggestuurd, via de HTTP-output-node.

**(1)** Kopieer de onderstaande flow naar een nieuwe NodeRed flow-tab:

.. code-block:: JSON

  [{"id":"1da46194.2d9ac6","type":"http in","z":"798cb349.253754","name":"","url":"/leds/0","method":"post","upload":false,"swaggerDoc":"","x":130,"y":200,"wires":[["7c2fe0e4.f7a1f"]]},{"id":"7c2fe0e4.f7a1f","type":"function","z":"798cb349.253754","name":"updateLed","func":"if (msg.payload.on == \"1\") {\n    flow.set(\"ledOn\", true);\n} else if (msg.payload.on == \"0\") {\n    flow.set(\"ledOn\", false);\n}\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":200,"wires":[["1b698b9d.d59cf4"]]},{"id":"a26ef255.d64668","type":"http in","z":"798cb349.253754","name":"","url":"/led-control","method":"get","upload":false,"swaggerDoc":"","x":140,"y":100,"wires":[["1b698b9d.d59cf4"]]},{"id":"a6316fe5.e385d","type":"http response","z":"798cb349.253754","name":"","statusCode":"","headers":{},"x":650,"y":100,"wires":[]},{"id":"c12705b8.6d89b8","type":"template","z":"798cb349.253754","name":"","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n  <head>\n      <title>LED server</title>\n  </head>\n  <body> <h1>LED control</h1>\n    <p>\n      <form action=\"/leds/0\" method=\"post\">\n         <button type=\"submit\" name=\"on\" value=\"1\">On</button>\n         <span style=\"font-weight:bold;color:{{color}};\"> [[LED]] </span>\n         <button type=\"submit\" name=\"on\" value=\"0\">Off</button>\n      </form>\n    </p>\n    <table>\n        <tr><td>Temperature</td>   <td>{{temperature}} &deg;C</td></tr>\n        <tr><td>Atm.pressure</td>  <td>{{barometer}} hPa</td> </tr>\n    </table>\n    <p><a href=\"/led-control\">refresh</a></p>\n  </body>\n</html>","output":"str","x":500,"y":100,"wires":[["a6316fe5.e385d"]]},{"id":"1b698b9d.d59cf4","type":"function","z":"798cb349.253754","name":"Properties","func":"if (flow.get(\"ledOn\") || false) {\n    msg.color = \"red\";\n    msg.led = 100;\n} else {\n    msg.color = \"black\";\n    msg.led = 0;\n}\n\nmsg.temperature = 23.4;\nmsg.barometer = 2014.5;\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":100,"wires":[["c12705b8.6d89b8","3a7d94d1.f202dc"]]},{"id":"3a7d94d1.f202dc","type":"ui_gauge","z":"798cb349.253754","name":"LED","group":"6ab33fd6.a18d4","order":0,"width":0,"height":0,"gtype":"gage","title":"gauge","label":"units","format":"{{led}}","min":0,"max":"100","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":510,"y":200,"wires":[]},{"id":"6ab33fd6.a18d4","type":"ui_group","z":"","name":"Simulated LED","tab":"8ac9c2af.6b6b3","disp":true,"width":"6","collapse":false},{"id":"8ac9c2af.6b6b3","type":"ui_tab","z":"","name":"Simulator","icon":"dashboard"}]

Het html-template in de template-node heeft 3 parameters: ``{{color}}``,
``{{temperature}}``, en ``{{barometer}}`` - respectievelijk de kleur van de LED-tekst,
de temperatuur, en de luchtdruk.

.. code-block:: jinja

  <html>
    <head>
        <title>LED server</title>
    </head>
    <body> <h1>LED control</h1>
      <p>
        <form action="/leds/0" method="post">
           <button type="submit" name="on" value="1">On</button>
           <span style="font-weight:bold;color:{{color}};"> [[LED]] </span>
           <button type="submit" name="on" value="0">Off</button>
        </form>
      </p>
      <table>
          <tr><td>Temperature</td>   <td>{{temperature}} &deg;C</td></tr>
          <tr><td>Atm.pressure</td>  <td>{{barometer}} hPa</td> </tr>
      </table>
      <p><a href="/led-control">refresh</a></p>
    </body>
  </html>

In de functienodes tussen de HTML-input-nodes en de template-node worden deze parameters ingevuld.
De waarden voor de temperatuur en de luchtdruk zijn fantasiewaarden:
we hebben in deze gesimuleerde knoop geen echte sensoren.

Als voorbeeld geven we de functie ``updateLed``: deze wordt uitgevoerd nadat een HTTP-POST-request met de URL ``/leds/0`` ontvangen is.

.. code-block:: javascript

  if (msg.payload.on == "1") {
      flow.set("ledOn", true);
  } else if (msg.payload.on == "0") {
      flow.set("ledOn", false);
  }
  return msg;

**(2)** Plaats een debug-node aan de output van de http-input-node ``leds/0``.
Gebruik deze om het ontvangen request te bekijken.
Stel de output van deze debug-node in als "complete msg object".
Je gebruikt deze flow als webserver met de URL: ``<<nodered>>/led-control``,
waarin ``<<nodered>>`` de URL van je NodeRed-server is.

1. wat is de method van het request?
2. wat is de URL van het request?
3. wat is de body van het request?
4. wat is de "user agent" (d.w.z., de browser)?
5. welk soort resultaat wordt verwacht ("accept"-header)?
6. wat is de payload?

Knipperende LED
---------------

Nachtlamp
---------

.. todo::

  * gebruik van inject-node om led te laten knipperen
    (nb: we hebben dan wel een webserver-knoop in het publieke internet nodig, of tenminste een gesmuleerde versie daarvan).
  * gebruik van schedule-node om led via tijd te besturen
  * dashboard voor een webserver-knoop; gebaseerd op "polling" van deze webserver-knoop; andere presentatie van gegevens.
