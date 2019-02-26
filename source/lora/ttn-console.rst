***********
TTN-console
***********

Via het console van TheThingsNetwork kun je de TTN-*application* en de bijbehorende *devices* (IoT-knopen) configureren.
Ook kun je de data van deze knopen zichtbaar maken.

.. figure:: IoT-TTN-console.png
  :width: 500
  :align: center

  TTN application - met IoT-knopen (devices) en externe toepassingen (integrations)


  .. figure:: IoT-TTN-model.png
    :width: 500
    :align: center

    TTN model - application, devices (IoT-knopen) en externe toepassingen.
    (key): gegenereerde waarde; de data onderin zorgen voor de koppeling (pijl).

Als je je *application* selecteert, krijg je in het *Overview* de volgende gegevens te zien:

* Application ID: de logische naam van de application, zoals deze bij TTN bekend is.
  De Application ID naam kies je bij het aanmaken van de application; deze kun je later niet meer veranderen.
  Deze naam moet je o.a. invullen in de TTN-nodes in NodeRed.
* Application EUI: de numerieke ID van de application.
  Deze EUI is een TTN-gegenereerde waarde van 8 bytes/64 bits.
* Devices: de IoT-knopen bij deze application. Een device hoort altijd bij één toepassing.
* Collaborators: de personen die toegang hebben tot deze application.
* Access keys: de sleutels waarmee externe toepassingen toegang hebben tot deze application.
  De toegangsrechten kun je per access key instellen.
  Deze access key moet je bijvoorbeeld invullen in de TTN-nodes in NodeRed.


Je ziet hier ook de tabs voor de andere gegevens:

* Devices - de IoT-knopen;
* Payload formats - voor de conversie van het interne LoRa payload-formaat naar een extern (leesbaar) formaat.
* Integrations - voor de koppeling met externe toepassingen
    * voor NodeRed is deze koppeling al voorbereid.
* Data - de berichten van en naar de devices (IoT-knopen).
* Settings - voor het aanpassen van de verschillende onderdelen van de application.

Device
======

Per *device* (IoT-knoop) vind je de volgende gegevens:

* Device ID: de logische naam van het device in de toepassing.
  Deze naam kies je bij het aanmaken van het device in de application;
  deze kun je later niet meer veranderen.
* Activation method: OTAA of ABP (zie verderop).
* Device EUI: de 64-bits unieke identificatie van het device.
  Het *MAC-address* van het device, door de fabrikant vastgelegd.
  Deze EUI vul je hier in aan de hand van de fabrikantgegevens van het device.
  (Maar, zie verderop.)
* Application EUI: zie hierboven, bij Application.
* App key: op basis van de bovenstaande gegevens, device EUI en application EUI,
  genereert TTN de App key.
  Deze App key moet je *in het device* invoeren, samen met de Application EUI;
  bijvoorbeeld in de programmatekst van het device.
* Device address: 32-bits adres gebruikt voor de communicatie in het netwerk.
  Dit adres wordt toegewezen bij de *device activation*, bijvoorbeeld de *join*-operatie bij OTAA.

Device Activation: OTAA of ABP
------------------------------

*Device activation* is het koppelen van het device (de IoT-knoop) aan het TTN-netwerk.
Hierbij wordt onder meer het 32-bits *device address* voor de communicatie in het netwerk bepaald.
Er zijn twee manieren voor device activation:

* OTAA: over the air activation: het device stuurt een "join" verzoek naar het netwerk (via een gateway).
  In de interactie tussen device en netwerk wordt naast get device address o.a. de encryptiesleutel bepaald.
  Bij elke join begint de *frame counter* weer bij 0 (zie verderop).
* ABP: activation bij personalization: je maakt van te voren de koppeling tussen het device en het TTN-netwerk.
  De gegevens van deze koppeling, onder andere de encryptiesleutel, leg je vast in het device (meestal in de programmatekst).

In de TTN-voorbeelden gebruiken wij meestal OTAA.
Zodra een IoT-knoop aangezet wordt, verstuurt deze een join-verzoek naar het netwerk.

 Het *device address* kun je vergelijken met het (lokale) IP-adres dat je computer krijgt via DHCP.
 Dit is het adres dat gebruikt wordt voor de communicatie.
 Elke keer als je computer een IP-adres aanvraagt, kan dit een ander adres zijn: het is *dynamisch*.
 De *device EUI* kun je vergelijken met het MAC-adres van je computer.
 Dit verandert niet: het is *statisch*, aangemaakt door de fabrikant.

 Het *device address* hoeft binnen het netwerk niet uniek te zijn:
 de combinatie van device address en cryptografische sleutel identificeert het device (de IoT-knoop).

Device EUI
----------

In de meeste gevallen wijst de fabrikant het Device EUI toe.
Dit staat soms op het device gedrukt; soms kun je dit via software achterhalen.

Soms moet je zelf een EUI toewijzen.
Het TTN console kan een EUI voor je genereren:

* P.M.


Frame counter
-------------

De berichten van een device zijn opeenvolgend genummerd: de *frame counter*
geeft het nummer van het laatst ontvangen bericht aan.

Deze nummering heeft twee doelen:

1. betrouwbaarheid: je kunt zien of en hoeveel berichten er verloren geraakt zijn;
2. veiligheid: een vorig bericht kan niet door een derde partij herhaald worden ("replay attack").
   Alleen berichten met (iets) hogere nummers worden geaccepteerd door het netwerk.

Bij een "join" (OTAA) wordt de framecounter automatisch op 0 gezet.
In andere gevallen moet je soms zelf zorgen voor de framecounter-reset in het console,
bijvoorbeeld als het device door een reset weer bij 0 begint.


.. todo::

  * waarom meerdere EUIs per application?
  * genereren van device EUI

Aanmaken van een application
============================

(Dit materiaal moet opgenomen worden in IoT-1?)


Toevoegen van een device
========================

1. *registreren* van het device bij de application (https://www.thethingsnetwork.org/docs/devices/registration.html);
2. programmeren of configureren van het device, met de volgende gegevens:
    1. (als er geen hardware device EUI is): device EUI
    2. Application EUI
    3. App key
3. als je het device aanzet: "device activation", device stuurt een *join*-bericht naar het netwerk.
   Het device wordt gekoppeld aan het netwerk en krijgt een *device address*.

Deze eerste twee stappen hoef je maar één keer per device uit te voeren.
