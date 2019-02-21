*************
Voorbereiding
*************

De uitwerking van de opdrachten vereist een IoT-infrastructuur.
De docent moet deze van te voren beschikbaar maken voor de leerlingen.

Voor de IoT-knopen zijn er verschillende alternatieven:

a. gesimuleerde IoT-knopen en IoT-knopen elders.
b. (a), aangevuld met hardware IoT-knopen:
    1. WiFi-knopen (ESP8266)
    2. RFM69-knopen met WiFi gateway
    3. LoRa knopen met LoRa gateway

Het is niet nodig om alle varianten direct beschikbaar te hebben:
je kunt dit in de loop van de tijd uitbreiden.
Het gebruik van hardware IoT-knopen maakt het materiaal wel veel aantrekkelijker voor leerlingen.

Naast deze IoT-knopen zijn er nog nodig:

1. NodeRed server "in the cloud"
    * voor het oefenen met NodeRed kan FRED (fred.sensetecnic.com) gebruikt worden.
    * voor een langduriger
2. MQTT broker "in the cloud"
    * als onderdeel van het lesmateriaal is een MQTT-broker "in the cloud" beschikbaar.
    * als docent kun je hiervoor een account aanvragen (mail naar info at infvo.nl).

.. todo::

  * gebruik van een lokale Raspberry Pi

Een uitgebreide uitleg van de verschillende IoT-knopen is te vinden op IoT-1.
Daar zijn de hardware-schema's te vinden, en de gegevens voor het configureren en programmeren.
De software is te vinden op GitHub:

* MQTTT
* IoT-knoop simulator

Wat moeten leerlingen zelf doen?

* aanmelden bij FRED, voor een gratis account;
* aanmelden bij TTN (gratis).

TheThingsNetwork
================

Leerlingen moeten toegang hebben tot de TTN-application met de actieve IoT-knopen.
Voor de oefenopdrachten kun je met één account per school of per klas werken.


Problemen met het schoolnetwerk
===============================

* schoolnetwerk heeft WiFi-toegang via persoonlijke wachtwoorden
    * de WiFi IoT-knopen en de RFM69-gateways zijn hier niet op voorbereid.
* schoolnetwerk blokkeert MQTT-poorten (1883, 1884).
* schoolnetwerk blokkeert UDP-verkeer voor LoRaWan/TTN gateway.
