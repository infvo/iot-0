MQTT: publish-subscribe
=======================

MQTT is een zogenaamd publish-subscribe protocol.
De communicatie vindt plaats tussen clients, met tussenkomst van een broker (makelaar).
De broker heeft alleen als taak om berichten door te sturen:

* een client kan zich bij de broker abonneren (*subscribe*) op de berichten van een bepaald onderwerp (*topic*);
* een client kan een bericht van een bepaald onderwerp publiceren (*publish*).
  De broker stuurt dit bericht dan door naar alle clients zich op dit onderwerp geabonneerd hebben.
* een client kan zowel *publisher* als *subscriber* zijn.

Dit laatste betekent dat de broker op een willekeurig moment een bericht naar een client kan *pushen*:
deze client hoeft niet steeds bij de broker te vragen of er een nieuw bericht aangekomen is.

  *Dit is een verschil met het HTTP-protocol: in dat geval ligt het initiatief altijd bij de client,
  deze "trekt" (*pull*) de documenten van de server.*

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

Structuur van een topic
=======================

Een MQTT-topic bestaat uit een aantal strings gekoppeld door ``/``,
bijvoorbeeld ``abd``, ``abc/def``, ``abc/123/def``.
Dit lijkt op de padnaam in een URL.

Bij een *subscribe* kun je in de topic-string ook wildcards opnemen:
``+`` staat voor een willekeurige string zonder ``/``;
``#`` voor een willekeurige string waarin ook het koppelteken ``/`` mag voorkomen.
* voorbeeld: ``node/+/sensors`` matcht met ``node/12/sensors`` en ``node/432/sensors``.
* voorbeeld: ``node/#`` matcht met ``node/12/sensors`` en ``node/432/led``

JSON
====

.. sidebar:: JSON in het web

  Ook het web gebruikt het JSON-formaat, als onderdeel van AJAX.
  JavaScript-functies in een app communiceren vanuit de browser met de server,
  met (naar verhouding) kleine hoeveelheden data.
  Deze communicatie gebruikt het compacte en leesbare JSON-formaat, in plaats van complete HTML-documenten.

In dit hoofdstuk gebruiken we MQTT voor het bewaken en besturen van een IoT-knoop.
De communicatie tussen de IoT-knoop en de toepassing (app), via de MQTT-broker,
vindt plaats in de vorm van JSON-berichten.
Het JSON-formaat is voor het IoT wat HTML is voor het web.
We leggen hieronder eerst de principes van JSON uit.
In de opdracht gebruik je JSON voor het aansturen van de IoT-knoop.

JSON staat voor "JavaScript Object Notatie".
Een JSON-document is een tekstdocument: een leesbare vorm van een JavaScript-object.
Een object bestaat uit een aantal <code>"naam": waarde</code>-paren.
Een waarde kan een enkelvoudige waarde zijn, bijvoorbeeld een getal, een string, of een boolean.
Het kan ook een samengestelde waarde zijn: een object of een array.
In JavaScript gebruiken we de notatie: ``naam: waarde``.
In JSON staat de naam tussen dubbele quotes: ``"naam": waarde``.

We geven in de onderstaande voorbeelden de JavaScript-objecten en de bijbehorende JSON-notatie.

.. code-block:: javascript

  {temp: 21, press: 1015, id: "e4c7"}

.. code-block:: json

  {"temp":123,"press":1012, "id": "e4c7"}

.. todo::

  * meer JSON-voorbeelden

Je kunt een JSON-document eenvoudig omzetten in een JavaScript-object, en omgekeerd:

* ``obj = JSON.parse(str);``
* ``str = JSON.stringify(obj);``

Niet alle JavaScript-objecten kun je in JSON omzetten:

* objecten met functie-waarden
* objecten met onderlinge verwijzingen die een lus (cykel) vormen.

Veel programmeertalen hebben functies om JSON-objecten te verwerken.

* Python: https://docs.python.org/3/library/json.html
* Arduino: https://arduinojson.org/

Links
=====

* referentie: https://www.json.org/
* referentie: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
* tutorial: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON
