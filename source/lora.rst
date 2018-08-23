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

.. toctree::
  :hidden:

  lora/lorawan-keten.rst
  lora/opdrachten.rst
  lora/toetsvragen.rst
