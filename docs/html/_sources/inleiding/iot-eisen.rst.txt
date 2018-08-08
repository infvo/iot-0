*****
Eisen
*****

.. bij de Inleiding

.. todo::

  * eisen aan de communicatie
  * eisen aan de security (en privacy)

.. rubric:: Het Internet of Things stelt eigen eisen aan de internet-communicatie

Het Internet of Things stelt andere eisen aan de communicatie dan het web.
Dit betekent dat je andere soorten verbindingen (radio's) en protocollen nodig hebt.

.. rubric:: Eisen aan de communicatie

* bandbreedte (bitrate) is vaak beperkt (behalve als er beeld en geluid bij betrokken zijn)
    * per sensor (meting) weinig data
    * per tijdseenheid weinig metingen
* latency soms wel belangrijk - in het bijzonder bij real-time besturing van "dingen".
* veiligheid is essentieel - zowel vanwege de veiligheid van de "dingen"
  als van de privacy van mensen in hun omgeving.
    * vooral bij besturingstoepassingen is dit van belang
    * (opmerking: het IoT kan ook bijdragen aan de fysieke veiligheid...)
* aantallen IoT-knopen kan erg groot zijn - veel groter dan het aantal mensen dat een server gebruikt.
    * een IoT-toepassing (server) moet dan grote hoeveelheden kleine berichten kunnen verwerken.

.. rubric:: Verschillen tussen IoT en het web

Het IoT is veel heterogener dan het web, en veel minder gestandaardiseerd.
Dit betekent dat we veel verschillende oplossingen en vormen van de IoT-keten tegenkomen.

Deze IoT-keten heeft andere karakteristieken dan de keten voor het web:

* de sensordata van een enkele IoT-knoop bestaan uit weinig bytes - veel minder dan de gemiddelde webpagina, en nog veel minder dan nodig voor audio en video;
* het aantal IoT-knopen in een toepassing kun erg groot zijn - veel groter dan bijvoorbeeld het aantal gebruikers van een website;
* de fysieke wereld stelt soms absolute grenzen aan de latency; in de virtuele wereld is deze latency wat minder van belang;
* de resultaten van het web worden aan mensen gepresenteerd, die op basis daarvan beslissingen nemen;
* de resultaten van het IoT worden soms gebruikt om direct "dingen" te besturen, zonder menselijke tussenkomst (M2M, machine to machine).
* de verschillen tussen "dingen" zijn veel groter dan de verschillen tussen mensen: het IoT is veel heterogener dan het web.
