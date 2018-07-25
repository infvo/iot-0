Opdrachten
==========

In de opdrachten gebruiken we een IoT-knoop met daarop een webserver.
Via een computer (browser) in hetzelfde netwerk als deze IoT-knoop maak je contact met deze webserver:
daarmee kun je sensorwaarden van de IoT-knoop opvragen en de actuatoren (LEDs) van de knoop besturen.

.. todo::

   * één webserver-programma voor LED(s) en sensoren.
   * opmerking over (te) primitieve aanpak van de webserver,
     niet volgens de REST-conventies van het web.
     Voordeel van de gekozen aanpak: deze kun je zelf via het URL-venster uitvoeren.
   * ontwikkelen van een alternatieve versie, wel volgens de REST-conventies?
   * een uitgebreide versie, met JSON-beschrijving van de toestand?

(1) Adresseren van de webserver
-------------------------------

In het eerste voorbeeld gebruiken we een IoT-knoop met een webserver voor het aansturen van een LED.

.. admonition:: Wat heb je nodig?

  * IoT-knoop met LED en webserver-software (``LED-webserver-0``);
  * computer of smartphone met browser;
  * computer en IoT-knoop moeten beide verbonden zijn in het lokale (WiFi) netwerk.

*Hoe vind je de webserver in de browser?*
Een webpagina adresseer je in de browser met een URL.

* Het eerste onderdeel van de URL geeft het protocol aan, in dit geval ``http:`` .
* Het volgende onderdeel is het adres van de server.
  Dit kan een *domeinnaam* zijn, zoals ``infvo.nl``, of een *IP-adres*, zoals ``xxx.xxx.xxx.xxx``.
  De browser gebruikt een Domain Name Server (DNS-server) om het IP-adres te vinden van een domeinnaam.
* Na het server-adres volgt de *padnaam*: dit identificeert de webpagina op de server.

Webservers in het lokale netwerk komen niet voor in de administratie van een publieke DNS-server.
Voor het lokale netwerk kun je een andere technologie gebruiken: mDNS (multicast DNS).
Je kunt dan een lokale webserver (van een apparaat) vinden aan de hand van een naam,
je hoeft het IP-adres niet te weten.
(Niet alle operating systems ondersteunen mDNS.)

De webserver-software meldt zich bij mDNS aan met de naam: ``esp8266-xxxx.local``,
waarin ``xxxx`` de laatste 4 cijfers van het *MAC-adres* van de knoop zijn.
Het MAC-adres is een adres dat gebonden is aan een apparaat: dit verandert niet
(Eigenlijk is het MAC-adres gebonden aan het netwerk-interface van het betreffende apparaat.
In het geval van de ESP8266 is dit het MAC-adres van het WiFi-interface.)
Het MAC-adres van de IoT-knoop is het hardware-adres van het WiFi-interface.
Dit adres is vast onderdeel van het apparaat, en is wereldwijd uniek.
Een WiFi-MAC-adres is een 48-bits adres, geschreven als een reeks van 6 bytes in hexadecimale notatie: ``00:00:00:00:00:00``.

* Zie: https://en.wikipedia.org/wiki/MAC_address.

Het IP-adres van een apparaat is *gebonden aan het netwerk*:
dit adres verandert als je het apparaat in een ander netwerk verbindt.
Bovendien kan het veranderen als je het apparaat opnieuw in hetzelfde netwerk verbindt.

1. zoek uit wat het MAC-adres is van je IoT-knoop; dit staat waarschijnlijk op het apparaat;
2. zoek in de browser de webserver via de URL: ``http://esp8266-xxxx.local``,
   met voor ``xxxx`` de laatste 4 tekens van het MAC-adres.
3. je krijgt nu de webpagina van de IoT-knoop te zien (als het goed is).
   Deze geeft ook aan wat het eigen IP-adres is.
4. zoek in de browser de webserver via de URL: ``http://aaa.bbb.ccc.ddd``,
   waarin ``aaa.bbb.ccc.ddd`` het IP-adres van de IoT-knoop is.

Als de omgeving van je browser geen mDNS ondersteunt, probeer het dan via een andere computer of smartphone.
Voor zover bekend ondersteunen OS X, iOS en Android mDNS.

Opmerkingen:

* je kunt het IP-adres vinden via de ontwikkelaarstools in de browser, zie opdracht XXX.
* je kunt het IP-adres vinden via het ``ping``-commando (in Unix/Linux), bijvoorbeeld:
  ``ping esp8266-8f12.local``
* je kunt het IP-adres vinden in de DHCP-tabel van de lokale router/gateway (als je daar toegang toe hebt).

(2) Aansturen van de LED
------------------------

.. admonition:: Wat heb je nodig?

   * de opstellingn van de vorige opdracht
   * de gegevens van de vorige opdracht

We gebruiken de webserver om een LED van de IoT-knoop aan- en uit te zetten.

1. Open de webpagina van de IoT-knoop, zoals in de vorige opdracht;
2. Je ziet twee "knoppen" (web-links), om de LED aan- en uit te zetten.
   Zet de LED aan.
   De LED op de IoT-knoop brandt nu (als het goed is).
   Wat is nu de URL (in het URL-venster van de browser)?
3. Zet de LED uit.
   De LED is nu uit (als het goed is).
   Wat is nu de URL?
4. Verander de URL in het URL-venster van de browser,
   met de URL gevonden bij (2), en laad de pagina opnieuw.
   Wat gebeurt er?

We zien hier dat je een apparaat kunt besturen door verschillende URLs (met verschillende *padnamen*) te gebruiken.
In de webserver komt elke URL dan overeen met een afzonderlijke actie.
Dit vind je terug in de broncode van de webserver:

.. code-block:: c++

  server.on("/", handleRoot);
  server.on("/ledon", handleLedOn);
  server.on("/ledoff", handleLedOff);

Opmerkingen:

* We gebruiken in dit voorbeeld verschillende URL-padnamen, om de code van de webserver eenvoudig te houden.
  Een andere manier is om te werken met *parameters* in een URL.
  (Voorbeelden?)

(3) Browser-ontwikkelaarstools
------------------------------

* bestudeer de brontekst van het html-document, via de browser webtools, bronbestanden-sectie.
* ga via de browser webtools na wat het IP-adres is van de webserver (netwerk-sectie, "headers"/"kopteksten" gedeelte)
    * soms krijg je meer informatie als je op de naam van het document klikt
    * uit hoeveel tekens (bytes) bestaat het brondocument?
    * welke URL wordt gebruikt voor het inschakelen van de LED? welke voor het uitschakelen?
* je kunt de webserver benaderen via het IP-adres of via de lokale domeinnaam.
    * ga na (via de browser webtools, netwerk-sectie) of dit verschil uitmaakt in de totale tijd tussen aanvraag en resultaat.

(4) Uitlezen van sensor-waarden
-------------------------------

We kunnen via de webserver ook de waarden van de sensoren in de IoT-knoop uitlezen.
Deze kunnen we als getallen beschikbaar maken, maar we kunnen ook kiezen voor een grafische weergave,
bijvoorbeeld in de vorm van een dashboard.

* hiervoor heb je een knoop nodig met de ``sensor-webserver-0``-software.
* elke keer als je de webpagina ververst krijg je de actuele sensorwaarden te zien.
* je krijgt veranderde sensorwaarden niet automatisch te zien: je moet daarvoor de webpagina verversen.
    * dit verversen kun je wel automatiseren, maar dat verandert niets aan het principe: de client vraagt aan de server wat de actuele toestand is.
    * regelmatig de toestand opvragen heet ook wel "polling"; dit staat tegenover het wachten op een bericht met een veranderde toestand.


(5) De programmatekst van de IoT-knoop
--------------------------------------

In de programmatekst van de IoT-knoop kun je zien hoe de server een verzoek afhandelt,
en op basis van het URL-pad beslist welke actie op de LED plaatsvindt.

* welke functie bevat de tekst van de webpagina?
* welke functie wordt aangeroepen bij een request met URL ``/``?
* welke functie wordt aangeroepen bij een request met URL ``/ledon``?
* welke functie wordt aangeroepen bij een request met URL ``/ledoff``?
* wat gebeurt er als je een onbekende URL invoert?
    * geef daarbij eventueel *parameters* mee, bijvoorbeeld ``?x=123&y=groen``

(6) Een eigen voorbeeld
-----------------------

Zoek een apparaat in je omgeving dat via een webinterface bediend kan worden.
Enkele suggesties: router; netwerkprinter; IoT-gateway (zoals de Hue Bridge).

1. Maak een schermafdruk van een bedieningspagina van dit apparaat.
2. beschrijf de karakteristieken van dit apparaat:
    a) is de webserver altijd online?
    b) hoe kun je de webserver vinden?
    c) hoe krijg je veranderingen in de toestand van het apparaat gemeld?
    d)  moet je daarvoor de pagina in de browser verversen?

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

.. |br| raw:: html

   <br />
