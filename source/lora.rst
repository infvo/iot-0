***********
LoRa-knopen
***********

.. todo::

  * LoRa
  * LoRaWan
  * TTN
  * gateway; protocol-conversie (van binair naar JSON)
  * LoRaWan-server; protocol-conversie, formaat-conversie (van payload)

LoRa is een radiotechnologie waarmee je redelijk grote afstanden kunt overbruggen:
van enkele honderden meters tot enkele kilometers.
Dit vormt de basis voor LoRaWan (*Wide Area Network*) netwerken, waarin je IoT-knopen in een groot gebied kunt verbinden.

  *Je kunt LoRaWan vergelijken met het netwerk voor mobiele telefonie - maar dan voor IoT-knopen.*

Dit grote bereik gaat ten koste van de bandbreedte (bitrate): je kunt maar enkele kleine kleine berichten versturen per uur.
Hierdoor is LoRaWan vooral geschikt voor monitoring-toepassingen met eenvoudige sensoren;
voor besturingstoepassingen is dit minder geschikt.

Alternatieven voor LoRaWan zijn bijvoorbeeld SigFox en NB-IoT.
De toepassingsmogelijkheden voor deze technologieën zijn vergelijkbaar met die van LoRaWan.

TheThingsNetwork (TTN) is een LoRaWan-aanbieder (*provider*) op basis van een *community*:
deze community verzorgt de infrastructuur van het netwerk.
Dit TTN-netwerk is voor iederen gratis te gebruiken.

Naast TTN zijn er ook commerciële aanbieders, zoals KPN.
Sommige bedrijven gebruiken een eigen LoRaWan-netwerk.

We geven in dit hoofdstuk eerst een overzicht van de LoRaWan-keten.
Vervolgens behandelen we de LoRa-radio.
Daarna komt de LoRaWan-keten aan bod, aan de hand van TheThingsNetwork.
Tenslotte geven we enkele voorbeelden van LoRaWan-toepassingen.


.. toctree::
  :hidden:

  lora/lorawan-keten.rst
  lora/ttn-console.rst
  lora/opdrachten.rst
  lora/toetsvragen.rst
