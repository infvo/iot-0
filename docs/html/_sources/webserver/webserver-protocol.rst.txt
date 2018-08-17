Protocollen
===========

.. figure:: IoT-webserver-protocol.png
   :width: 500 px
   :align: center

   Webserver protocolstack

De protocolstack voor de webserver-keten bevat de volgende elementen, vanaf de toepassing gezien:

* web-app
* web: hypertext transfer protocol (HTTP): HTML-documenten enz.
* internet: transmission control protocol (TCP): betrouwbare bytestromen
* internet-protocol (IP): best-effort pakketcommunicatie;
  universeel datatransport, onafhankelijk van hardware en toepassingen
* fysieke (hardware) verbinding: WiFi of Ethernet

Elk protocol heeft zijn eigen vorm van *adressering* en zijn eigen formaat voor datatransport.
Dit bevat de "netto" te transporteren data: de *payload*,
en extra protocolgegevens voor de adressering, de beveiliging van de payload, enz.

.. topic:: 3-lagen model

  In de ICT komen we op allerlei plaatsen het 3-lagen model tegen:

  * toepassingen-laag
  * logische laag (systeemsoftware)
  * fysieke laag (hardware)

  De *logische laag* schermt de toepassingen af van de onderliggende hardware:
  je kunt daardoor eenzelfde toepassing combineren met allerlei verschillende hardware.
  De logische laag schermt ook de hardware af van de toepassingen:
  je kunt dezelfde hardware gebruiken voor allerlei verschillende toepassingen.
  De logische laag zorgt er ook voor dat de evolutie van de toepassingen onafhankelijk is van de evolutie van de hardware.
  Je kunt je bestaande toepassingen gebruiken op (bijna) elke nieuwe computer.
  En je hoeft niet voor elke nieuwe toepassing een nieuwe computer te kopen.
  Zowel de evolutie van de hardware als die van de software kunnen daardoor veel sneller verlopen.

  (In de begintijd van de computer waren de hardware en de toepassingssoftware veel directer gekoppeld.
  De ontkoppeling door een universele logische laag is een cruciale stap in de ontwikkelingn van de computer.)

  Voorbeelden van dit model:


.. topic:: end-to-end principe

  Het IP-protocol verzorgt het datatransport in het internet.
  Dit protocol is *in het netwerk* aanwezig: in de verbonden computers en in de routers e.d.
  De hogere protocollen (TCP, UDP; HTTP; enz.) zijn alleen in de eindpunten aanwezig.
  Het IP-netwerk is hierdoor onafhankelijk van de toepassingen - en daardoor geschikt voor (bijna) alle toepassingen.
  Dit betekent dat je voor een nieuwe toepassing het netwerk niet hoeft aan te passen.
  Iedereen kan een nieuwe toepassing voor het internet maken: hiervoor is alleen software in de eindpunten nodig,
  bijvoorbeeld als toepassingssoftware op een smartphone in combinatie met een server.

  Het IP-protocol is universeel wat de fysieke verbindingen betreft:
  dit protocol schermt de (meeste) eigenschappen van de fysieke verbindingen af voor de hogere protocols.
  Het IP-protocol is ook universeel wat de toepassingen betreft:
  het schermt de eigenschappen van de toepassingen af van de onderliggende hardware (*3-lagen model*).


Adressering
-----------

Elk protocollen in de stack heeft een eigen vorm van adressering.
In het gebruik van de IoT-knoop-webserver kun je deze allemaal tegenkomen.

.. csv-table:: Adressering in de protocolstack
  :header: "Protocol", "Adres", "Voorbeeld"
  :widths: 20, 20, 20

  "HTTP", "URL-pad",   "``/leds/0``"
  "TCP",  "TCP-poort", "``80`` (voor HTTP)"
  "IP",   "IP-adres",  "``94.132.250.160``"
  "WiFi Ethernet", "MAC-adres", "``5CCF7F6EAE3B``"

Het MAC-adres is gekoppeld aan de hardware: dit identificeert de IoT-knoop.
Als domeinnaam van de IoT-knoop-webserver gebruiken we ``esp8266-xxxx``,
waarbij ``xxxx`` de laatste 4 tekens van het MAC-adres zijn.

De webserver adresseer je vanuit de browser via een URL van de vorm: ``http://<domeinnaam>:<poortnummer>/<URL-pad>``

* ``http`` geeft het protocol aan voor de interactie;
* ``<domeinnaam>`` is de domeinnaam of het IP-adres van de webserver, voor het IP-protocol;
* het ``<poortnummer>`` geeft de TCP-poort aan; als je dit onderdeel weglaat, wordt voor HTTP poort 80 gebruikt;
* de webserver gebruikt het ``<URL-pad>`` om de verschillende verzoeken te onderscheiden.
  Een URL-pad identificeert een *resource*: in het geval van een IoT-knoop, bijvoorbeeld een sensor of een actuator.

Voorbeeld: ``http://esp8266-ae3b/leds/0``

De webserver krijgt dynamisch een lokaal IP-adres toegewezen van de lokale router, door middel van DHCP.
De webserver laat dit adres zien via de Arduino-IDE (Seriële Monitor).
Soms heeft de webserver een display waarop dit IP-adres te vinden is.
Als je browser verbinding heeft met de webserver vind je het IP-adres ook via de *browser-ontwikkelaarstools*.

Payload
-------

Elk van de protocollen in de stack heeft een eigen dataformaat:
dit bevat *header*-gegevens voor het protocol zelf, bijvoorbeeld voor de adressering,
en de *payload*: de data die via dat protocol getransporteerd worden.

.. csv-table:: Protocol- en payload-formaten
  :header: "Protocol", "Adres", "Dataformaat", "Payload"
  :widths: 10, 10, 20, 20

  "HTTP", "URL-pad",   "HTTP-request/response", "HTML-document (etc.)"
  "TCP",  "TCP-poort", "TCP segment, met bytestroom data", "HTTP-request/response"
  "IP",   "IP-adres",  "IP-pakket", "TCP-segment"
  "WiFi Ethernet", "MAC-adres", "WiFi/Ethernet frame", "IP-pakket"

Omdat de payload van een IP-pakket voor verschillende protocollen gebruikt kan worden,
bevat een IP-pakket een veld om aan te geven wat het gebruikte payload-protocol is.
De ontvanger weet dan op welke manier deze payload geïnterpreteerd moet worden.
Een vergelijkbaar principe kom je op elk niveau in de protocolstack tegen.
