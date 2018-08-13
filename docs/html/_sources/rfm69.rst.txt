************
RFM69-knopen
************

.. admonition:: Concepten en leerdoelen

  * ISM-band 868 MHz
  * pakketradio
  * adressering in RFM-netwerk
  * binaire payload, LPP formaat
  * gateway, protocolconversie

In dit hoofdstuk behandelen we de RFM69-keten, als voorbeeld van een lokaal protocol.
We gebruiken de RFM69-radio als *pakketradio* in de 868MHz band.
Deze radio heeft een bereik van maximaal 50-200 meter:
geschikt voor toepassingen in en rond huis (of school).
Ook radio's als ZigBee en Z-wave gebruiken deze band, voor vergelijkbare toepassingen.

Omdat de RFM69-pakketten vrij klein zijn (ca. 60 bytes), gebruiken we het *binaire LPP-formaat* voor de payload.
Een *gateway* sluit het RFM69-netwerk aan op het internet
Deze zet de RFM69-LPP-pakketten om in MQTT/JSON-berichten, en omgekeerd (*protocolconversie*).

.. Topic:: De 868 MHz ISM band

  De 868 MHz band is een zogenaamde *Industrial, Scientific and Medical (ISM) band*:
  deze band is vrij te gebruiken, zonder speciale licenties.
  Wel zijn er enkele beperkingen om ervoor te zorgen dat de gebruikers elkaar niet in de weg zitten:

  * een radio mag alleen met een klein vermogen zenden;
  * een radio mag niet meer dan 1% van de tijd zenden (*1% max. duty cycle*)

  Een pakketradio mag dan na het versturen van een pakket met een *air time* van 0.1 seconde,
  de volgende 10 seconden niet zenden.

  Je kunt de *air time* van aan pakket klein houden door (i) het aantal bytes per pakket te beperken;
  (ii) een grote bitrate te gebruiken.
  Als je de bitrate groter maakt, neemt het bereik af.
  Voor een lokale radio zoals de RFM69 (bereik 50-200m) maken we daarom andere keuzes dan voor "long range" radio's zoals LoRa (bereik tot enkele km).


RFM69 keten
===========

.. figure:: rfm69/IoT-rfm69-keten.png
  :width: 600 px
  :align: center

  De RFM69 keten

We gebruiken een eigen protocol voor de RFM69-radio:
hiermee houden we de pakketten klein, en de software eenvoudig.
Het binaire Cayenne Low Power Payload (LPP) formaat gebruiken we voor de payload.

Een *gateway* zet dit RFM69/LPP-protocol om in het MQTT/JSON-protocol voor onze IoT-toepassingen.

Op het niveau van onze IoT-toepassingen hebben alle MQTT/JSON-berichten dezelfde structuur:
de berichten van de RFM69-IoT-knopen verwerken we op dezelfde manier als de berichten van de WiFi-knopen.


(We gebruiken voor de IoT-toepassing een JSON-formaat met dezelfde structuur, zie XXX.)

RFM69 protocol
==============

We hebben te maken met de volgende *protocolstack*:

* het hardware-protocol (RFM69 radio);
* het protocol van de RFM69-library;
* het protocol van de IoT-toepassing

.. figure:: rfm69/IoT-rfm69-pakket.png
  :width: 600 px
  :align: center

  RFM69 pakket met LPP payload

Het *hardware-protocol* zorgt voor:

* het adresseren (*destination*), zenden en ontvangen van pakketten;
* het controleren van de CRC (cyclic redundancy check, checksum):
  als deze klopt, concludeert de hardware dat het pakket correct ontvangen is;
* het versleutelen en ontsleutelen van de payload (encryptie).

Het protocol van de *RFM69-library* voegt het adres van de afzender (*source*) toe aan het pakket,
als eerste byte van de payload in het hardware-pakket.
Het protocol van de RFM69-library noemen we hier het *RFM69-protocol*.

Het RFM69-protocol gebruikt 5-bits adressen (0..63) voor de knopen in het netwerk.
De adressen 0, 61, 62 en 63 hebben een speciale betekenis.
RFM69-adres 0 als *destination* is het *broadcast address*: alle IoT-knopen in het RFM69-netwerk ontvangen zo'n bericht.

Het *toepassingsprotocol* gebruikt de rest van de hardware-payload.
Dit protocol gebruikt verschillende formaten voor uplink- en voor downlink-pakketten.
We bespreken hier alleen de binaire formaten op basis van het Cayenne *low power payload* (LPP) formaat.
Dit protocol noemen we hier het *appl:LPP* protocol.

.. rubric:: *appl:LPP* uplink-berichten

De payload voor uplink-berichten bevat de volgende elementen:

* *port* (1 byte): dit geeft aan welk protocol gebruikt wordt voor de payload;
  ``port=1`` geeft het LPP-formaat aan.
* *nodeid* (2 bytes): de identificatie van de knoop (gebruikt in het MQTT-topic);
* *counter* (2 bytes): het volgnummer van het bericht;
  we gebruiken dit onder meer voor de veiligheid, om een *replay attack* te voorkomen.
* LPP-*payload*, per sensor/actuator:
    * *channel* (1 byte): identificeert de sensor/actuator in de IoT-knoop;
    * *type* (1 byte):type van de sensor/actuator;
    * *value* (1 of meer bytes): de waarde(n) van de sensor (of actuator).

Voorbeelden van sensor/actuatorgegevens in LPP-formaat:

* ``0, 1, 1``: channel=0 (LED 0), type="dOut", value=1 ("on")
* ``3, 103, 0, 235``: channel=3, type="temperature", value=235 (23,5 'C)

De waarde(n) van een sensor/actuator bestaat uit een reeks gehele getallen, van 1 of 2 bytes elk.
Door gehele getallen te gebruiken voorkomen we onder meer het (dure) rekenen met floating point getallen in de IoT-knoop.
Bovendien leveren de meeste sensoren een geheel getal als gemeten waarde:
het werken met floating point getallen voegt niets toe.

De types en de interpretatie van de data staan beschreven in de Cayenne-documentatie,
zie XXX

Enkele voorbeelden van veel voorkomende types sensoren en actuatoren:

.. csv-table:: LPP types
   :header: "Sensor", "Naam", "Type", "Hex", "Bytes", "Resolutie"
   :widths: 15, 10, 5,  2, 2, 15

   "Digitale input",    "dIn",    0, 0, 1, "1"
   "Digitale output",   "dOut", 	1, 1,	1, "1"
   "Analoge input", 	  "aIn",  	2, 2,	2, "0.01 Signed"
   "Analoge output", 	  "aOut", 	3, 3,	2, "0.01 Signed"
   "Lichtniveau",       "illuminance",  101, 65, 2, "1 Lux Unsigned"
   "Aanwezigheid",      "presence",     102, 66, 1, "1"
   "Temperatuur",       "temperature", 	103, 67, 2,	"0.1 °C Signed"
   "Rel. Luchtvochtigheid", "humidity", 104, 68, 1, "0.5% Unsigned"
   "Luchtdruk",         "barometer",    115, 73, 2,	"0.1 hPa Unsigned"

.. rubric:: *appl_LPP* downlink-berichten

De payload voor een downlink-bericht is erg eenvoudig:

* *port*: geeft het toepassingsprotocol aan; ``port=1`` staat voor de LPP payload;
* de LPP-payload, per actuator:
    * *channel* (1 byte);
    * *value* (1 of 2 bytes)
* een afsluiter (1 byte): ``0xff`` (geen geldig channel)

We nemen hier het type van de actuator niet op: dit is in de knoop zelf bekend.
Ook de *nodeid* is niet nodig: de IoT-knoop kent zijn eigen *nodeid*.

* een *counter* (volgnummer) voor de veiligheid is wel nodig;???

Gateway
=======

De *gateway* koppelt het RFM69-netwerk aan het internet, meer in het bijzondere aan de IoT-toepassing via het MQTT-protocol.
Een RFM-IoT-knoop verstuurt RFM69-pakketten met daarin een IOT-toepassingspakket met een LPP-payload.

.. figure:: rfm69/IoT-rfm69-keten-stacks.png
  :width: 600 px
  :align: center

  Protocol-stacks in de RFM69-keten

De omzetting tussen de beide protocollen bestaat uit twee onderdelen:

* omzetten van de *adressering*: van RFM69-adres naar MQTT-topic (en omgekeerd);
* omzetten van de *payload*: van RFM69-LPP-pakket naar MQTT-JSON bericht.

Opmerking: de omzetting van de adressering voor *uplink*-berichten is eenvoudig:
de *nodeid* in het RFM69-LPP-pakket wordt ingevuld in het MQTT-topic ``node/<nodeid>/sensors``.

Voor *downlink*-berichten is dit lastiger: in het IoT-toepassingsprotocol (MQTT/JSON) is het RFM69-adres onbekend.
De gateway houdt daarom een tabel bij: nodeid -> RFM69-adres.
Als de gateway een RFM69-LPP-bericht ontvangt (in het formaat hierboven) wordt deze tabel bijgewerkt.
Als de gateway vervolgens een MQTT-bericht ontvangt voor een topic met de nodeid van een lokale node, wordt dit (na omzetting) verstuurd naar de bijbehorende RFM69-node.

Opdrachten
==========

.. toctree::

  rfm69/opdrachten.rst
  rfm69/toetsvragen.rst
