.. _configureren:

************
Configureren
************

WiFi-knoop (ESP8266)
====================

Voor het configureren van een IoT-knoop met de software ``wifi-node-x`` heb je nodig:

* de gegevens van het WiFi-netwerk: SSID (netwerknaam) en wachtwoord;
* de MQTT-broker-gegevens: domeinnaam, poortnummer, gebruikersnaam, wachtwoord

Vraag deze gegevens eventueel aan je docent.

.. adminition: Let op

  N.B. De knopen werken alleen met netwerken met een wachtwoord voor het netwerk, voor alle gebruikers.
  Veel eenvoudige WiFi-netwerken, zoals een thuisnetwerk of een telefoon als access point, gebruiken deze aanpak.
  De knopen werken niet met WiFI-netwerken die voor elke gebruiker een naam/wachtwoord-combinatie hebben,
  zoals veel schoolnetwerken.

.. figure:: configureren/IoT-conf-step123a.png
  :width: 800px
  :align: center

  IoT-knoop reset naar access point mode

Stappen voor het configureren:

1. Reset de knoop in "Access Point" mode:
    1. druk *button 0* in (de buitenste of rechterknop), en houd deze ingedrukt.
    2. druk de *reset-knop* in (links van de USB-aansluiting),
       en laat deze weer los;
    3. laat *button 0* na 3 seconden los (als led0 brandt);
    4. NB: in een oudere versie van de software krijg je niet dezelfde feedback van de leds,
       voor het overige werkt de Access-point reset op dezelfde manier.
2. Maak via de browser contact met de knoop:
    1. selecteer in je computer het WiFi-netwerk van de knoop;
       dit heeft als naam ``ESPAP-`` gevolgd door de ID van de knoop,
       bijvoorbeeld ``ESPAP-8f12``
    2. geef in de browser het IP-adres van het access point op: ``192.168.4.1``.
       Je krijgt nu de homepagina van de webserver van de knoop te zien,
       met onder andere de waarden van de sensoren.
3. Klik in de homepagina op de link: ``Setup``; op de setup-pagina kun je de configuratie-parameters instellen.
   Vul alleen die gegevens in die nieuw zijn: al eerder ingevulde gegevens (ook niet-getoonde wachtwoorden) blijven bewaard.
4. "Submit" de ingevulde gegevens.
5. Reset de knoop met de *reset-knop* (indrukken en loslaten).
   De knoop probeert nu verbinding te maken met het lokale WiFi netwerk,
   en vervolgens met de broker.
   Tijdens het zoeken naar het lokale WiFi-netwerk brandt de blauwe LED op de knoop;
   als dit erg lang duurt, probeer je nog een hardware-reset.
6. Selecteer weer het normale WiFi-netwerk in je computer.
7. Via :ref:`MQT3` kun je nu controleren of de MQTT-broker de berichten van de IoT-knoop ontvangt.
   Stel in het ``IoT-node``-venster de ID van de knoop in, bijvoorbeeld ``8f12``.
   Na enige tijd moeten dan de waarden van de sensoren verschijnen.
   Je kunt met MQT3 ook de LED aan- en uitschakelen.

RFM69-WiFi gateway
==================

Het configureren van de RFM69-WiFi-gateway gaat op dezelfde manier als bij de WiFi-knoop.

Zodra de gateway voor het lokale WiFi-netwerk en de MQTT-broker geconfigureerd is,
stuurt deze de berichten van de lokale RFM69-knopen door.

RFM69-knoop
===========

De RFM69-knopen kunnen via de Seriële Monitor van de Arduino IDE geconfigureerd worden.
Het is niet nodig om deze knopen opnieuw te programmeren.
(Dat blijft wel mogelijk, via de Arduino IDE, zie IoT-1.)

De configuratie van de RFM69-knopen is in principe eenmalig:
je hoeft deze alleen aan te passen als het RFM69-netwerk verandert,
niet bij een ander WiFi-netwerk (daarvoor pas je de gateway aan).

LoRa-knoop
==========

De configuratie van een TTN LoRa(Wan)-knoop betekent dat je deze koppelt aan de toepassing.
Een TTN-LoRa(Wan)-knoop werkt met iedere TTN-gateway;
je kunt deze zelfs mobiel gebruiken.

Met de huidige versie van de software kun je een knoop alleen configureren door deze opnieuw te programmeren:
de codes voor de toepassing pas je aan in de programmatekst.
