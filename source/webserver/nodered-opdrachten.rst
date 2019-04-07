NodeRed-opdrachten
==================

.. bij webserver-keten

.. admonition:: Leerdoelen en concepten

  * Gebruik van NodeRed als Webserver;
  * Kennismaking met het HTTP-protocol, met behulp van NodeRed;
  * NodeRed voorbeeld: dashboard van sensordata;
  * NodeRed voorbeeld: bediening van de actuatoren (LED);

.. admonition:: Wat heb je nodig?

  * NodeRed-installatie, bijvoorbeeld:

      * FRED - https://fred.sensetecnic.com (gratis versie)
      * NodeRed op Raspberry Pi (onderdeel van standaardsoftware)

Voorkennis: elementaire HTML-kennis.

1. Eerste webserver
-------------------

NodeRed is een HTTP-server op basis van Node.js.
Dit kun je vergelijken met Apache of Nginx op andere systemen.
Met NodeRed kun je snel een simpele webserver maken.
We gebruiken dit voor het bestuderen van het HTTP-protocol.

We gebruiken hiervoor de volgende knopen:

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

Een HTTP-request van een browser bevat onder andere de volgende onderdelen:

* het pad-gedeelte van de URL van het HTTP-request, bijvoorbeeld: ``/mypage``.
* de HTTP-request *method*, bijvoorbeeld ``GET`` of ``POST``.

Maak met deze nodes de volgende flow:

.. figure:: Nodered-hello.png
   :width: 600 px
   :align: center

   NodeRed http flow-voorbeeld

(De namen van de knopen hoef je niet aan te passen.)

Door het configureren van de nodes maken we een eigen webserver,
voor het afhandelen van een eigen pagina.

1. De eerste stap in een HTTP-flow is een http-input-node.
   Deze configureren we als volgt (dubbel-klik op de node):
   1. gebruik als *method*: ``GET``
   2. gebruik als *URL*: ``/mypage``
2. verbind een debug-node met de output van deze http-input-node.
   De andere verbindingen kun je laten zoals ze zijn.
3. Configureer de debug-node *Output*: ``complete msg object`` (en "Done").
4. "Deploy"

5. Nu kun je testen of een GET-request voor ``/mypage`` afgehandeld wordt.

Gebruik hiervoor als URL in een browser:

* voor FRED: de URL van je NodeRed-instantie, gevolgd door ``/api/mypage``.
  Bijvoorbeeld:  ``https://anna.fred.sensetecnic.com/api/mypage``
* voor een Raspberry Pi: de URL van de NodeRed-instantie, gevolgd door ``/mypage``.

In het geval van FRED moet je dit doen in een venster van de browser waarmee je ingelogd bent bij FRED.
Dit betekent dat alleen jij deze website kunt zien, anderen niet.
Dit is een beperking van de gratis versie van FRED.

Een "normale" NodeRed-installatie heeft deze beperking niet: iedereen kan dan je webpagina zien.

Als response in de browser krijg je:  ``This is the payload: [object Object] !``

6. zoek in de debug-output naar het ``req``-deel van het msg-object.
   Daarin vind je onder andere de velden ``URL`` en ``method``.
   Controleer of deze kloppen met wat je verwacht.

7. De volgende stap is het aanpassen van de webpagina.
   Configureer de template-node, en vul als template-waarde in:

.. code-block::html

  <!doctype HTML>
  <html>
    <head>
      <title>My page</title>
    </head>
    <body>
      <h1>Welkom op mijn website</h1>
    </body>
  </html>

Vul als *Name* in: ``mypage``.
En "Done" en "Deploy".

Open nu in de browser een webpagina met de URL van je NodeRed-pagina.
(Bijvoorbeeld: ``https://anna.fred.sensetecnic.com/api/mypage``.)
Controleer of je nu je eigen webpagina te zien krijgt.

2. Een tweede pagina
--------------------

De volgende stap is het maken van een tweede pagina voor je website.

1. Kopieer de flow met de 3 nodes: http-input, template, http-output.
   Hiervoor kun je in NodeRed Copy-Paste gebruiken: (i) selecteer de nodes;
   (ii) Copy; (iii) Paste.
2. Configureer de tweede http-input-node, met *URL*: ``my2ndpage``.
   Als *method* laat je ``GET`` staan. En "Done".
3. Configureer de template-node, en vul als *template* in:

.. code-block:: html

  <!doctype HTML>
  <html>
    <head>
      <title>My 2nd page</title>
    </head>
    <body>
      <h1>Mijn tweede pagina</h1>
    </body>
  </html>

"Done" en "Deploy".

4. Controleer in een browser-venster of deze URL werkt.
   (Bijvoorbeeld: ``https://anna.fred.sensetecnic.com/api/my2ndpage``)
5. Maak nu een link van deze pagina naar de vorige.
   Pas de template-tekst van de template node daarvoor aan,
   en voeg toe onder ``<h1>...</h1>``:

Voor NodeRed via FRED:

.. code-block:: html

  <p>
    Dit is mijn tweede webpagina.
    De eerste vind je via deze link:
    <a href="/api/mypage">Home page<a>
  </p>

*Opmerking*: in deze html-code gebruiken we de URL ``/api/mypage``.
Dit is nodig voor de FRED-versie.
Voor andere NodeRed-installaties gebruik je als URL: ``/mypage``.

"Done" en "Deploy".

Bezoek deze pagina in de brower,
en controleer of de link naar de homepagina werkt.

6. Voeg op dezelfde manier een link toe van je eerste pagina naar je tweede.
   Controleer of deze werkt.

Je hebt nu een website met twee pagina's die onderling verbonden zijn.

Je kunt de tekst van de pagina's zo groot maken als je wilt.
Vaak is het handig om grotere teksten in een bestand op te slaan.
Dit kun je dan inlezen via de file-node;
in FRED is deze helaas niet beschikbaar.

3. Een teller
-------------

De website die we tot nu toe gemaakt hebben is *statisch*:
dat wil zeggen dat de inhoud niet afhangt van de toestand van de server.
Bij eenzelfde URL krijg je altijd hetzelfde resultaat.

Een volgende stap is dat we de website voorzien van een bezoekers-teller:
elke keer als er een http-verzoek voor een webpagina binnenkomt,
verhogen we deze teller.
We laten de huidige waarde van de teller in de webpagina zien.
Dit is een eenvoudig voorbeeld van een *dynamische website*.

Hiervoor maken we gebruik van context-variabelen in NodeRed,
zie: https://nodered.org/docs/user-guide/context.
In een context kun je een waarde opslaan die tussen de verschillende request bewaard blijft.
We gebruiken de *flow-context*: deze is gemeenschappelijk voor de flows op één pagina.

Deze opdracht is ook een demonstratie van het gebruik van *templates*:
een tekst waarin je de waarde van variabelen kunt invullen.
NodeRed gebruikt voor deze templates de Mustache-notatie,
zie: https://mustache.github.io.

1. Voeg een functie-node in tussen de http-input-node (``api/mypage``) en de template-node:
   Verwijder eerst de verbinding tussen de http-input-node en de template-node.
   Sleep een function node naar de flow.
   Verbind de output van de http-input-node met de input van de function-node.
   Verbind de output van de function-node met de input van de template-node.
   (En maak de layout weer netjes.)
2. Configureer de function-node: *Name*: ``inc-count``; *Function*:

.. code-block:: javascript

  var count = flow.get("count") || 0;
  count = count + 1;
  flow.set("count", count);
  msg.count = count;
  return msg;

In de eerste regel halen we de waarde van de flow-variabele "count" op.
Als deze nog niet gedefinieerd is, gebruiken we de waarde 0.
(Dit is een veel-voorkomende JavaScript-constructie.)
Deze waarde hogen we met 1 op, en bewaren deze weer in de flow-variabele "count".
De nieuwe waarde voegen we toe aan de NodeRed-message ``msg``,
om later in het template in te vullen.

3. Configureer de template-node.
   Voeg in, vóór  ``</body>``:

.. code-block:: html

  <p> visits: {{count}} </p>

Via de constructie ``{{count}}`` wordt de waarde van de variable ``msg.count`` in de template-tekst ingevuld.

4. "Deploy"

Controleer via de browser of je bij elke reload van de pagina
(bijvoorbeeld ``https://anna.fred.sensetecnic.com/api/mypage`` )
een volgende waarde van de teller krijgt.

4. LED-besturing
----------------

In deze opdracht werken we uit hoe je via een webserver een LED kunt aansturen.
In de FRED-versie hebben we geen toegang tot een LED;
we simuleren deze door de kleur in de webpagina.

In de Raspberry Pi-versie heb je vanuit NodeRed toegang tot de GPIO-pinnen.
Daarmee kun je eventueel een LED aansturen.

Voor het aansturen van de LED gebruiken we twee URLs: ``/ledon`` en ``/ledoff``.
Hiervoor maken we twee flows, met voor elk dezelfde opzet als bij de teller:
een http-input-node, een function-node, een template-node, en een http-output-node.

.. figure:: Nodered-flow-led-on-off.png
   :width: 600 px
   :align: center

   NodeRed flow voor led-besturing

1. Maak deze flow voor ``ledon``, door de nodes naar het flow-venster te slepen en verbinden.
2. Configureer de http-input-node: *URL*: ``/ledon``, *method*: ``GET``.
3. Configureer de function node: *Name*: ``led-on``, ``Function``:

.. code-block:: javascript

  msg.color = "red";
  return msg;

Als je toegang hebt tot hardware zul je in deze functie de LED uitschakelen.

4. Configureer de template node: *Template*:

.. code-block:: html

  <!doctype HTML>
  <html>
    <head>
      <title>LED control</title>
    </head>
    <body>
      <h1>LED control</h1>
      <p>
        <a href="/api/ledon">on</a>
        <span style="color: {{color}}"> [[LED]]</span>
        <a href="/api/ledoff">off</a>
      </p>
    </body>
  </html>

*Opmerking*: in deze html-code gebruiken we de URLs ``/api/ledon`` en ``/api/ledoff``.
Dit is nodig voor de FRED-versie.
Voor andere NodeRed-installaties laat je het ``/api``-deel weg.

5. Kopieer deze flow voor ``ledoff``
6. Configureer in deze kopie de http-input-node: *URL*: ``/ledoff``.
7. Configureer de function-node: *Name*: ``led-off``, ``Function``:

.. code-block:: javascript

  msg.color = "black";
  return msg;

Als je toegang hebt tot hardware zul je in deze functie de LED uitschakelen.

8. De template-node hoef je niet aan te passen.
9. "Deploy" en controleer via de browser de werking van de webpagina's.
   (bijvoorbeeld: ``https://anna.fred.sensetecnic.com/api/ledon``)
10. Je kunt deze flows vereenvoudigen: voor beide flows zijn de "staarten" gelijk.
    Deze kun je combineren: verbind de output van function-node ``led-off``
    met de template-node in de flow van ``/ledon``.
    Verwijder de tweede template-node en de bijbehorende http-output-node.
    Je krijgt dan onderstaande flow:

.. figure:: Nodered-flow-led-on-off-combined.png
   :width: 600 px
   :align: center

   Ledbesturing met gedeelde response-"staart"

Door slim gebruik te maken van templates kun je vaker flows op zo'n manier combineren.

.. topic:: Idempotente opdrachten

  Waarom gebruiken we hier *twee links (knoppen)* voor het besturen van de LED?
  Je kunt toch met één drukknop een lamp aan- en uitzetten?
  De ene keer drukken zet de lamp aan, de volgende zet deze weer uit.
  Maar: deze aanpak geeft problemen als de drukknop niet betrouwbaar is,
  zoals bij communicatie vaak het geval is.
  Als een omschakelbericht niet aankomt,
  heeft het volgende bericht een tegengestelde betekenis.
  Door twee knoppen te gebruiken, heeft elke knop een duidelijke betekenis.
  Je kunt dan een knop nog een keer gebruiken,
  "voor de zekerheid", bijvoorbeeld als je nog geen reactie gezien hebt.

  Een opdracht die dezelfde betekenis houdt als je deze herhaalt noemen we *idempotent*.
  Het maakt dan niet uit of je deze 1, 2 of 10 keer uitvoert.

  De HTTP GET-opdracht voor het ophalen van een webpagina is idempotent.
  Je kunt altijd een "reload" van een webpagina doen: je krijgt dan hetzelfde resultaat.

  De HTTP POST-opdracht, voor het insturen van een formulier, is niet idempotent.
  De browser geeft een waarschuwing als je voor een formulier een "reload" uit wilt voeren:
  je loopt bijvoorbeeld het risico dat je een artikel nog een keer bestelt.

  *Vraag*: welke knoppen op een TV-afstandsbediening zijn idempotent?

5. Webformulieren
-----------------

In deze opdracht gaan we aan de slag met een webformulier:
in de browser vul je de waarden in het formulier in;
de browser stuurt het formulier via een POST-request (in plaats van GET) naar de server;
de server verwerkt dit request, en stuurt een (HTML)document terug.

Een formulier heeft in HTML de vorm:

.. code-block:: html

  <form action="/form-url" method="post">
    ... <input type="text" name="inputname1"> ...
    ... <input type="number" name="inputname2"> ...
    <button type="submit">Submit</button>
  </form>

Bij de form-tag geef je op wat de URL is van het formulier,
en wat de bijbehorende http-method is:
in dit voorbeeld, POST met als URL ``/form-url``.
Daarna volgen een aantal input-velden, voor tekst, wachtwoord, datum, meerkeuze, enz.
Een formulier heeft meestal een *submit-button* waarmee je het ingevulde formulier opstuurt.

We gebruiken hier een formulier voor het aansturen van de LED:

.. code-block:: html

  <form action="/leds/0" method="post">
     <button type="submit" name="on" value="1">On</button>
     <span style="color:{{color}};"> [[LED]] </span>
     <button type="submit" name="on" value="0">Off</button>
  </form>

De URL van het formulier is ``/leds/0``: dit geeft aan dat het om LED-0 gaat.
(De hardware kan meerdere LEDs bevatten.)
De method is ``POST``: via het formulier veranderen we de toestand van de LED.
(Eigenlijk zou dit een PUT-opdracht moeten zijn, maar dat kan niet in HTML;
zie de opmerkingen over REST API's verderop.)
We gebruiken hier 2 buttons: voor het aan- en uitzetten van de LED.
Beide buttons zijn submit-buttons: deze zorgen ervoor dat het formulier direct verstuurd wordt.

Het formulier heeft in dit geval 1 parameter: ``on``, met als mogelijke waarden ``0`` en ``1``.
De parameters van een formulier worden verstuurd als een (gecodeerde) string van de vorm:
``name0=value0&name1=value1...&namex=valuex``.
Voor dit voorbeeld krijgen we dan ``on=1`` of ``on=0``.

Merk op dat we nu één URL hebben voor beide schakelaars (buttons);
de waarde voor het aansturen van de LED geven we nu niet weer als twee verschillende URLs,
maar als parameter van de formulier-URL.

In de (NodeRed) server verwerken we deze parameter als volgt:

.. code-block:: javascript

  if (msg.payload.on == "1") {
      flow.set("ledOn", true);
  } else if (msg.payload.on == "0") {
      flow.set("ledOn", false);
  }

We kunnen dit ook schrijven als: ``flow.set("ledOn", msg.payload=="1")``.

1. Maak de volgende flow, door de nodes naar het flow-deel te slepen en te verbinden.

.. figure:: Nodered-form-flow-0.png
   :width: 600 px
   :align: center

   NodeRed form flow

(De namen van de nodes hoef je nog niet aan te passen.)

2. Configureer de bovenste http-input-node: *URL*: ``/led-control``,
   *method*: ``GET``.
   Dit is de node/URL voor de html-pagina met het formulier.
3. Configureer de onderste http-input-node: *URL*: ``/leds/0``,
   *method*:``POST``.
   Dit is de node/URL voor het afhandelen van het ingevulde formulier.
4. Configureer de onderste function-node (``updateLed``),
   voor het afhandelen van het formulier:

.. code-block:: javascript

  if (msg.payload.on == "1") {
      flow.set("ledOn", true);
  } else if (msg.payload.on == "0") {
      flow.set("ledOn", false);
  }
  return msg;

5. Configureer de bovenste function-node (``properties``),
   voor het zetten van de template-parameters.

.. code-block:: javascript

  if (flow.get("ledOn") || false) {
      msg.color = "red";
  } else {
      msg.color = "black";
  }
  return msg;

6. Configureer de template-node:

.. code-block:: html

  <html>
    <head>
        <title>LED server</title>
    </head>
    <body> <h1>LED control</h1>
      <p>
        <form action="/api/leds/0" method="put">
           <button type="submit" name="on" value="1">On</button>
           <span style="font-weight:bold;color:{{color}};"> [[LED]] </span>
           <button type="submit" name="on" value="0">Off</button>
        </form>
      </p>
      <p><a href="/api/led-control">Home</a></p>
    </body>
  </html>

*Opmerking*: in deze html-code gebruiken we de URLs ``/api/leds/0`` en ``/api/led-control``.
Dit is nodig voor de FRED-versie.
Voor andere NodeRed-installaties laat je het ``/api``-deel weg.


7. "Deploy" en test de website.

.. figure:: Nodered-form-gauge.png
   :width: 600 px
   :align: center

   NodeRed flow: formulier met dashboard-meter

8. Voeg als uitbreiding van deze flow, een dashboard-node ("gauge", ronde meter).
   Verbind de input daarvan met de output van de function-node ``properties``.
   Configureer deze node als volgt:

   1. Configureer deze node: *Group*: ``add new ui group``,
   2. Voeg een nieuwe ui groep toe met als naam: Simulated LED;
      gebruik hiervoor het potloodje rechts van ``add new ui group``
   3. met *Tab*: add new tab, met als naam: Simulator.

9. Pas de function-node ``Properties`` aan: zet ``msg.value`` op 0 of 10,
   voor led "aan" of "uit".

.. code-block:: javascript

  if (flow.get("ledOn") || false) {
      msg.color = "red";
      msg.value = 0;
  } else {
      msg.color = "black";
      msg.value = 10;
  }
  return msg;

10. "Deploy" en test deze flow.

.. topic:: REST-interfaces

  Waarom gebruiken we hier een formulier voor het veranderen van de toestand van de LED?
  Dit is (bijna) een voorbeeld van een REST-interface (https://en.wikipedia.org/wiki/Representational_state_transfer).
  Dit is een manier om interfaces in het web te definiëren.

  * elke *resource*, bijvoorbeeld een LED, heeft een eigen adres (URL) in het web.
    In dit geval is het adres van de LED: "/led/0".
  * voor het opvragen van de toestand van een resource gebruik je een HTTP GET-opdracht.
    De afspraak is dat je hiermee de toestand niet verandert:
    voor de resource maakt het niet uit of je deze opdracht 0 maal of vaker uitvoert.
    In dat geval is GET een *veilige* opdracht (ook wel: *nullipotent*).
  * voor het veranderen van de toestand van een resource gebruik je een andere opdracht;
    voor dit voorbeeld zou dit een HTTP PUT moeten zijn; deze is *idempotent*,
    als je deze herhaalt blijft de betekenis gelijk.
  * we kunnen een PUT-opdracht niet gebruiken in een HTML-formulier:
    dat kan alleen vanuit JavaScript.
    Voorlopig behelpen we ons hier met een POST:
    we houden ons daarmee niet aan de regels voor REST API's.

  *Vraag*: bestudeer de (onofficiële) documentatie van de Philips Hue Bridge
  (http://www.burgestrand.se/hue-api/api/lights/).
  Met welke opdracht zet je een lamp aan? Met welke uit?

6. (*)Sensor-dashboard
----------------------

Met enige moeite kunnen we met NodeRed de (sensor)gegevens van een website halen en weergeven in een dashboard.
Hiervoor halen we een html-document op van een website (met een HTTP GET request),
vervolgens ontleden we dit document om de sensorgegevens eruit te halen.
Daarna geven we deze sensorgegevens weer in een dashboard.

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

Zie voor het installeren van deze nodes de NodeRed-inleiding (in Bouwstenen).

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
In het lokale netwerk zou je deze kunnen benaderen via ``http://esp8266-ec54.local/`` of via het IP-adres.

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
