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
    De "eigenlijke data" die getransporteerd wordt als onderdeel van een bericht, pakket of frame.
    Zie ook *metadata*.

  metadata
    "data voor het beschrijven van data".
    In het geval van communicatie, meestal data over de communicatie,
    bijvoorbeeld het volgnummer van een bericht, tijdstip van de communicatie,
    of de signaalsterkte bij radiocommunicatie.

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
    communicatie in de verwachting dat deze meestal aankomt, zonder garantie voor de betrouwbare aflevering.
    Veel basisprotocollen zijn best effort, bijvoorbeeld: Ethernet, IP, RFM69, LoRa.
    Betrouwbare communicatie moet dan (als dat nodig is) door een protocol in een hogere laag gerealiseerd worden.
    ("best effort" zegt niets over de daadwerkelijke betrouwbaarheid van de communicatie.)

  sensor
    (elektronisch) onderdeel voor het waarnemen of meten van een fysisch verschijnsel: bijvoorbeeld temperatuur, luchtdruk,
    licht, beeld, geluid, versnelling, gasconcentraties, enz.
    Ook drukknoppen, camera's, microfoons, GPS-ontvangers zijn voorbeelden van sensoren.
    Moderne smartphones hebben tientallen sensoren.

  actuator
    onderdeel voor het besturen van een fysisch verschijnsel (vanuit een elektronische schakeling).
    Voorbeelden van sensoren: luidspreker, beeldscherm, lamp, led, relais (voor het schakelen van grotere stromen),
    waterklep, buzzer, enz.

  controller
    computer of computerprogramma voor het meten en aansturen van fysische verschijnselen via sensoren en actuatoren.
    Een IoT-knoop heeft een *microcontroller*: een volledige besturingscomputer op een chip.

  signaal
    een signaal heeft op elk moment een waarde.
    In een signaal kunnen *events* gedetecteerd worden, bijvoorbeeld de overgang van "laag" naar "hoog";
    of een "hartslag".

  event
    gebeurtenis die op een bepaald moment plaatsvindt, waarbij de duur niet van belang is.
    Voorbeelden: het indrukken van een knop; het openen van een deur; een hartslag.
