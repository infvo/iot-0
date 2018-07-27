**********
Opdrachten
**********

.. bij mqtt

.. admonition:: Wat heb je nodig?

  * MQTT broker in publieke internet;
      * de MQTT-broker fungeert ook als statische webserver, voor de app MQTT0
      * je moet de domeinnaam, het poortnummer, en eventueel een gebruikersnaam/wachtwoord kennen.
  * IoT knoop met software: MQTT-0; verbonden in het lokale WiFi-netwerk; en verbonden met de MQTT broker;
      * je moet de ID van de knoop kennen; dit bestaat uit de laatste 4 cijfers van het MAC-adres;
      * dit kan eventueel een gesimuleerde IoT-knoop zijn.


.. rubric:: De app MQTT1

Bij veel opdrachten gebruiken we de app MQTT1 (MQTT0):
dit is een mqtt-client waarmee we mqtt-berichten kunnen sturen en ontvangen.
Door de aard van het mqtt-protocol kunnen we daarmee ook *topics* "afluisteren".

.. figure:: IoT-mqtt0-app.png
   :width: 400 px
   :align: center

   MQTT test app

De  web-app kun je vinden via de broker: http://infvopedia.nl:1884/mqtt1.html.
Hierin is ``infvopedia.nl`` de domeinnaam van de broker.
``1884`` is het HTTP-poortnummer van de broker: de broker fungeert ook als (statische) webserver.
Het MQTT-protocol gebruikt poort ``1883``.

De app heeft de volgende invoervelden en knoppen:

* invoer ``IoT-node``: de ID van de IoT-knoop die je wilt aansturen, en waarvan je de sensordata wilt ontvangen
    * de sensordata worden ontvangen via topic ``node/ID/sensors``
* knoppen On en Off: sturen bericht ``{"led0":1}`` respectievelijk ``{"led0":1}`` met ``topic node/ID/actuators``
* invoer ``subscribe to topic``: hier geef je het topic op waarvan je de berichten wilt ontvangen
    * in het venster daaronder verschijnen de ontvangen berichten met hun topic
    * in het topic kun je ook wildcard-tekens opnemen, zoals ``+`` en ``#``.
* invoer ``publish to topic`` - om een bericht met een topic te versturen;
    * het bericht zelf voer je daaronder in, en
    * met de "publish"-knop verstuur je het bericht.

.. rubric:: Alternatief: MQTT-box

Er zijn ook desktop-applicaties waarmee je deze opdrachten kunt uitvoeren.
Een voorbeeld is MQTTbox (http://workswithweb.com/mqttbox.html).

(1) Chatten
-----------

**(stap 1)** Je kunt met MQTT op een eenvoudige manier met een groep gebruikers chatten,
via de MQTT1-app in de browsers van deze gebruikers.

1. Spreek af welk *topic* je gebruikt voor de communicatie, bijvoorbeeld ``chat``
2. Stel dit topic in bij de beide topic-vensters: voor het ontvangen (*subscribe*)
   en voor het verzenden (*publish*) van de berichten.
3. Als je nu in de ene browser in de app een bericht invoert en publiceert,
   zie je dit in de andere browsers in de app verschijnen.

**(stap 2)** Een nadeel van deze aanpak is dat je niet kunt zien van wie een bericht afkomstig is.
Dit kun je oplossen door de naam van de afzender in het topic op te nemen:

4. vul als **subscribe-topic** een wildcard-topic in, bijvoorbeeld ``chat/+`` .
   Deze ``+`` matcht met een willekeurige string.
5. vul als **publish-topic** een topic in met daarin de naam van de afzender,
   bijvoorbeeld ``chat/frans`` .
6. je ziet dan bij de ontvangen berichten steeds de naam van de afzender in het topic.

Je moet er in dit geval wel op vertrouwen dat iedereen zijn eigen naam invult:
je kunt de afzender in het topic niet authenticeren.

(2): Volgen van een knoop
-------------------------

In deze opdracht gebruik je MQTT1 om het verkeer van een bestaande IoT-knoop te volgen,
en om de sensordata netjes weer te geven.
Voor de communicatie tussen MQTT1 en de IoT-knoop met ID ``xxxx`` gebruiken we de volgende *topics*:

* ``node/xxxx/sensors`` - voor de sensorwaarden
* ``node/xxxx/actuators`` - voor de actuatorwaarden.

Met het wildcard-topic ``node/+/+`` ontvang je alle sensor/actuatorwaarden van alle knopen.
Er is geen standaard die de vorm van deze topics afdwingt: het is onze eigen, praktische keuze.
Deze keuze gebruiken we in onze IoT-knopen en in onze toepassingen (apps).

(stap 1) Eerst moeten we de ID van een bestaande knoop zien te vinden.
Dit doen we door voor het subscribe-veld een *wildcard*-topic in te vullen.

1. vul als subscribe-topic in: ``node/+/sensors``;
2. je krijgt nu de sensor-berichten te zien van alle knopen die deze broker gebruiken.
3. kies hiervan een knoop die je regelmatig langs ziet komen, en bepaal de ID xxxx (uit ``node/xxxx/sensors``).

(stap 2) We willen de sensordata weergeven in het bovenste deel van MQTT1.

4. vul bij ``IoT-node`` de ID in van je gekozen knoop.
5. vul als subscribe-topic in: ``node/xxxx/+`` (waarin ``xxxx`` staat voor de gekozen ID).
   je ziet dan alle sensor- en actuator-berichten van deze IoT-knoop verschijnen.
6. na enige tijd verschijnen de waarden van de sensoren in tabelvorm;
   in het subscribe-venster zie je deze in JSON-vorm.

Om over na te denken:

* hoe weet je wat de eigenschap (het veld) ``"temp"`` voorstelt?

(3) Aansturen van een knoop
---------------------------

Met MQTT1 kunnen we de LEDs van de knoop ``xxxx`` ook aansturen.

**(stap 1)** Aansturen van ``led0``:

1. vul als ``subscribe to topic`` in: ``node/xxxx/+`` (waarin ``xxxx`` staat voor de gekozen ID);
2. door het indrukken van de knoppen in MQTT1 kun je ``led0`` aan- en uitschakelen;
3. controleer bij de sensorwaarden of deze led inderdaad uit- en uitgaat.
4. ga in het subscribe-venster na welke berichten langskomen als je een knop indrukt,
   voor de actuatoren (leds) en voor de sensoren.
   (a) Welk bericht stuurt de knop On? met welk topic?
   (b) En welk bericht stuurt de knop Off?

Als de (hardware) IoT-knoop ``xxxx`` in je buurt is, kun je controleren of de LED aan- en uitgaat.

**(stap 2)** Je kunt ``led1`` "met de hand" aansturen.
Door de juiste JSON-berichten te sturen naar het actuator-topic van je knoop,
kun je ook de andere led besturen.

5. vul als ``publish to topic`` in: ``node/xxxx/actuators`` (waarin ``xxxx`` staat voor de gekozen ID)
6. vul in het ``publish``-veld in: het bericht dat je gevonden hebt onder (4a) hierboven,
   aangepast aan ``led1``.
7. klik op "Publish"
8. controleer de berichten die langskomen in het subscribe-venster,
   en de status van ``led1`` in de sensortabel.
9. schakel beide leds uit (of aan) met één enkel JSON-bericht.

(4) Knoop-simulator
-------------------


(5) Gebruik van JSON in het Internet of Things
----------------------------------------------

.. todo::

    nog aanvullen

Het JSON-formaat wordt in het Internet of Things op meerdere plaatsen gebruikt.
Bestudeer de volgende manieren van gebruik:

* importeren en exporteren van NodeRed-flows
* berichten van TTN gateways (via MQTT)
    * welke informatie voegt een TTN gateway toe aan de "payload" van een IoT-knoop?
