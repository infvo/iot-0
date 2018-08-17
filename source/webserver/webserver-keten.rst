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

.. figure:: IoT-client-server-0.png
   :width: 500 px
   :align: center

   Client-server interactie

De *interactie* tussen de webclient en webserver is een *client-server interactie*.
Deze interactie verloopt via het HTTP-protocol (Hypertext transfer protocol).
de webclient stuurt een HTTP-verzoek naar de webserver; de webserver stuurt een HTTP-response terug,
met daarin een HTML-document.

.. topic:: Webservers: van klein tot zeer groot

  Zoals je in de voorbeelden in deze module ziet, kan een webserver heel klein zijn.
  Deze kleine webservers gebruiken dezelfde protocollen en formaten als de webservers die bijvoorbeeld voor Wikipedia gebruikt worden:
  de servers voor dergelijke grote websites staan opgesteld in *server farms*, vaak in de buurt van een energiecentrale.
