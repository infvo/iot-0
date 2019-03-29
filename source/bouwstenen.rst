**********
Bouwstenen
**********

In dit hoofdstuk geven we een overzicht van de bouwstenen van het Internet of Things.
We geven verschillende voorbeelden van de manier waarop deze bouwstenen tot een IoT-keten gecombineerd worden.
In de volgende hoofdstukken werken we deze bouwstenen en IoT-ketens verder uit.

.. note::

  Dit hoofdstuk is bedoeld als een inleidend overzicht:
  het is niet nodig de begrippen die we hier invoeren in detail te begrijpen.
  In de volgende hoofdstukken werken we deze bouwstenen en IoT-ketens verder uit.
  Je kunt dit hoofdstuk later gebruiken om deze details in het geheel te kunnen plaatsen.

.. rubric:: IoT-knoop: koppelen van de fysieke wereld aan de virtuele wereld

.. figure:: bouwstenen/IoT-knoop-0.png
  :width: 400px
  :align: center

  IoT-knoop

Het Internet of Things koppelt de fysieke wereld van de "dingen" aan de virtuele wereld van het internet.
De koppeling van een "ding" aan het internet noemen we een *IoT-knoop*.
Deze gebruikt  *sensoren en actuatoren* om het "ding" te bewaken en te besturen.
Een *microcontroller*, een complete computer op een chip, bestuurt deze sensoren en actuatoren.
De IoT-knoop heeft *communicatie* nodig om deze microcontroller met sensoren en actuatoren in het internet te verbinden.
Mobiele IoT-knopen gebruiken een *radio* voor communicatie *draadloze communicatie*.
Voor mobiele knopen moet ook de *energievoorziening* (elektriciteit) draadloos zijn:
je hebt dan energiezuinige elektronica nodig, met een batterij en soms met lokale energieopwekking ("energy harvesting" of "scavenging", zoals zonnecellen).
Tenslotte zijn de fysieke afmetingen en het gewicht van de IoT-knoop belang:
hoe kleiner en lichter (en goedkoper) een IoT-knoop, voor des te meer "dingen" kun je deze gebruiken.

.. rubric:: De IoT-keten: van sensoren en actuatoren tot beslissingen

.. figure:: bouwstenen/IoT-keten-RFM69.png
  :width: 500px
  :align: center

  Voorbeeld van een IoT-keten

Een IoT-toepassing omvat een keten van onderdelen, van sensoren en actuatoren tot besluitvorming,
mogelijk ondersteund door Data Science en Artificial Intelligence.
In deze keten komen we meestal de volgende onderdelen tegen:

* *IoT-knopen*, met sensoren en actuatoren verbonden aan de fysieke wereld;
* *gateways*, om deze IoT-knopen aan het internet te verbinden;
* *servers en brokers* in het publieke internet, als publiek toegangspunt voor de data van de IoT-knopen;
* *diensten* op het gebied van Data Science, Artificial Intelligence, visualisatie e.d.;
* *controllers*, bijvoorbeeld op basis van NodeRed, voor het besturen van de IoT-knopen, in combinatie met verschillende diensten en toepassingen;
* *protocollen* voor datatransporten en voor interactie tussen de verschillende onderdelen.

De IoT-knopen vormen de buitenkant ("edge") van het Internet of Things.
Eén van de ontwerpvragen bij een Internet of Things-toepassing is hoe het rekenwerk verdeeld moet worden over de "edge" en de kern (servers) van het IoT.

.. toctree::
  :hidden:

  bouwstenen/iot-bouwstenen.rst
  bouwstenen/iot-ketens.rst
  bouwstenen/iot-protocollen.rst
  bouwstenen/opdrachten.rst
  bouwstenen/nodered-opdrachten.rst
  bouwstenen/toetsvragen.rst
