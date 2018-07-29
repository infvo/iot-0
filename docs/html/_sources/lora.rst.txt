***********
LoRa-knopen
***********

.. todo::

  * LoRa
  * LoRaWan
  * TTN
  * gateway; protocol-conversie (van binair naar JSON)
  * LoRaWan-server; protocol-conversie, formaat-conversie (van payload)

De LoRa (long range) radio werkt in dezelfde vrije ISM-frequentieband (868MHz) als bijvoorbeeld SigFox, RFM69, ZigBee en Z-wave.
Dit betekent dat deze radio dezelfde beperkingen heeft:
ten hoogste 1% van de tijd zenden met een beperkt vermogen.
De LoRa-radio (long range) heeft een bereik van een paar kilometer;
een groter bereik gaat ten koste van de bitrate (bits/s):
je gebruikt dan meer energie per bit, over de tijd uitgesmeerd.
Je kunt het bereik aanpassen aan de omstandigheden:
dit betekent dat de bit-rate groter kan zijn als je een kleinere afstand hoeft te overbruggen.
Eenzelfde bericht kan dan in kortere tijd verstuurd worden, met minder energie.
Dit laatste betekent een langere batterijduur voor een low-power IoT-knoop.

Het bereik (en daarmee de bitrate) wordt aangegeven door de *Spreading Factor* (SF),
gebruikelijk van SF7 tot SF12.
Een volgende SF-waarde geeft een halvering van de bitrate,
een verdubbeling van de energie per bit, en een groter bereik.

Als een LoRa-IoT-knoop zich aanmeldt bij een gateway,
onderhandelen deze om de laagst bruikbare SF te vinden.
Dit verkleint de belasting van het netwerk en de energie die de IoT-knoop nodig heeft.

De LoRa-radio biedt *best-effort* *pakketcommunicatie*,
waarbij een pakket gewoonlijk een *payload* heeft van een 10-tal bytes.
Door het gebruik van opeenvolgend genummerde pakketten kun je zien of er pakketten verloren zijn gegaan.

.. rubric:: LoRaWan en TheThingsNetwork

*LoRaWan* is een netwerkprotocol op basis van LoRa.
Een typisch LoRaWan-netwerk bestaat (naast de LoRa-IoT-knopen) uit één of meer *gateways*,
die verbonden zijn met een *server*.
Deze server coördineert het verkeer van en naar de gateways,
en zorgt voor de interactie met de rest van het internet.

Je kunt een gateway in dit verband vergelijken met een mobiele zendmast:
deze zorgt voor het verkeer tussen de (mobiele) IoT-knopen en de rest van het netwerk.
Overigens kan een bericht van een IoT-knoop door meerdere gateways ontvangen worden.

Er zijn verschillende aanbieders met een eigen LoRaWan-netwerk,
in Nederland onder andere KPN.

Een bijzondere aanbieder is TheThingsNetwork:
dit netwerk is georganiseerd rond een *community* van individuen, groepen en bedrijven,
die hun eigen gateways aanbieden voor dit netwerk.
Iedereen kan in deze community meedoen, en gratis deze infrastructuur gebruiken.

.. rubric:: gateways

.. rubric:: TTN-server



.. rubric:: TTN - Fair use regels

Er zijn wel enkele regels voor het eerlijk gebruik ("fair use"):
één van die regels is dat een radio niet meer dan 30 seconden per dag mag zenden.
Op die manier geef je de andere IoT-knopen ook de gelegenheid om het netwerk te gebruiken.

Je kunt de tijd die een radio (van een IoT-knoop) zendt beperken door:

* korte berichten te versturen, van enkele bytes;
* het aantal berichten per uur te beperken (tot 10-20);
* waar mogelijk gebruik te maken van een gateway in de buurt.

Deze regels zijn ook gunstig voor het verlengen van de batterijduur van de IoT-knoop.

.. rubric:: LoRaWan-toepassingen

Voor welke toepassingen is LoRaWan geschikt?

*
* veiligheid (security): de communicatie gebruikt "end-to-end encryptie",
  van de IoT-knoop tot de toepassing in de TTN-server.

Voor welke toepassingen is LoraWan *niet* geschikt?

* veel berichten per uur;
* grote berichten (meer dan een 10-tal bytes);
* grote betrouwbaarheid;


.. rubric:: links

* https://www.rs-online.com/designspark/eleven-internet-of-things-iot-protocols-you-need-to-know-about

.. toctree::

  lora/lorawan-keten.rst
  lora/opdrachten.rst
  lora/toetsvragen.rst
