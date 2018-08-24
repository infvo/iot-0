Webserver-keten
===============

.. figure:: IoT-webserver-1.png
   :width: 300 px
   :align: center

   IoT-knoop als webserver

In de webserver-keten hebben we te maken met de volgende spelers (*agents*):

* *webserver*: de IoT-knoop, als onderdeel van een apparaat.
  Deze IoT-knoop is verbonden in het lokale netwerk, draadloos (WiFi) of bedraad (Ethernet).
  De webserver levert het HTML-document (de "web app") waarmee je via de browser het apparaat kunt besturen.

* *webclient*: een browser op een computer of smartphone,
  draadloos (WiFi) of bedraad (Ethernet)verbonden in het lokale netwerk.

Een derde speler is de lokale WiFi-router: deze zorgt voor de fysieke verbinding tussen deze spelers.
Deze heeft verder geen invloed op de interactie tussen de webclient en de webserver.

De webserver is in het lokale netwerk verbonden, en daardoor (meestal) niet toegankelijk via het publieke internet.
Dit betekent dat je het apparaat alleen via smartphones en computers in het lokale netwerk kunt bedienen.

.. topic:: Webservers: van klein tot zeer groot

  Zoals je in de voorbeelden in deze module ziet, kan een webserver heel klein zijn.
  Deze kleine webservers gebruiken dezelfde protocollen en formaten als de webservers die bijvoorbeeld voor Wikipedia gebruikt worden:
  de servers voor dergelijke grote websites staan opgesteld in *server farms*, vaak in de buurt van een energiecentrale.

Client-server interactie
------------------------

.. figure:: IoT-client-server-0.png
   :width: 500 px
   :align: center

   Client-server interactie

De *interactie* tussen de webclient en webserver is een *client-server interactie*.
Deze interactie verloopt via het HTTP-protocol (Hypertext transfer protocol).
de webclient stuurt een HTTP-verzoek naar de webserver; de webserver stuurt een HTTP-response terug,
met daarin een HTML-document.
Anders gezegd: de webclient "pull"t het HTML-document van de webserver.

Het initiatief voor deze client-server interactie ligt bij de browser (client).
De webserver stuurt niet op eigen initiatief gegevens naar de browser ("push").
Deze HTTP-aanpak is geschikt voor het besturen van een apparaat (actuatoren).
Voor monitoring van sensoren is deze aanpak minder geschikt.

.. topic:: Client-server in het internet

  Andere voorbeelden:

  * mail: server voor versturen (SMTP) en server voor ontvangen (POP, IMAP) van mail
  * time server
  * DNS-server
  * (web)server kan als client optreden van andere server (->NodeRed voorbeeld)
  * in al deze voorbeelden: initiatief bij client;
    client hoeft alleen tijdens interactie verbonden te zijn aan het internet;
    server moet altijd online zijn.
