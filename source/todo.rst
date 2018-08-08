****
ToDo
****

.. todo::

  MQTT1:

  * naamgeving?
  * aanpassen van HTML-title
  * toevoegen van een "clear"-button voor subscribe-venster
  * publish-to topic: actuators ipv. sensors
  * geen automatische clear van publish-venster (evt. ook clear-button).
  * publish-venster kan kleiner.
  * kunnen we subscribe-venster automatisch laten scrollen?

.. todo::

  Knoop-simulator:

  * check met welke frequentie de knoop-simulator de sensorgegevens verstuurt.
  * worden deze direct verstuurd bij het veranderen van een sensor?
      * dat is niet de bedoeling: alleen bij het ontvangen van een actuator-bericht,
        of bij het indrukken van een knop moeten de sensorwaarden verstuurd worden.
  * meesturen van een tijd of een volgnummer
  * toevoegen van lichtniveau aan simulator


.. todo::

  * duidelijk(er) maken dat mqtt niet voorkomt dat verschillende knopen onder dezelfde ID/hetzelfde topic publiceren.
    (er is een mqtt-client-id, maar hoe kun je die achterhalen, in de verschillende contexten?)

.. todo::

  onderstaande tekst verhuizen

De technologische voortgang op elk van deze onderdelen maakt nu kleine, lichte en (relatief) goedkope IoT-knopen mogelijk.
Deze ontwikkeling zal de komende jaren nog in snel tempo voortgaan, geholpen door de grote massa van het Internet of Things.
Daarnaast is ook de verwerking van IoT-data sterk verbeterd.
De ontwikkelingen op het gebied van Data Science en Artificial Intelligence maken nieuwe IoT-toepassingen mogelijk.
Deze ontwikkelingen versterken elkaar en zorgen samen voor een stroomversnelling in de technologie en in de toepassingen.

.. admonition:: Een andere versie van Moore's Law

  De digitale elektronica maakt al decennia een exponentiële ontwikkeling door, volgens "Moore's Law".
  De exponentieel stijgende vraag uit de markt, bijvoorbeeld naar smartphones, ondersteunt deze technologische ontwikkeling.

  De traditionele versie van Moore's Law gaat vooral om "meer": meer rekenkracht en meer geheugen.
  Bij het Internet of Things gaat het vooral om "minder": minder energie, minder gewicht, en kleiner.
  Ook in dit geval is een exponentiële ontwikkeling te verwachten.
  Het grootste probleem lijkt daarbij de energievoorziening te zijn:
  vooral de batterij bepaalt de omvang en het gewicht van een IoT-knoop.

.. todo::

  Onderstaande tekst verhuizen (verschillen IoT en het web)

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


.. todolist::
