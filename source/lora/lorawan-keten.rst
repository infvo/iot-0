*************
LoRaWan-keten
*************

.. figure:: IoT-publieke-gateway-1.png
  :width: 600px
  :align: center

  LoRaWan-keten met publieke gateway

* lora: best-effort pakketcommunicatie
* pakket:
* adressering:
* payload: zo compact mogelijk; binaire codering
* volgnummers: (detectie van verloren pakket)
* gateway:
    * protocol-conversie
    * metadata
    * onderhandeling met IoT-knoop
* server:
    * application:
    * device: (IoT-knoop)
* toepassing: (bijv. dashboard)
    * communicatie via mqtt
    * hoe gebeurt authenticatie van gebruiker?


.. todo::

   * LoRa radio: eigenschappen, regels voor gebruikers
       * begrip payload
       * adressering
       * best-effort pakketcommunicatie
   * LoRaWan netwerk: architectuur; div. aanbieders
   * TheThingsNetwork: wereldwijd LoRaWan netwerk
   * LoRa radio module: RFM95 (880 MHz)
   * LoRa radio module - driver (Arduino)
   * wat zijn typische LoRa-toepassingen?


In dit hoofdstuk maken we kennis met LoRaWan: een netwerk voor IoT-knopen met een LoRa (long range) radio.
Deze LoRa radio biedt een groot bereik,tot enkele kilometers;
dit gaat ten koste van de bitrate en van het aantal berichten per uur, maximaal ca. 10-20.
We beschrijven eerst de eigenschappen van de LoRa radio,
vervolgens de opzet van een LoRaWan netwerk,
en daarna een speciale aanbieder van dit netwerk: TheThingsNetwork.

Tenslotte beschrijven we een aantal opdrachten en experimenten met voorgeconfigureerde LoRaWan-knopen.
Voor een beschrijving van de gebruikte onderdelen in meer detail verwijzen we weer naar IoT-2: IoT voor makers.




Typische toepassingen van LoRaWan


LoRa-radio
==========

De LoRa-radio gebruikt de 868MHz band (tenminste, in Europa; elders worden ook 433MHz en 915MHz gebruikt).
Deze band kun je zonder vergunning gebruiken, mits je je houdt aan de volgende regels:

* een radio mag zenden met een maximaal vermogen van +20dB;
* een radio mag maximaal 1% van de tijd zenden, (gemiddeld over een uur?)

Zie:

(Ook de RFM69-radio gebruikt deze radioband, en heeft dezelfde beperkingen.)

Bij IoT-radio's heb je te maken met een afweging tussen het bereik en de beschikbare bandbreedte (bitrate).
Bij de LoRa radio is gekozen voor een groot bereik, ten koste van de bitrate.

De afweging tussen bereik en bitrate wordt dynamisch gemaakt:
als een LoRa-radio voor het eerst verbinding zoekt met een LoRa gateway,
bepalen deze samen de beste verdeling tussen bereik en bitrate.
Een lage "spreading factor" (SF) geeft een hoge bitrate met een beperkt bereik.
Een hoge SF geeft een groter bereik, maar met een beperkte bitrate.
Elke volgende SF (tussen SF7 en SF12) geeft de helft van de bitrate van de vorige.

De *airtime* van een radio-bericht is de tijd dat dit bericht het radiokanaal gebruikt.
Gedurende het verzenden van een bericht kan het radiokanaal niet door anderen gebruikt worden.
Eenzelfde bericht neemt bij een volgende SF tweemaal zoveel airtime.
Een bericht dat bij SF7 10ms kost, neemt bij SF12 2^5 = 32 * 10 ms.

Het is dus belangrijk om bij een lage bitrate de berichten *zo kort mogelijk* te houden.
Een speciale binaire codering van sensorwaarden is (aanmerkelijk) korter dan een JSON-formaat.
(TheThingsNetwork heeft speciale voorzieningen voor de decodering van zo'n speciaal binair formaat.)


Een lage bitrate heeft nog een nadeel: de latency neemt erdoor toe.
Immers, de latency is tenminste even groot als de airtime.

De Sigfox-radio is een ander voorbeeld waarbij dezelfde keuze gemaakt is.
Voor Sigfox gelden de volgende regels/eigenschappen:

    * 12 bytes upload (payload); frame 26 bytes;
    * 8 bytes download (payload)
    * low bitrate: 100-600 bit/s (26*8 bits = 208 bits: ca. 2 sec per message)

Voor de radioband (880MHz) die LoRa gebruikt gelden de volgende regels:

LoRaWan
=======

De LoRa-radio biedt alleen elementaire pakket-communicatie: voor het communiceren in een netwerk heb je nog een extra protocol daarboven nodig.
LoRaWan is het netwerk-protocol voor LoRa-radio's.

Er zijn allerlei aanbieders voor LoRaWan-netwerken: via deze aanbieders kun je je eigen LoRaWan-knopen aansluiten op het internet.
Je kunt deze aanbieders vergelijken met de aanbieders van mobiele telefonie:
een aanbieder heeft verspreid over het land LoRaWan-gateways staan voor de communicatie met LoRaWan-knopen.
Deze gateways sturen de LoRa-berichten door naar de LoRaWan-servers, die zorgen voor de aansluiting op het internet.
LoRaWan-toepassingen communiceren met deze LoRaWan-servers.

.. todo::

  * Figuur van LoRaWan-keten.
  * LoRaWan - protocol stack
  * LoRaWan gateway: protocol-conversie (ook nog eens in de server?)

Aanbieders van LoRaWan in Nederland:

* KPN
* TheThingsNetwork

TheThingsNetwork
================

TheThingsNetwork is een speciale LoRaWan-aanbieder: het netwerk is opgebouwd door een *community*.
Het netwerk is gratis te gebruiken.
Er zijn overal in de wereld, en vooral in Nederland, lokale *communities* die ervaringen uitwisselen,
en die zorgen voor de lokale LoRaWan-dekking door eigen gateways aan te bieden.

Voor TheThingsNetwork gelden nog een extra regel:

* de totale duur (airtime) van de berichten van een radio (IoT-knoop) mag per dag niet meer dan 30 s. zijn.

Deze beperking is bedoeld om het TTN-netwerk op een eerlijke manier te gebruiken ("fair use"):
iedereen kan dit netwerk gratis gebruiken, maar de gebruikers moeten wel rekening houden met elkaar.

Typische LoRa(Wan)-toepassingen
===============================

Wat zijn typische LoRa-toepassingen?
Aan welke eisen moeten deze voldoen?

* sensoren (en geen actuatoren)
* met grote tussenpozen
* lage latency niet essentieel

Mogelijke voorbeelden:

* bewaken van parkeerplaatsen (elke 5 min?)
* logistiek: waar is mijn xxx? (elke 5 min?)
* smart agriculture (bijv. koeien in Afrika; eigenschappen van gewassen, grond)
*

Met andere woorden: LoRa is *niet geschikt voor*:

* home control (zoals Philips Hue)

Belangrijke eigenschappen van LoRa(Wan) voor IoT-toepassingen:

* long range (1 km of meer)
* low power
* low cost
    * voor 20-30 euro kun je een LoRaWan-IoT-knoop maken
    * voor 300-500 euro heb je een LoRaWan gateway

Links
=====

* https://en.wikipedia.org/wiki/LoRa
* https://www.thethingsnetwork.org
* `spreadsheet voor berekenen van LoRaWan airtime <https://docs.google.com/spreadsheets/d/1QvcKsGeTTPpr9icj4XkKXq4r2zTc2j0gsHLrnplzM3I/edit#gid=0>`_
* https://zakelijkforum.kpn.com/lora-forum-16/welcome-to-the-kpn-lora-forum-an-overview-11123
