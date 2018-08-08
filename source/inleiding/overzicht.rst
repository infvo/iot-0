*********
Overzicht
*********

.. bij de inleiding; overzicht van het materiaal van deze module.

.. rubric:: Bouwstenen

Als eerste stap beschrijven we de bouwstenen van de internet of things.
Deze bouwstenen kunnen we op verschillende manieren combineren tot een IoT-keten:
van "ding" tot toepassing.

* Een IoT-knoop koppelt een fysiek "ding" aan de virtuele wereld.
* Het MQTT-protocol koppelt een IoT-knoop aan de toepassingen in het internet.
* We gebruiken een uniform JSON-formaat voor de verschillende soorten IoT-knopen:
  op het niveau van de toepassingen kun je deze dan gelijk behandelen.
* Met NodeRed kun je IoT-knopen, diensten en toepassingen op een grafische manier aan elkaar verbinden.
* Een IoT-knoop is vaak draadloos:
  er zijn verschillende soorten radio's om de verbinding met het internet te maken,
  afhankelijk van de eisen die het "ding" en de toepassing stellen.
*

.. rubric:: MQTT

MQTT is een veelgebruikt protocol voor het internet of things.
MQTT is een publish-subscribe-protocol: dit maakt het mogelijk om gegevens te *pushen* naar de bestemming.
(In de client-server interactie in het web, via het HTTP-protocol haalt de browser de gegevens van de server (*pull*).)

De oefeningen in dit deel zijn gericht op het leren werken met MQTT.
dit vormt de basis van de IoT-ketens die later aan de orde komen.)
Bij deze oefeningen gebruik je een paar eenvoudige hulpprogramma's die later ook van pas komen.

.. todo::

  <<JSON>>

.. rubric:: NodeRed

Met NodeRed kun je IoT-knopen, diensten en toepassingen op een grafische manier aan elkaar verbinden.

Dit onderdeel vormt een tutorial in het gebruik van NodeRed, gericht op het gebruik in deze module.
Dit tutorial behandelt een aantal NodeRed-flows voor het koppelen van IoT-knopen en toepassingen.
Een eenvoudige toepassing is een *dashboard* waarin je de sensorgegevens van één of meer IoT-knopen kunt volgen.

.. rubric: Webserver-knopen

Sommige IoT-knopen beschikken over een webserver.
Zo'n knoop je bewaken en besturen via een browser, via het HTTP-protocol.

.. rubric:: WiFi-knopen

.. todo::

  * MQTT; MQTT-broker (publiek - op afstand besturen; eventueel lokaal);
  * JSON (JSON-formaat voor sensorknopen)

.. rubric:: RFM69-knopen

We gebruiken de RFM69-radio als voorbeeld van *pakketcommunicatie*.
Deze radio gebruikt een vrije ISM-band (868MHz).
Voor het gebruik van deze radioband gelden enkele beperkingen.
Het bereik van deze radio is 20-200 meter, afhankelijk van het zendvermogen, de antenne en de bitrate.
Net als vergelijkbare radio's zoals ZigBee en Z-wave is deze radio goed te gebruiken voor toepassingen in en rond het huis,
zoals *domotica*.

Voor het aansluiten van het RFM69-netwerk op het internet gebruiken we een gateway.
Deze zorgt onder andere voor de protocol-conversie, van het binaire LPP-formaat naar het tekst-gebaseerde JSON-formaat.

.. rubric:: LoRa-knopen

De LoRa-radio heeft een bereik van enkele kilometers:
deze is dan ook vooral geschikt voor IoT-knopen die mobiel zijn in een groot gebied.
Dit grote bereik gaat wel ten koste van de bitrate:
een IoT-knoop kan maar een tiental keren per uur een klein bericht versturen.

In dit onderdeel gebruiken we het publieke netwerk van TheThingsNetwork (TTN).
Je koppelt IoT-knopen in een gegeven TTN-toepassing aan een eigen dashboard.
Met hardware-knopen met LoRaWan/TTN-software kun je een eigen TTN-toepassing maken.
