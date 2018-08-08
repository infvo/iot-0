***********************
MQTT: publish-subscribe
***********************

MQTT is een zogenaamd publish-subscribe protocol.
De communicatie vindt plaats tussen clients, met tussenkomst van een broker (makelaar).
De broker heeft alleen als taak om berichten door te sturen:

* een client kan zich bij de broker abonneren (*subscribe*) op de berichten van een bepaald onderwerp (*topic*);
* een client kan een bericht van een bepaald onderwerp publiceren (*publish*).
  De broker stuurt dit bericht dan door naar alle clients zich op dit onderwerp geabonneerd hebben.
* een client kan zowel *publisher* als *subscriber* zijn.

Dit laatste betekent dat de broker op een willekeurig moment een bericht naar een client kan *pushen*:
deze client hoeft niet steeds bij de broker te vragen of er een nieuw bericht aangekomen is.

  Dit is een verschil met het HTTP-protocol: in dat geval ligt het initiatief altijd bij de client,
  deze "trekt" (*pull*) de documenten van de server.

  .. figure:: MQTT.png
     :width: 600 px
     :align: center

     MQTT voorbeeld

In de figuur publiceert client A bericht M met topic T.
De broker stuurt dit bericht door (*push*) naar clients B en D,
die zich eerder met "subscribe(T)" op topic T geabonneerd hebben.
Client C heeft zich niet geabonneerd op topic T, en ontvangt bericht M dus niet.

Op een volgend moment kan client B een bericht publiceren met een topic waar bijvoorbeeld A, C en D op geabonneerd zijn.
Client B is dan zowel *publisher* als *subscriber*.

Deze publish/subscribe-aanpak heeft de volgende eigenschappen:

* *push* van berichten van de client naar de broker, en van de broker naar de clients.
* *losse koppeling* tussen de clients:
  de clients communiceren via een topic, en weten niet welke clients aan dat topic gekoppeld zijn.
  In het bijzonder weet een publisher niet welke clients op het topic geabonneerd zijn.


MQTT in het IoT
===============

In de context van het Internet of Things zijn zowel IoT-knopen als IoT-apps clients van de MQTT-broker.

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
   :width: 600 px
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

MQTT-topics
===========

Een MQTT-topic bestaat uit een aantal strings gekoppeld door ``/``,
bijvoorbeeld ``abd``, ``abc/def``, ``abc/123/def``.
Dit lijkt op de padnaam in een URL.

Bij een *subscribe* kun je in de topic-string ook wildcards opnemen:
``+`` staat voor een willekeurige string zonder ``/``;
``#`` voor een willekeurige string waarin ook het koppelteken ``/`` mag voorkomen.

* voorbeeld: ``node/+/sensors`` matcht met ``node/12/sensors`` en ``node/432/sensors``.
* voorbeeld: ``node/#`` matcht met ``node/12/sensors`` en ``node/432/led``

MQTT voor IoT-knopen
====================

We gebruiken in de software bij deze module een aantal vaste afspraken voor MQTT-topics
en voor MQTT-berichten (zie JSON, verderop).
Hierdoor kunnen we de verschillende soorten IoT-knopen combineren met de verschillende toepassingen.

* voor sensoren: ``node/<nodeid>/sensors``
* voor actuatoren: ``node/<nodeid>/actuators``

Hierin is ``<nodeid>`` de identificatie (string) van de IoT-knoop.
