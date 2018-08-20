Protocol: MQTT
==============

MQTT is een publish-subscribe-protocol dat je in allerlei omgevingen kunt gebruiken.
In het bijzonder is MQTT geschikt voor het Internet of Things.
In die context zijn zowel IoT-knopen als IoT-toepassingen (apps) clients van de MQTT-broker.
De interactie is in principe symmetrisch: zowel een IoT-knoop als een IoT-toepassing kan een bericht publiceren naar de broker.

Een typisch gebruik van MQTT voor IoT-toepassingen:

* een IoT-knoop publiceert zijn sensordata naar de broker
    * als de sensordata veranderd zijn, of met een vaste regelmaat;
    * met een topic dat gekoppeld is aan de identiteit van de IoT-knoop;
* een IoT-knoop abonneert zich op de actuatordata voor de eigen actuatoren
    * met een topic dat gekoppeld is aan de identiteit van de knoop;
* een IoT-app abonneert zich op de sensordata van de relevante knopen;
* een IoT-app stuurt een actuator in een IoT-knoop aan door een "publish"
    * van een bericht met het actuator-topic van de IoT-knoop.

.. rubric:: Voorbeeld

.. figure:: MQTT-IoT.png
   :width: 500 px
   :align: center

   MQTT in het Internet of Things

Uitleg:

* IoT-knoop A publiceert zijn sensordata onder topic ``A/sensors``
* IoT-knoop A (na "subscribe(A/led)") ontvangt de berichten voor het aansturen van de eigen LED
* analoog voor IoT-knoop B
* toepassing (app) C ontvangt (na "subscribe(+/sensors)") de sensordata van A en van B
    * "+" is hierin een wildcard-teken: dit past op alle strings (node-id's)
* toepassing C stuurt de LED van de node A aan.
* analoog voor app D
    * deze communiceert alleen met node B.
* de broker stuurt de sensordata door naar de apps, en de led-aansturing naar de IoT-knopen.

Protocolstack
-------------

* MQTT op basis van TCP
* alternatief: MQTT op basis van websockets (op basis van TCP)


Adressering: topic
------------------

Door middel van een *topic* adresseert een MQTT-client de berichtenstroom voor een publish- of subscribe-actie.
Een MQTT-topic bestaat uit een aantal strings gekoppeld door ``/``,
bijvoorbeeld ``abd``, ``abc/def``, ``abc/123/def``.
Dit lijkt op de padnaam in een URL.

Bij een *subscribe* kun je in de topic-string ook wildcards opnemen:
``+`` staat voor een willekeurige string zonder ``/``;
``#`` voor een willekeurige string waarin ook het koppelteken ``/`` mag voorkomen.

* voorbeeld: ``node/+/sensors`` matcht met ``node/12/sensors`` en ``node/432/sensors``.
* voorbeeld: ``node/#`` matcht met ``node/12/sensors`` en ``node/432/led``

.. rubric Gebruik van topics in onze voorbeelden

We gebruiken in de software bij deze module een aantal vaste afspraken voor MQTT-topics.
Hierdoor kunnen we de verschillende soorten IoT-knopen combineren met de verschillende toepassingen.

* voor sensoren: ``node/<nodeid>/sensors``
* voor actuatoren: ``node/<nodeid>/actuators``

Hierin is ``<nodeid>`` de identificatie (string) van de IoT-knoop.

Payload: JSON
-------------

Er is geen vast voorgeschreven payload-formaat voor MQTT.
In onze voorbeelden gebruiken we JSON (JavaScript Object Notation) als MQTT-payload:
dit is een tekst-gebaseerd formaat voor objecten.
