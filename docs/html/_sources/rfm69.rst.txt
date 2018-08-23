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

.. toctree::
  :hidden:

  rfm69/rfm69-keten.rst
  rfm69/opdrachten.rst
  rfm69/toetsvragen.rst
