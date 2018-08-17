****************
Webserver-knopen
****************

In dit hoofdstuk behandelen we IoT-knopen die als webserver dienen.

.. admonition:: Leerdoelen en concepten

  * client-server interactie; request en response
  * lokaal netwerk; adressering (IP-adres, domeinnaam)
  * HTTP protocol
  * adressering: URL
  * payload: HTML formaat

Sommige apparaten beschikken over een webserver waarmee je het apparaat kunt bedienen.
Voorbeelden hiervan zijn een (thuis)router en een netwerkprinter.
Via de browser van een computer of smartphone bedien je het apparaat op afstand.

.. figure:: webserver/IoT-webserver-interface-0.png
   :width: 500 px
   :align: center

   Webserver-interface van een netwerkprinter

In de figuur zie je het interface van een netwerkprinter, als webpagina.
Via de browser kun je de toestand van de printer bekijken en de instellingen aanpassen.
De webserver biedt een uitgebreid gebruikersinterface.
Het apparaat zelf kan dan met een eenvoudig interface volstaan,
met een paar knoppen en eventueel een klein schermpje.

.. figure:: webserver/IoT-client-server-0.png
   :width: 500 px
   :align: center

   Client-server interactie

.. index::
   single: client-server interactie; request; response

De ingebouwde webserver gebruikt de web- en internetprotocollen.
De interactie tussen de browser en deze webserver is een *client-server interactie*, via het HTTP-protocol.
De client (browser) stuurt een HTTP request (bijv. GET) met een bepaald URL-pad naar de webserver;
de webserver stuurt als response een HTML-document terug, dat de browser vervolgens weergeeft voor de gebruiker.
Dit HTML-document is in dit geval het gebruikersinterface van het apparaat.
De client-server interactie via HTTP is een "pull" interactie: het initiatief voor de interactie ligt bij de browser.

.. figure:: webserver/IoT-webserver-1.png
   :width: 300 px
   :align: center

   IoT-knopen met webserver

Enkele eigenschappen van deze aanpak:

* het apparaat is verbonden in het *lokale netwerk*: draadloos (via WiFi) of bedraad (Ethernet);
* het apparaat (de IoT-knoop) moet *permanent verbonden* zijn in dit netwerk;
  voor batterij-apparaten is dit is een probleem;
* het apparaat is alleen binnen het lokale netwerk te bedienen;
* je moet het IP-adres of de lokale domeinnaam (*zeroconfig*) van het apparaat kennen
* de client (browser of een andere computer) moet de gegevens actief van de webserver halen ("pull").

.. toctree::
   :maxdepth: 2

   webserver/webserver-keten.rst
   webserver/webserver-protocol.rst 
   webserver/opdrachten.rst
   webserver/toetsvragen.rst
