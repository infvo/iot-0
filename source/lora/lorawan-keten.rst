*************
LoRaWan-keten
*************

De LoRaWan-keten
================

.. figure:: IoT-keten-LoRaWan.png
  :width: 600px
  :align: center

  LoRaWan-keten, van IoT-knoop tot dashboard

De IoT-knopen zijn via de LoRa-radio en het LoRaWan-protocol verbonden met de LoRaWan-gateway(s).
Door het grote bereik van de radio kan een bericht van een IoT-knoop door meerdere gateways ontvangen worden.
Deze gateways sturen de "upstream" berichten van de IoT-knopen door naar de TTN/LoRaWan-server.
Deze combineert de berichten van meerdere gateways afkomstig van een enkele IoT-knoop tot één bericht.
Dit gecombineerde bericht kun je bekijken via de TTN-console.
De TTN-server kan dit ook doorsturen via het MQTT-protocol naar een NodeRed-server,
die dit bericht kan gebruiken in een dashboard.

Vanuit NodeRed kun je ook "downstream" berichten sturen naar de IoT-knoop.
De TTN-server kiest hiervoor een gateway dichtbij de knoop.
Deze bewaart het bericht totdat er een bericht van de IoT-knoop binnenkomt:
als reactie stuurt de gateway het downstream-bericht naar de IoT-knoop.
Met andere woorden: de IoT-knoop bepaalt wanneer deze een downstream-bericht kan ontvangen.

Zowel de gateways als de TTN-server behandelen de berichten van meerdere toepassingen.
Deze toepassingen worden van elkaar afgeschermd door middel van encryptie:
de eigenaar van een toepassing bepaalt welke IoT-knopen gekoppeld zijn aan de toepassing,
en wie toegang tot de data van deze IoT-knopen heeft.



LoRa-radio
==========

De LoRa-radio ("long range") is een *pakketradio* bedoeld voor communicatie over grotere afstanden,
van enkele honderden meters tot enkele kilometers.
De communicatie is *best effort*, met beperkte mogelijkheden voor ontvangstbevestiging.

De LoRa-radio gebruikt (in Europa) de 868MHz ISM-band.
Deze band kun je zonder vergunning gebruiken, mits je je houdt aan de volgende regels:

* een radio mag zenden met een maximaal vermogen van +20dB;
* een radio mag maximaal 1% van de tijd zenden, (gemiddeld over een uur?)

Bij een IoT-radio kun je een afweging maken tussen bitrate en bereik.
De LoRa-radio maakt een groot bereik mogelijk, met als gevolg een lage bitrate.
In combinatie met de beperkingen van de ISM-band betekent dit dat een LoRa-radio maar een klein aantal kleine pakketten per uur mag versturen.

  De Sigfox-radio is een ander voorbeeld van een radio met groot bereik en lage bitrate.
  Voor Sigfox gelden de volgende regels/eigenschappen:

      * 12 bytes upload (payload); frame 26 bytes;
      * 8 bytes download (payload)
      * low bitrate: 100-600 bit/s (26*8 bits = 208 bits: ca. 2 sec per message)

Ook de gateway moet voldoen aan de 1%-beperking van de ISM-band.
Omdat een gateway mogelijk enkele honderden IoT-knopen kan bedienen,
is het aantal uitgaande (downstream) berichten van een gateway *per IoT-knoop* veel beperker dan het aantal uitgaande (upstream) berichten van een IoT-knoop.

Spreading factor en airtime
---------------------------

De afweging tussen bereik en bitrate wordt dynamisch gemaakt:
als een LoRa-radio voor het eerst verbinding zoekt met een LoRa gateway,
bepalen de radio en de gateway samen de beste verdeling tussen bereik en bitrate.
Een lage "spreading factor" (SF) geeft een hoge bitrate met een beperkt bereik.
Een hoge SF geeft een groter bereik, maar met een beperkte bitrate.
Elke volgende SF (tussen SF7 en SF12) geeft de helft van de bitrate van de vorige.

De *airtime* van een radio-bericht is de tijd dat dit bericht het radiokanaal gebruikt.
Gedurende het verzenden van een bericht kan het radiokanaal niet door anderen gebruikt worden.
Eenzelfde bericht neemt bij een volgende SF tweemaal zoveel airtime.
Een bericht dat bij SF7 50ms kost, neemt bij SF12 2^5 * 50 = 32 * 50 ms = 1,6 s.
Bij SF7 mag je ruim 30 maal meer berichten per uur sturen dan bij SF12.

Binaire codering van berichten
------------------------------

Bij een lage bitrate is het belangrijk om  de berichten kort te houden.
Voor LoRa-berichten gebruik je daarom meestal een binaire formaat (zoals Cayenne LPP),
in plaats van een tekstformaat zoals JSON.
TheThingsNetwork heeft voorzieningen voor het decoderen van een binair formaat van een application.

LoRaWan
=======

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


==> Wat zijn de eigenschappen van LoRaWan?

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

------------

TheThingsNetwork
================

TheThingsNetwork is een speciale LoRaWan-aanbieder: het netwerk is opgebouwd door een *community*.
Het netwerk is gratis te gebruiken.
Er zijn overal in de wereld, en vooral in Nederland, lokale *communities* die ervaringen uitwisselen,
en die zorgen voor de lokale LoRaWan-dekking door eigen gateways aan te bieden.

  Als school (of als hobbyist) kun je je eigen TTN-gateway installeren, en daarmee het TTN-netwerk versterken.

Fair access policy
------------------

Het gebruik van de TTN infrastructuur is gratis,
maar gebruikers moeten zich wel "netjes gedragen".
TTN gebruikt als "fair access policy":

* de totale duur (airtime) van de berichten van een radio (IoT-knoop) mag per dag niet meer dan 30 s. zijn.

Deze voorwaarde komt bovenop de 1%-eis van de ISM-band.


-------

LoRaWan-toepassingen
====================

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


.. rubric:: LoRaWan en TheThingsNetwork


(mogelijk voor vragen:)

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

----

Beveiliging: volgnummers (en voor controle op verloren berichten). (Wordt bij OOTA automatisch op 0 gezet.)

Details bij IoT-1: OOTA vs. ABP; wanneer wel/niet ontvangstbevestiging.

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

   * LoRa radio: adressering
   * volgnummers, voor beveiliging, en controle op ontvangst
   * LoRaWan netwerk/protocol: architectuur; div. aanbieders
   * TheThingsNetwork: wereldwijd LoRaWan netwerk


In dit hoofdstuk maken we kennis met LoRaWan: een netwerk voor IoT-knopen met een LoRa (long range) radio.
Deze LoRa radio biedt een groot bereik,tot enkele kilometers;
dit gaat ten koste van de bitrate en van het aantal berichten per uur, maximaal ca. 10-20.
We beschrijven eerst de eigenschappen van de LoRa radio,
vervolgens de opzet van een LoRaWan netwerk,
en daarna een speciale aanbieder van dit netwerk: TheThingsNetwork.

Tenslotte beschrijven we een aantal opdrachten en experimenten met voorgeconfigureerde LoRaWan-knopen.
Voor een beschrijving van de gebruikte onderdelen in meer detail verwijzen we weer naar IoT-2: IoT voor makers.




Typische toepassingen van LoRaWan

----

Links
=====

* https://en.wikipedia.org/wiki/LoRa
* https://www.thethingsnetwork.org
* `spreadsheet voor berekenen van LoRaWan airtime <https://docs.google.com/spreadsheets/d/1QvcKsGeTTPpr9icj4XkKXq4r2zTc2j0gsHLrnplzM3I/edit#gid=0>`_
* https://zakelijkforum.kpn.com/lora-forum-16/welcome-to-the-kpn-lora-forum-an-overview-11123
