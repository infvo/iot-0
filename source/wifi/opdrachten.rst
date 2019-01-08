MQTT-opdrachten
===============

.. bij wifi-mqtt-knopen

.. topic:: Wat heb je nodig?

  * MQTT broker in publieke internet;
      * bijvoorbeeld: ``http://infvopedia.nl:1883``;
      * de MQTT-broker fungeert ook als statische webserver,
        voor de app MQTTT en voor de knoop-simulator;
  * de app MQTTT (zie :ref:`MQTTT`)
  * zo mogelijk: IoT knoop (hardware),
      * met software: ``mqtt-node-0``;
      * verbonden met de MQTT broker, via het lokale WiFi-netwerk;
      * daarvoor moet je van deze broker de domeinnaam, het poortnummer,
        en een gebruikersnaam/wachtwoord kennen;
        vraag dit eventueel aan je docent.
      * de ID van de knoop bestaat uit de laatste 4 cijfers van het MAC-adres;
        deze ID staat ook op de knoop zelf.

(1) MQTT-chat
-------------

**(stap 1)** Je kunt met MQTT op een eenvoudige manier met een groep gebruikers chatten,
via de MQTTT-app in de browsers van deze gebruikers.

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

(2): Knoop volgen
-----------------

In deze opdracht gebruik je MQTTT om het verkeer van een bestaande IoT-knoop te volgen,
en om de sensordata netjes weer te geven.
Voor de communicatie tussen MQTTT en de IoT-knoop met ID ``xxxx`` gebruiken we de volgende *topics*:

* ``node/xxxx/sensors`` - voor de sensorwaarden
* ``node/xxxx/actuators`` - voor de actuatorwaarden.

Met het wildcard-topic ``node/+/+`` ontvang je alle sensor/actuatorwaarden van alle knopen.
Er is geen standaard die de vorm van deze topics afdwingt: het is een eigen, praktische keuze.
Deze keuze gebruiken we in de IoT-knopen en in de toepassingen (apps).

(**stap 1**) Eerst moeten we de ID van een bestaande knoop zien te vinden.
Dit doen we door voor het subscribe-veld een *wildcard*-topic in te vullen.

1. vul als subscribe-topic in: ``node/+/sensors``;
2. je krijgt nu de sensor-berichten te zien van alle knopen die deze broker gebruiken.
3. kies hiervan een knoop die je regelmatig langs ziet komen, en bepaal de ID xxxx (uit ``node/xxxx/sensors``).

Als je een hardware-IoT-knoop hebt, dan staat de ID waarschijnlijk op de knoop zelf.
De node-ID bestaat uit de laatste 4 tekens van het MAC-adres.
Controleer of je knoop actief is: er moeten dan regelmatig JSON-berichten langskomen in het subscribe-venster
(bijvoorbeeld elke 1-2 minuten).

(**stap 2**) We willen de sensordata weergeven in het bovenste deel van MQTTT.

4. vul bij ``IoT-node`` de ID in van je gekozen knoop.
5. vul als subscribe-topic in: ``node/xxxx/+`` (waarin ``xxxx`` staat voor de gekozen ID).
   je ziet dan alle sensor- en actuator-berichten van deze IoT-knoop verschijnen.
6. na enige tijd verschijnen de waarden van de sensoren in tabelvorm;
   in het subscribe-venster zie je deze in JSON-vorm.
7. Verduister de knoop (bijvoorbeeld met een doekje).
   Controleer dat het volgende sensor-bericht van de knoop een andere waarde voor de analoge input (LDR) geeft.

Om over na te denken:

* hoe weet je wat de eigenschap (het veld) ``"temperature"`` voorstelt?

(3) Knoop besturen
------------------

Met MQTTT kunnen we de LEDs van de knoop ``xxxx`` ook aansturen.

**(stap 1)** Aansturen van ``led0``:

1. vul als ``subscribe to topic`` in: ``node/xxxx/+`` (waarin ``xxxx`` staat voor de gekozen ID);
2. door het indrukken van de knoppen in MQTTT kun je ``led0`` aan- en uitschakelen;
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


(4) JSON in het IoT
-------------------

.. todo::

    nog aanvullen

Het JSON-formaat wordt in het Internet of Things op meerdere plaatsen gebruikt.
Bestudeer de volgende manieren van gebruik:

* importeren en exporteren van NodeRed-flows
* berichten van TTN gateways (via MQTT)
    * welke informatie voegt een TTN gateway toe aan de "payload" van een IoT-knoop?
