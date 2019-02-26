********
Glossary
********

.. glossary::

  downlink
    van de toepassing naar de IoT-knoop; tegenover uplink.

  gateway
    verbinding tussen twee netwerken.
    Als de protocollen van deze netwerken verschillen,
    dan verzorgt de gateway de *protocol-conversie*.

  payload
    De "eigenlijke data" die getransporteerd wordt als onderdeel van een pakket of frame.
    Zie ook *metadata*.

  metadata
    "data voor het beschrijven van data".
    In het geval van communicatie, meestal data over de communicatie,
    bijvoorbeeld het volgnummer van een bericht, tijdstip van de communicatie,
    of de signaalsterkte bij de communicatie.

  replay-attack
    poging om in te breken in een beveiligde communicatie, door het herhalen van een vroeger bericht.
    Stel dat je altijd hetzelfde (radio)bericht gebruik om een deur te openen:
    iemand kan dit bericht opvangen en opnieuw afspelen als het hem uitkomt.
    Je kunt dit voorkomen door elk bericht van een volgnummer te voorzien:
    een bericht dat niet in de volgorde past wordt overgeslagen.

  uplink
    van de IoT-knoop naar de toepassing; tegenover downlink.

  pakketradio
    radio voor pakket-georiënteerde communicatie. Pakketten voor IoT-radio's zijn vaak vrij klein,
    met bijvoorbeeld een maximale payload van 60 bytes (RFM69) of 51 bytes (LoRaWan - KPN).

  best effort communicatie
    communicatie zonder garantie voor de betrouwbare aflevering. Veel basisprotocollen zijn best effort,
    bijvoorbeeld: Ethernet, IP, RFM69, LoRa.
    Betrouwbare communicatie moet dan (als dat nodig is) door een protocol in een hogere laag gerealiseerd worden.
    ("best effort" zegt niets over de daadwerkelijke betrouwbaarheid van de communicatie.)
