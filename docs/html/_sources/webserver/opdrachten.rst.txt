Webserver-Opdrachten
====================

.. bij Webserver

In de opdrachten gebruiken we een IoT-knoop met daarop een webserver.
Via een computer (browser) in hetzelfde netwerk als deze IoT-knoop maak je contact met deze webserver:
daarmee vraag je sensorwaarden van de IoT-knoop op en bestuur je de actuatoren (LEDs) van de knoop.

(1) Adresseren van de webserver
-------------------------------

In het eerste voorbeeld gebruik je een IoT-knoop met een webserver voor het aansturen van een LED en
voor het uitlezen van sensorwaarden.

.. admonition:: Wat heb je nodig?

  * IoT-knoop met LED en webserver-software (``sensor-webserver-0``);
  * computer of smartphone met browser;
  * computer en IoT-knoop moeten beide verbonden zijn in het lokale (WiFi) netwerk.

*Hoe vind je de webserver in de browser?*
Een webpagina adresseer je in de browser met een URL.

* Het eerste onderdeel van de URL geeft het protocol aan, in dit geval ``http:`` .
* Het volgende onderdeel is het adres van de server.
  Dit kan een *domeinnaam* zijn, zoals ``infvo.nl``, of een *IP-adres*, zoals ``192.168.1.103``.
  De browser gebruikt een Domain Name Server (DNS-server) om het IP-adres te vinden van een domeinnaam.
* Na het server-adres volgt de *padnaam*: dit identificeert de webpagina op de server.

Een webserver in het lokale netwerk komt niet voor in de administratie van een publieke DNS-server.
Voor het lokale netwerk kun je een andere technologie gebruiken: mDNS (multicast DNS;
niet alle operating systems ondersteunen dit).

De sensor-webserver-software meldt zich bij mDNS aan met de naam: ``esp8266-xxxx.local``,
waarin ``xxxx`` de laatste 4 cijfers van het WiFi MAC-adres van de knoop zijn.
Dit unieke adres is een *overanderlijk onderdeel van het apparaat*.
Een WiFi MAC-adres is een 48-bits adres, geschreven als een reeks van 6 bytes in hexadecimale notatie: ``00:00:00:00:00:00``.

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

* je kunt het IP-adres vinden via de ontwikkelaarstools in de browser, zie opdracht (3).
* je kunt het IP-adres vinden via het ``ping``-commando (in Unix/Linux), bijvoorbeeld:
  ``ping esp8266-8f12.local``
* je kunt het IP-adres vinden in de DHCP-tabel van de lokale router/gateway (als je daar toegang toe hebt).

(2) Aansturen van de LED
------------------------

.. admonition:: Wat heb je nodig?

   * de opstelling van de vorige opdracht
   * de gegevens van de vorige opdracht

We gebruiken de webserver om een LED van de IoT-knoop aan- en uit te zetten.

1. Open de webpagina van de IoT-knoop, zoals in de vorige opdracht;
2. Je ziet twee knoppen (buttons) om de LED aan- en uit te zetten.
   Zet de LED aan.
   De LED op de IoT-knoop brandt nu (als het goed is).
   Wat is nu de URL in het URL-venster van de browser?
3. Zet de LED uit.
   De LED is nu uit (als het goed is).
   Wat is nu de URL?
4. Wat gebeurt er als je in de browser de pagina opnieuw laadt ("refresh" of "reload")?
5. Wat gebeurt er als je de URL van (2) of (3) invoert in het URL-venster van de browser?

Je ziet hier dat je een apparaat kunt besturen met een webpagina.
De beide buttons zijn onderdeel van een *webformulier*.
De browser stuurt dit formulier via een HTTP-POST-request naar de server,
met als naam-waard-paren: ``on=1`` voor "aan", ``on=0`` voor "uit".

De server verwerkt het verzoek als volgt:

.. code-block:: c++

  void handleLed0() {
    if (server.method() == HTTP_POST) {
      for (uint8_t i=0; i < server.args(); i++) {
        if (server.argName(i) == "on") {
          String argvalue = server.arg(i);
          if (argvalue == "0") {
            digitalWrite(ledPin, LOW);
          } else if (argvalue == "1") {
            digitalWrite(ledPin, HIGH);
          }
        }
      }
    }
    sendHTMLdoc();
  }

  server.on("/", handleRoot);
  server.on("/leds/0", handleLedOn);

(3) Browser-ontwikkelaarstools
------------------------------

* bestudeer de brontekst van het html-document, via de browser ontwikkelaarstools, bronbestanden-sectie.
    * deze vind je bij Chrome: Weergave->Ontwikkelaar->Ontwikkelaarstools
    * bij FireFox: Extra->Webontwikkelaar->Hulpmiddelen in-/uitschakelen
    * bij Safari: Ontwikkel->Toon webinfovenster (mogelijk moet je in de voorkeuren instellen dat dit menu getoond wordt: )
* ga via de browser webtools na wat het IP-adres is van de webserver (netwerk-sectie, "headers"/"kopteksten" gedeelte)
    * soms krijg je meer informatie als je op de naam van het document klikt
    * uit hoeveel tekens (bytes) bestaat het brondocument?
    * welke URL wordt gebruikt voor het inschakelen van de LED? welke voor het uitschakelen?
    * welke verzoekgegevens worden gebruikt voor het in- en uitschakelen van de LED?
* je kunt de webserver benaderen via het IP-adres of via de lokale domeinnaam.
    * ga na (via de browser webtools, netwerk-sectie) of dit verschil uitmaakt in de totale tijd tussen aanvraag en resultaat.

(4) Uitlezen van sensorwaarden
-------------------------------

Via de webserver lees je ook de waarden van de sensoren in de IoT-knoop uit.

* hiervoor heb je een knoop nodig met de ``sensor-webserver-0``-software.
* elke keer als je de webpagina ververst krijg je de actuele sensorwaarden te zien.
* je krijgt veranderde sensorwaarden niet automatisch te zien: je moet daarvoor de webpagina verversen.
    * dit verversen kun je wel automatiseren, maar dat verandert niets aan het principe:
      de client vraagt aan de server wat de actuele toestand is.
    * regelmatig de toestand opvragen bij de server heet ook wel "polling";
      dit staat tegenover het wachten op een bericht als de toestand veranderd is.


(5) De programmatekst van de IoT-knoop
--------------------------------------

In de programmatekst van de IoT-knoop kun je zien hoe de server een verzoek afhandelt,
en op basis van het URL-pad beslist welke actie op de LED plaatsvindt.

* welke functie bevat de tekst van de webpagina?
* welke functie wordt aangeroepen bij een request met URL ``/``?
* welke functie wordt aangeroepen bij een request met URL ``/leds/0``?
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
