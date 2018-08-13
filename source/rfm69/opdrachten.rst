**********
Opdrachten
**********

.. bij RFM69

(1) Radio-bereik
================

Het bereik van een radio hangt onder meer af van het zendvermogen,
de frequentie van het signaal, de bandbreedte, de bitrate, de antenne,
de plaatsing en de obstakels tussen zender en ontvanger.

In deze opdracht bestudeer je het bereik van de RFM69(H)-radio,
met de gebruikelijke configuratie (vermogen, frequentie, bandbreedte, bitrate, antenne).

Als zender gebruik je een RFM69 IoT-knoop met het programma "ping".
Als ontvanger gebruik je een RFM69-WiFi-gateway met de normale gateway-software.

Het programma "ping" stuurt elke 5 seconden een speciaal *ping* bericht naar de gateway.
De gateway stuurt dan direct een bericht terug naar de afzender.
De gateway stuurt ook een MQTT/JSON-bericht met als topic ``node/<nodeid>/ping`` naar de broker;
dit bericht bevat onder meer de signaalsterkte (``rssi``) van het ontvangen bericht.

1. bepaal het maximale bereik van de radio "in het open veld",
   waarbij er geen obstakels zijn tussen de zender (IoT-knoop) en de ontvanger.
   Plaats de RFM69-WiFi-gateway zodat deze een WiFi-verbinding heeft.
2. bepaal het maximale bereik van de radio in het gebouw waarin je dit gebruikt,
   bijvoorbeeld je school.
3. maak een tabel met de signaalsterkte als functie van de afstand (in het open veld of in een gebouw).
   (Je kunt de MQTT-berichten via MQTTT ontvangen op je mobiele telefoon.)

(2) Payload-formaat
===================

De RFM69-IOT-knoop stuurt een payload in binair formaat, volgens de Cayenne low power payload-regels.
De gateway zet deze binaire payload om in een JSON-formaat.
Het JSON-bericht bevat ook de oorspronkelijke binaire payload.

* bestudeer de binaire payload van een bericht .
  Geef de overeenkomst aan tussen de bytes in de binaire payload en de payload-elementen in het JSON-bericht.

(3) Dashboard
=============

Maak een dashboard voor een RFM69 IoT-knoop, op dezelfde manier als voor een WiFi-knoop.

(4) Koppelen van twee knopen
============================

Koppel via NodeRed de knoppen van een RFM69-IoT-knoop aan een LED van een andere IoT-knoop.
Hiervoor gebruik je dezelfde aanpak als voor de WiFi-knopen.

*Opmerking:* bij de bovenstaande opdrachten heb je voordeel van het uniforme JSON-formaat voor de verschillende soorten knopen.
