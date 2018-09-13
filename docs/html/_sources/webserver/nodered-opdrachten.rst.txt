NodeRed-opdrachten
==================

.. bij webserver-keten

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

P.M.

Nachtlamp
---------

P.M.

Sensor-dashboard
----------------

Met een sensor-dashboard kun je de waarden van de sensoren via een browser bekijken.

.. figure:: Nodered-dashboard-display-0.png
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

.. |dashboard-gauge| image:: nodered-dashboard-gauge.png
.. |dashboard-chart| image:: nodered-dashboard-chart.png

.. admonition:: Installeren van dashboard-nodes

  De dashboard-nodes zijn niet altijd beschikbaar in het node-palet links.
  Voor het toevoegen van deze nodes aan NodeRed gebruik je de volgende stappen:

  * selecteer hamburger-menu (rechts) -> Manage Palette
  * selecteer de tab *Install*
  * type in het zoekveld: dashboard
  * klik op "install" voor *node-red-dashboard* *(A set of dashboard nodes for Node-RED)*
  * na deze installatie zijn de nodes in het palet links beschikbaar.

  Als je FRED gebruikt, dan kun je de nodes als volgt installeren:

  * selecteer in de FRED-zijbalk (helemaal links): Tools-> add or remove nodes
  * type in het zoekveld: dashboard
  * vink aan: *Dashboard (a set of dashboard nodes for NodeRed)*.

Om een dashboard te maken moeten we eerst de gegevens van de sensoren ophalen.
In dit geval (IoT-knoop als webserver) hebben we hierbij twee problemen:

* de IoT-knoop-webserver is alleen beschikbaar in het lokale netwerk.
  Dit betekent dat we het dashboard alleen via een computer in het lokale netwerk kunnen laten werken.
  Dit kan bijvoorbeeld een Raspberry Pi met NodeRed zijn.
* de gegevens zijn beschikbaar als HTML-document (tekst).
  Hierin moeten we de sensorgegevens zien te vinden.
  NodeRed heeft o.a. een html-node om elementen uit een HTML-document te selecteren.

Voor demonstratiedoeleinden gebruiken we hier een website die hetzelfde interface heeft als de IoT-knopen.
De website http://infvopedia.nl:1880/sensors is gekoppeld aan de hardware-IoT-knoop ``ec54``.
In het lokale netwerk zou je deze kunnen benaderen via ``http://esp8266-ec54.local/``.

.. figure:: IoT-webserver-dashboard-flow.png
   :width: 600 px
   :align: center

   Webserver dashboard flow

Uitleg bij deze flow:

* je start de flow door op de "inject"-knoop te klikken.
  (Dit kun je later automatiseren.)
* de volgende actie is het versturen van een HTTP GET-request,
  met als URL: ``http://infvopedia.nl:1880/sensors``
* het resultaat daarvan vind je via de debug-knoop in het debug-venster.
  Dit resultaat is een HTML-document (tekst).
* via de html-node selecteren we alle "td" elementen in deze tekst.
  het resultaat daarvan vind je weer in het debug-venster.
* we vullen de sensorwaarden in de payload in, als normale velden (``msg.payload.temperature``, enz.),
  via de functie *Select-sensor-values*.
* via de change-nodes zetten we de payload met de gewenste sensorwaarde,
  bijvoorbeeld ``set msg.payload to msg.payload.temperature``.
  (We zouden hiervoor ook een function-node kunnen gebruiken.)
* deze payload maken we zichtbaar via de dashboard-nodes.

De html-node die we hier gebruiken is handig voor het selecteren van elementen in een html-document.

In het debug-venster vind je de outputs van de verschillende debug-nodes.
Als je een output hierin selecteert, zie je van welke node deze afkomstig is.
Je kunt een debug-node tijdelijk uitzetten met de knop aan de rechterkant.
Je moet de veranderde flow dan wel activeren met de *deploy*-knop.

De functie *Select-sensor-values*:

.. code-block:: JavaScript

  var input = msg.payload;
  msg.payload = {};
  msg.payload.temperature = parseFloat(input[1]);
  msg.payload.barometer = parseFloat(input[3]);
  return msg;

Hieronder staat de flow in JSON-notatie.
Deze kun je met Copy-Paste overbrengen en vervolgens importeren in je NodeRed-editor.

.. code-block:: json

  [{"id":"9d733e0b.01778","type":"inject","z":"f5bf33f.ce9c85","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":120,"y":80,"wires":[["56b0445f.766c5c"]]},{"id":"56b0445f.766c5c","type":"http request","z":"f5bf33f.ce9c85","name":"","method":"GET","ret":"txt","url":"http://infvopedia.nl:1880/sensors","tls":"","x":300,"y":80,"wires":[["dba2f76f.d09588","da3db487.e2c9a8"]]},{"id":"dba2f76f.d09588","type":"html","z":"f5bf33f.ce9c85","name":"","property":"payload","outproperty":"payload","tag":"td","ret":"html","as":"single","x":170,"y":220,"wires":[["ec6a7f9b.10bca8","6b21f3b7.50e694"]]},{"id":"ec6a7f9b.10bca8","type":"debug","z":"f5bf33f.ce9c85","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":370,"y":160,"wires":[]},{"id":"6b21f3b7.50e694","type":"function","z":"f5bf33f.ce9c85","name":"Select-sensor-values","func":"var input = msg.payload;\nmsg.payload = {};\nmsg.payload.temperature = parseFloat(input[1]);\nmsg.payload.barometer = parseFloat(input[3]);\nreturn msg;","outputs":1,"noerr":0,"x":380,"y":220,"wires":[["6491c42f.903274","8a35258e.865308","ee82ff5d.2ac73"]]},{"id":"6491c42f.903274","type":"debug","z":"f5bf33f.ce9c85","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":630,"y":220,"wires":[]},{"id":"7db1e8a8.5d3ca8","type":"ui_gauge","z":"f5bf33f.ce9c85","name":"","group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage","title":"Temperatuur","label":"'C","format":"{{payload}}","min":0,"max":"50","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":630,"y":300,"wires":[]},{"id":"9583a0a1.3f21e","type":"ui_chart","z":"f5bf33f.ce9c85","name":"","group":"6afe9bdf.976fec","order":0,"width":0,"height":0,"label":"Temperatuur","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"0","ymax":"50","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":630,"y":340,"wires":[[],[]]},{"id":"8a35258e.865308","type":"change","z":"f5bf33f.ce9c85","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.temperature","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":400,"y":320,"wires":[["9583a0a1.3f21e","7db1e8a8.5d3ca8"]]},{"id":"872c6883.661e3","type":"ui_gauge","z":"f5bf33f.ce9c85","name":"","group":"a4643fc8.e80d68","order":0,"width":0,"height":0,"gtype":"gage","title":"Luchtdruk","label":"units","format":"{{payload}}","min":"950","max":"1050","colors":["#00b500","#e6e600","#ca3838"],"seg1":"","seg2":"","x":620,"y":380,"wires":[]},{"id":"380c3cf1.a6ac94","type":"ui_chart","z":"f5bf33f.ce9c85","name":"","group":"6afe9bdf.976fec","order":0,"width":0,"height":0,"label":"Luchtdruk","chartType":"line","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"950","ymax":"1050","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"86400","cutout":0,"useOneColor":false,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"x":620,"y":420,"wires":[[],[]]},{"id":"ee82ff5d.2ac73","type":"change","z":"f5bf33f.ce9c85","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.barometer","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":400,"y":400,"wires":[["872c6883.661e3","380c3cf1.a6ac94"]]},{"id":"da3db487.e2c9a8","type":"debug","z":"f5bf33f.ce9c85","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":530,"y":80,"wires":[]},{"id":"a4643fc8.e80d68","type":"ui_group","z":"","name":"Web-meters","tab":"9c0984df.31b73","disp":true,"width":"6","collapse":false},{"id":"6afe9bdf.976fec","type":"ui_group","z":"","name":"Web-graphs","tab":"9c0984df.31b73","disp":true,"width":"6","collapse":false},{"id":"9c0984df.31b73","type":"ui_tab","z":"","name":"Web-dashboard","icon":"dashboard"}]

.. rubric:: Opdrachten

(a) gebruik van de flow

  * importeer de dashboard-flow in een lege flow-pagina
  * activeer deze flow (via deploy)
  * controleer de flow door op de inject-knop (in de flow) te klikken.
  * bekijk het dashboard:

(b) experiment 1: http-request

  * verwijder de link tussen de http-request-node en de html-node (``td``).
  * klik op de inject-knop, en bekijk de output van de debug-node: dit is een html-document.
  * pas de http-request node aan, voor een andere website naar keuze;
    en bekijk de html-code van die website.
  * herstel de oorspronkelijke flow (eventueel kun je alle knopen verwijderen en de flow opnieuw importeren)

(c) experiment 2: html-node

  * verwijder de link tussen de html-code (``td``) en *select-sensor-values*.
  * klik op de inject-knop, en bekijk de output van de debug-node van ``td``:
    dit is een array met alle ``td``-elementen in het html-document.
  * configureer de html-node: verander de selector in ``h1`` of ``p``.
  * controleer of je nu de overeenkomstige elementen uit het html-document krijgt.
  * herstel de oorspronkelijke flow.

(d) experiment 3: herhaald opvragen van sensordata

  * configureer de inject-node: verander de "Repeat" in "Interval";
  * vul als waarde voor het interval in: 30 seconden;
  * ga na (aan de hand van de grafiek in het dashboard) of je dit interval ook groter kunt maken.

**Opmerkingen**

* html-documenten zijn niet erg handig om data van een server te halen.
  Een formaat dat in veel website-API's gebruikt wordt is JSON:
  dat komt in het volgende hoofdstuk aan de orde.
* tegenwoordig maken websites het steeds lastiger om data uit het html-document te halen,
  bijvoorbeeld doordat deze data pas op het laatste moment ingevuld wordt vanuit javascript.
  Je kunt het geluk hebben dat zo'n website een JSON-API heeft, anders heb je pech.
* (Het FRED-voorbeeld van de Google Finance website werkt niet meer,
  en Google heeft de finance-API gedeactiveerd.)
* het herhaald opvragen van de sensorwaarden door de client bij de (web)server heet "polling".
  Dit is geen handige aanpak: je krijgt veel onnodige communicatie, zeker als de sensorwaarden niet veranderen.
  Het is handiger als de IoT-knoop deze waarden opstuurt als ze veranderd zijn:
  een aanpak daarvoor zien we in het volgende hoofdstuk.


.. todo::

  * gebruik van inject-node om led te laten knipperen
    (nb: we hebben dan wel een webserver-knoop in het publieke internet nodig, of tenminste een gesimuleerde versie daarvan).
  * gebruik van schedule-node om led via tijd te besturen
  * opdracht: maak met NodeRed een website/dashboard dat weergeeft of je morgen het eerste uur vrij hebt.
    (...als de rooster-website van je school dit mogelijk maakt...)
