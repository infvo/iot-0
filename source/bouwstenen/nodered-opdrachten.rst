******************
NodeRed opdrachten
******************

In deze opdrachten maak je kennis met NodeRed.
Je gebruikt NodeRed voor een eenvoudige IoT-toepassing: een dashboard van een IoT-knoop.
Met dit dashboard kun je op een computer of op een smartphone de toestand van de IoT-knoop waarnemen,
en de IoT-knoop besturen.

Met NodeRed kun je allerlei protocollen, diensten en besturingen aan elkaar koppelen, op een grafische manier.
Hiermee kun je je IoT-keten samenstellen van sensoren en actuatoren tot Data Science- en Artificial Intelligence-diensten.

.. admonition:: Wat heb je nodig?

  * NodeRed-installatie
      * bijvoorbeeld: FRED - https://fred.sensetecnic.com (gratis versie)
  * een IoT-knoop, bijvoorbeeld:
      * een gesimuleerde IoT-knoop
      * een IoT-knoop "elders"
      * een eigen hardware IoT-knoop, bijvoorbeeld een ESP8266-knoop met het programma ``esp8266-node-0``
        (zie https://infvo.github.io/iot-1/html/combinatie.html)

Voorbereiding
=============

Lees als voorbereiding op de NodeRed-opdrachten eerst onderstaande tekst.
Doe dit bij voorkeur met een geopende versie van NodeRed,
zodat je de verschillende onderdelen direct kunt vinden en uitproberen.

**Nodes en knopen**: om verwarring te voorkomen gebruiken we in deze opdrachten het woord "knoop" voor een IoT-knoop,
en "node" voor een NodeRed-node.

Nodes en flows
--------------

.. figure:: Nodered-hello.png
   :width: 600 px
   :align: center

   NodeRed http flow-voorbeeld

Een *flow* in NodeRed bestaat uit een netwerk van *nodes* en *verbindingen*.
Het aansluitpunt (bolletje) aan de linkerkant van een node is de input.
Een node zonder aansluiting links is een *input-node*, met een externe input, bijvoorbeeld een http-request.
De outputs staan aan de rechterzijde van de node.
Een node zonder aansluiting rechts is een *output-node*, met een externe output, bijvoorbeeld een http-response.

Een NodeRed-toepassing kan uit meerdere flows bestaan: elke flow heeft een eigen pagina (tab).

+--------------------+------------------+------------------+
| **figuur**         | **naam**         | **soort node**   |
+--------------------+------------------+------------------+
| |http-input-node|  | http-input-node  |  input           |
+--------------------+------------------+------------------+
| |http-output-node| | http-output-node |  output          |
+--------------------+------------------+------------------+
| |template-node|    | template-node    |  in-out          |
+--------------------+------------------+------------------+

.. |http-input-node| image:: nodered-http-input-node.png
.. |http-output-node| image:: nodered-http-output-node.png
.. |template-node| image:: nodered-template-node.png

**Hoe werkt een flow?**
Als een node een bericht (message) krijgt via de input,
dan voert deze node daarop een bewerking uit,
en genereert één of meer messages naar de output(s).
Deze output is weer verbonden  met de input van een andere node;
of de node is een output-node, met een externe output.

*Voor het bovenstaande flow-voorbeeld*: (i) de http-input-node ontvangt een http-request als
de http-method gelijk is aan ``get`` en het URL-pad gelijk is aan ``/hello``.
Deze http-input-node stuurt dan een message met dit request naar
(ii) de template-node ``hello.html``.
Deze genereert de bijbehorende output: een html-document,
en stuurt een message met dit document naar
(iii) de http-output-node, die uit de message de bijbehorende response samenstelt.
Deze node stuurt de response naar de afzender van het http-request.


NodeRed UI
----------

.. figure:: nodered-ui.png
   :width: 700 px
   :align: center

   NodeRed user interface

In het NodeRed user interface vind je helemaal bovenin de *Deploy-knop* en het *hamburgermenu* (drie streepjes).
Daaronder, van links naar rechts:

* het node-palette. Uit dit palette selecteer je nodes die je wilt gebruiken.
  Er zijn onder andere input-nodes (met een bolletje rechts),
  output-nodes (met een bolletje links), en function-nodes (met links en rechts een bolletje).
  Er zijn nodes voor allerlei protocollen, bijvoorbeeld: HTTP, TCP, MQTT.
  Er zijn ook nodes voor communicatie met toepassingen als bijvoorbeeld Twitter.
* het flow-gedeelte. Dit bestaat uit verschillende flow-tabs.
    * Met "+" maak je een nieuwe flow-tab aan.
    * Door double-click op de flow-naam krijg je het configuratie-venster voor deze flow te zien.
      Hiermee kun je de flow hernoemen, tijdelijk uitschakelen (disable), of verwijderen (delete).
* de info/debug/dashboard-sidebar
    * de info-tab geeft informatie over de geselecteerde node in het flow-gedeelte.
    * de debug-tab geeft de debug-output van de huidige flow, of van alle flows.
    * via de dashboard-tab kun je de UI-instellingen van het dashboard veranderen.
* (alleen FRED) FRED-sidebar (links)
    * met het pijltje linksonder maak je deze (on)zichtbaar

De volgende oefeningen zijn bedoeld om vertrouwd te raken met het user interface.
Deze oefeningen hebben geen effect op de flows zelf.

.. rubric:: Oefenen met het NodeRed interface

* klik op het hamburgermenu, en zoek de instellingen voor:
    * het (on)zichtbaar maken van de sidebar (info/debug/dashboard)
    * het importeren van flows (vanuit het Clipboard)
    * het zichtbaar maken van de tab met configuratie-nodes
* zoek in het palette:
    * HTTP input-node
    * MQTT output-node
    * Twitter output-node
* voeg een nieuwe flow-tab toe (via "+")
    * hernoem deze tot "Test-flow"
* (alleen voor FRED):
    * maak de FRED-sidebar (links) onzichtbaar en weer zichtbaar

Dashboard-nodes installeren
---------------------------

.. admonition:: Installeren van dashboard-nodes

  De dashboard-nodes zijn niet altijd beschikbaar in het node-palet links.

  **Als je FRED gebruikt**, dan installeer je de dashboard-nodes als volgt:

  * selecteer in de FRED-zijbalk (helemaal links): Tools-> add or remove nodes
  * type in het zoekveld: dashboard
  * vink aan: *Dashboard (a set of dashboard nodes for NodeRed)*.

  **Voor een normale NodeRed-installatie** gebruik je de volgende stappen:

  * selecteer hamburger-menu (rechts) -> Manage Palette
  * selecteer de tab *Install*
  * type in het zoekveld: dashboard
  * klik op "install" voor *node-red-dashboard* *(A set of dashboard nodes for Node-RED)*
  * na deze installatie zijn de nodes in het palet links beschikbaar.


1. Eerste flow
==============

Met deze eerste flow kun je zien of alles werkt:

.. figure:: Nodered-flow1.png
   :width: 500 px
   :align: center

   NodeRed: eerste flow

Hiervoor gebruik je de volgende nodes:

+----------------+---------------+------------------+
| **figuur**     | **naam**      | **soort node**   |
+----------------+---------------+------------------+
| |inject-node|  | inject-node   |  input           |
+----------------+---------------+------------------+
| |debug-node|   | debug-node    |  output          |
+----------------+---------------+------------------+

.. |inject-node| image:: inject-node.png
.. |debug-node| image:: debug-node.png

.. rubric:: Opdracht 1.1

Voer de onderstaande opdrachten uit in een lege (flow)tab in NodeRed.

* sleep een inject-node vanuit de lijst met nodes links naar het lege vlak in het midden
* plaats op dezelfde manier een debug-node;
* verbind de output (rechts) van de inject-node met de input (links) van de debug-node;
* activeer deze flow (rechts boven: **Deploy**);
* selecteer de debug-tab (rechts);
* test deze flow, door op het knopje links op de input-node ("timestamp") te klikken.

Als het goed is, krijg je in het debug-venster rechts nu de output van deze flow te zien.
Elke keer als je op de input-node klikt, genereert deze een timestamp-event.

.. rubric:: Opdracht 1.2

Voor onderstaande opdrachten uit; test de uitwerking (na "Deploy") via de debug-tab.

* verander de configuratie van de inject-knoop: zorg ervoor dat deze elke 10 seconden een timestamp oplevert.
    * double-click op een knoop geeft het configuratie-venster;
    * bewaar de nieuwe configuratie via de "Done"-knop.
* verander de configuratie van de inject-knoop: zorg ervoor dat deze een tekst levert als inhoud van het bericht (payload).
* verbind meerdere inject-knopen met herhalende berichten met dezelfde debug-knoop.

2. Een IoT-dashboard
====================

Als voorbeeld van een complete flow gebruiken we een dashboard voor een IoT-knoop.
Dit dashboard maakt de sensorwaarden van de IoT-knoop zichtbaar;
je kunt hiermee ook de LED van de IoT-knoop aansturen.

.. figure:: nodered-dashboard-flow-1.png
   :width: 700 px
   :align: center

   NodeRed dashboard flow

De flow zelf, in JSON formaat (voor importeren in NodeRed), vind je:

* op GitHub: `NodeRed dashboard (flow 1) <https://gist.github.com/eelcodijkstra/1f5e6bc7cab88e7fd230cdee8cb94d73>`_
* op NodeRed-library: `ieni2018-iot-flow-1 <https://flows.nodered.org/flow/1f5e6bc7cab88e7fd230cdee8cb94d73>`_

.. rubric:: Opdracht 2.1

* importeer de dashboard-flow:
    * selecteer de flow-tekst (in JSON-formaat), en kopieer deze naar het Clipboard
      (via "Copy" van het operating system).
    * in NodeRed: selecteer hamburgenmenu->Import->Clipboard
    * "Paste" de inhoud van het Clipboard in het input-venster.
    * "Import"
    * je krijgt nu een nieuwe flow met als naam (in de tab): IoT-flow-1
* selecteer in deze flow de MQTT-input-node, en configureer deze (double-click):
    * selecteer bij "Server": ``infvopedia.nl`` (met port 1883)
    * als deze niet beschikbaar is: selecteer bij "Server": ``Add new mqtt-broker...``
        * klik op het potloodsymbool rechts daarvan
        * vul in bij "Server": ``infvopedia.nl`` (met port 1883)
        * klik op "Add"
    * klik op het potloodsymbool rechts van "infvopedia.nl";
      je krijgt nu de broker-instellingen te zien;
    * selecteer de tab "Security", en *vul de opgegeven gebruikersnaam en wachtwoord in*
    * klik "Update" (voor de Server-instellingen)
    * klik "Done" (voor de instellingen van de MQTT-input-node)
* configureer de MQTT-output-node (selecteer en double-click):
    * selecteer bij "Server": ``infvopedia.nl:1883``
    * "Done"
* je krijgt nu het dashboard van de node ``e0f1``.
    * in de debug-tab worden de mqtt-berichten getoond
    * het dashboard krijg je via: dasboard-tab, hokje-met-pijltje rechts boven.
    * het dashboard komt dan in een apart browser-venster.
* voor het aanpassen aan een eigen node:
    * configureer de MQTT-input-node, en verander het "topic":
    * vervang ``e0f1`` door de ID van je eigen node
    * "Done"
* idem, voor de MQTT-output-node.

3. Automatiseren
================

Via NodeRed kun je allerlei protocollen en toepassingen koppelen.
Je kunt ook allerlei zaken automatiseren, bijvoorbeeld een lamp inschakelen als je thuiskomt.

Een eenvoudige automatisering is het laten knipperen van LED-0 op de IoT-knoop.

.. rubric:: Opdracht 3.1

Maak een NodeRed-flow waarmee je LED-0 van een (gesimuleerde) IoT-knoop laat knipperen.
Begin met de eenvoudige flow van Opdracht 1, en breid deze later uit met een MQTT-output-node.
Vergeet niet aan het eind van elke opdracht de flow te activeren ("Deploy");
controleer bij elke stap of het werkt.

1. Door op de knop links op de debug-node te klikken "injecteer" je een timestamp in de flow:
   de huidige tijd, als geheel getal. Via de debug-node krijg je dit te zien in het debug-venster.
2. Met de knop rechts op de debug-node kun je deze node tijdelijk uitschakelen.
3. Laat de inject-node automatisch werken. Configureer deze node (dubbel-klik op de node):
   zet "Repeat" van "none" naar "interval" (every 10 seconds). Bewaar deze aanpassing ("Done" knop).
4. Laat de inject-node een ander soort waarde injecteren. Configureer deze node:
   selecteer als "payload" (inhoud van het bericht): string, en vul als waarde in: "Hallo wereld".
   Bewaar deze aanpassing.
5. Idem, maar nu met de JSON-waarde: ``{"0": {"dOut": 1}}`` (Mogelijk komt deze waarde je bekend voor.)
   Tip: bij het invoeren van een JSON-waarde kun je de JSON-editor gebruiken,
   via de ``...`` rechts in het edit-venster.
6. Maak een kopie van de inject-node, en laat deze als JSON-waarde genereren: ``{"0": {"dOut": 0}}``.
   Verbind deze inject-node met dezelfde debug-node.
7. Zorg ervoor dat de tweede inject-node 5 seconden later begint dan de eerste:
   ``inject once after 5 seconds`` (in de node-configuration).
   Als het goed is krijg je nu om de 5 seconden een bericht te zien.
8. Voeg een MQTT output-node toe (als in het dashboard).
   Verbind de output van beide inject-nodes met de MQTT output-node.
   Configureer de MQTT output-node (zoals bij de dashboard-opdracht);
   gebruik als topic: ``node/xxxx/actuators``,
   waarin ``xxxx`` de ID van het IOT-knoop is.

Als het goed is knippert de LED van je IoT-knoop nu: 5 seconden aan, 5 uit, enz.

Deze manier van werken is typisch voor NodeRed: je bouwt een flow beetje voor beetje op,
waarbij je in het begin veel gebruik maakt van inject- en debug-nodes.
Je test hiermee elke stap.
Deze nodes kun je laten zitten tijdens het gebruik:
een debug-node kun je eenvoudig uitschakelen als je deze even niet nodig hebt.

Opmerkingen:

* je kunt in het debug-venster aangeven dat je alleen de "current flow" wilt zien;
* je kunt het debug-venster leeg maken via het vuilnisbakje (rechts boven).

NodeRed FAQ
===========

.. rubric:: hoe (de)activeer ik een hele flow?

Door double-click op de flow-tab krijg je het configuratievenster voor deze flow te zien.
Je kunt de flow (de)activeren via Status (Enabled of niet).
Het is soms handig om een flow te deactiveren, als deze andere flows in de weg zit.
Of als dit een test-flow is die je zo nu en dan nodig hebt.

Je kunt de flow (tab) hier ook een andere naam geven, of helemaal verwijderen.

.. rubric:: hoe maak ik de info/debug-sidebar (on)zichtbaar?

Via het hamburgermenu->View->Show sidebar.

.. rubric:: hoe maak ik de FRED sidebar (on)zichtbaar?

Deze sidebar kun je (on)zichtbaar maken via het pijltje in de hoek linksonder.

.. rubric:: hoe verwijder ik een hele flow?

Double-click op de flow tab: klik in het configuratie-venster op Delete,
links boven.
Door "Deploy" maak je de aangepaste flows actief.

.. rubric:: hoe installeer ik extra nodes?

Er zijn veel soorten nodes beschikbaar voor allerlei protocollen en toepassingen.
In de `NodeRed library <https://flows.nodered.org/>`_ vind je veel voorbeelden.

Bij een standaard NodeRed installatie kun je extra nodes meestal installeren via hamburgermenu->Manage palette.
Voor een uitgebreidere uitleg, zie https://nodered.org/docs/getting-started/adding-nodes.

In FRED kun je nodes installeren via de FRED sidebar, helemaal links.
Deze sidebar kun je (on)zichtbaar maken via het pijltje in de hoek linksonder.

Voorbeeld: installeren van nodes voor TTN (THe Things Network):

* selecteer Tools->Add or Remove Nodes
* selecteer IoT
* zet het vinkje bij Ttn (onderaan)

Na herstarten van de server verschijnen de TTN-nodes nu in het palette.

.. rubric:: waar vind ik de verborgen nodes?

NodeRed gebruikt *configuration nodes* voor bijvoorbeeld de MQTT-server-instellingen,
en voor de dashboard-instellingen.
Deze *configuration nodes* kun je zichtbaar maken via hamburgermenu->Configuration nodes.

In de gratis versie van FRED heb je een beperking van maximaal 50 nodes.
Daar tellen ook de verborgen nodes in mee.

.. rubric:: de MQTT-nodes blijven hangen in de "connecting" toestand

Mogelijk ontbreken de security-gegevens (username/password van de MQTT broker).
Double-click op de MQTT-node, en klik vervolgens op het potloodje naast de naam van de broker.
In de configuratie van de broker selecteer je de tab "security", en vult daar de username/password-combinatie van je broker in.
