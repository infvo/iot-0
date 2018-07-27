**********
Opdrachten
**********

.. voor IoT-bouwstenen

(1) Een eerste IoT-keten
========================

Een eerste voorbeeld van een IoT-keten, van IoT-knoop tot toepassings ("app"), bestaat uit de volgende onderdelen:

* een gesimuleerde IoT-knoop
    * http://infvopedia.nl:1884/iotnode-app-1.html
* een MQTT-broker in het publieke internet;
* het programma MQTT0 (of MQTT1), waarmee je het mqtt-verkeer van de sensor kunt bekijken.
    * http://infvopedia.nl:1884/mqtt1.html

Voer de onderstaande stappen uit:

1. open de gesimuleerde IoT-knoop op in een browservenster
2. open het programma MQTT1 in een ander browservenster
    * deze opzet werkt het best met twee browservensters naast elkaar.
3. voer in het "IoT-node"-venster van MQTT1 de nodeID in van de gesimuleerde IoT-knoop
4. verander één van de sliders van de IoT-knoop
    * je ziet nu de berichten met de nieuwe waarde langskomen, en de waarden bovenin in tabelvorm verschijnen.
5. druk in MQTT1 op de knop om de LED (led0) aan (of uit) te zetten
    * je ziet in de gesimuleerde IoT-knoop de linker LED aan (of uit) gaan.
6. druk één van de knoppen op de IoT-knoop in
    * wat gebeurt er?

Opmerkingen:

* Mogelijk zie je ook berichten van andere IoT-knopen langskomen:
  die gebruiken dezelfde MQTT-broker,
  en via ``subscribe: +/+/+`` ontvangt MQTT1 de berichten van alle knopen.
* De IoT-knoop-simulator verstuurt ca. elke 50 seconden de waarden van de lokale sensoren;
  dit zullen we later ook bij de hardware-IoT-knopen zien.
* je kunt meerdere (gesimuleerde) IoT-knopen hebben met dezelfde node-ID:
  deze zijn op het MQTT-niveau niet van elkaar te onderscheiden.

(2) een tweede IoT-keten
========================

  Verder uitwerken - ook de infrastructuur.

* gebruik van een IoT-dashboard
    * NB: uiteindelijk moeten we een dashboard zien te bieden waarin meerdere sensoren/IoT-knopen gecombineerd worden?
    * gebruik van IoT-dashboard voor gegeven knopen (elders)
    * gebruik van IoT-dashboard voor gesimuleerde knopen
