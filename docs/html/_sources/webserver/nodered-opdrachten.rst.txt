NodeRed-opdrachten
==================

(7) Een NodeRed webserver
--------------------------

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
