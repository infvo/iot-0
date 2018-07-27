**********
Bouwstenen
**********

.. todo::

  * IoT-knopen
  * protocol: MQTT (ipv. HTTP)
  * ketens: gateways;  PubSub (ipv. client-server)
  * NodeRed (als "lijm")

Wat zijn de belangrijkste bouwstenen van het Internet of Things (IoT)?
Waardoor maakt het IoT nu zo'n grote sterke ontwikkeling door?

.. rubric:: IoT-knoop: koppelen van de fysieke wereld aan de virtuele wereld

Het Internet of Things koppelt de fysieke wereld van de "dingen" aan de virtuele wereld van het internet.
De koppeling van een "ding" aan het internet noemen we een *IoT-knoop*.
Deze gebruikt  *sensoren en actuatoren* om het "ding" te monitoren (bewaken) en te besturen.
Een *microcontroller*, een complete computer op een chip, bestuurt deze sensoren en actuatoren .
De IoT-knoop heeft *communicatie* nodig om deze microcontroller met sensoren en actuatoren in het internet te verbinden.
Voor mobiele "dingen" moet deze communicatie *draadloos* zijn.
Ook voor andere "dingen" heeft draadloze communicatie voordelen.
Voor mobiele "dingen" moet ook de *energievoorziening* (elektriciteit) draadloos zijn:
je hebt dan energiezuinige elektronica nodig, met een batterij en soms met lokale energieopwekking ("energy harvesting" of "scavenging", zoals zonnecellen).
Tenslotte zijn de fysieke afmetingen en het gewicht van de IoT-knoop belang:
hoe kleiner en lichter (en goedkoper) een IoT-knoop, voor des te meer "dingen" kun je deze gebruiken.

.. rubric:: De IoT-keten: van sensoren en actuatoren tot beslissingen

Een IoT-toepassing omvat een keten van onderdelen, van sensoren en actuatoren tot besluitvorming, mogelijk ondersteund door Data Science en Artificial Intelligence.
In deze keten komen we meestal de volgende onderdelen tegen:

* *IoT-knopen*, met sensoren en actuatoren verbonden aan de fysieke wereld;
* *gateways*, om deze IoT-knopen aan het internet te verbinden;
* *servers en brokers* in het publieke internet, als publiek toegangspunt voor de data van de IoT-knopen;
* *diensten* op het gebied van Data Science, Artificial Intelligence, visualisatie e.d.;
* *schakelpunten*, om verschillende diensten, data en protocollen te combineren;
* *protocollen* voor datatransporten en voor interactie tussen de verschillende onderdelen.

De IoT-knopen vormen de buitenkant ("edge") van het Internet of Things.
Eén van de ontwerpvragen bij het Internet of Things is hoe het rekenwerk verdeeld moet worden over de "edge" en de kern (servers) van het IoT.

Het IoT is veel heterogener dan het web, en veel minder gestandaardiseerd.
Dit betekent dat we veel verschillende oplossingen en vormen van de IoT-keten tegenkomen.

Deze IoT-keten heeft andere karakteristieken dan de keten voor het web:

* de sensordata van een enkele IoT-knoop bestaan uit weinig bytes - veel minder dan de gemiddelde webpagina, en nog veel minder dan nodig voor audio en video;
* het aantal IoT-knopen in een toepassing kun erg groot zijn - veel groter dan bijvoorbeeld het aantal gebruikers van een website;
* de fysieke wereld stelt soms absolute grenzen aan de latency; in de virtuele wereld is deze latency wat minder van belang;
* de resultaten van het web worden aan mensen gepresenteerd, die op basis daarvan beslissingen nemen;
* de resultaten van het IoT worden soms gebruikt om direct "dingen" te besturen, zonder menselijke tussenkomst (M2M, machine to machine).
* de verschillen tussen "dingen" zijn veel groter dan de verschillen tussen mensen: het IoT is veel heterogener dan het web.

.. toctree::
  :hidden:

  bouwstenen/iot-bouwstenen.rst
  bouwstenen/iot-ketens.rst
  bouwstenen/opdrachten
  bouwstenen/toetsvragen
