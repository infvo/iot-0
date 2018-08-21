*****
Eisen
*****

.. bij de Inleiding

.. rubric:: Het Internet of Things stelt eigen eisen aan de internet-communicatie

Het Internet of Things verbindt de fysieke wereld van de "dingen" met de virtuele wereld van het internet.
Voor de verbinding met de fysieke wereld van een "ding" zorgen sensoren en actuatoren,
als onderdeel van een IoT-knoop gekoppeld aan dat "ding".
Deze IoT-knoop is vaak draadloos verbonden met het internet, bijvoorbeeld via een *gateway*.
De IoT-knopen en gateways vormen de rand (*edge*) van het Internet of Things.

In deze module ligt de focus op deze edge: de IoT-knopen en gateways,
en de communicatie daartussen.
De eisen aan deze edge en de bijbehorende communicatie volgen voor een deel uit de fysieke eigenschappen van de toepassingen,
zoals de gebruiker deze ervaart.
Met andere woorden: je kunt als gebruiker zonder kennis van de IoT-technologie beschrijven aan welke eisen deze edge(-communicatie) moet voldoen.
Deze eisen hebben gevolgen voor de technologische keuzes die je voor die edge maakt.

We beschrijven in dit gedeelte de eisen die van belang zijn voor deze keuzes.
Later behandelen we hoe je op grond van deze eisen de passende technologie kunt kiezen.

.. rubric:: snelheid: bitrate

De eerste soort eis heeft te maken met de snelheid van communicatie, gerekend in bits per seconde (bitrate).
Veel sensoren en actuatoren, bijvoorbeeld voor temperatuur of lichtniveau, werken met waarden van 1 of 2 bytes.
Bovendien hoef je die waarden vaak maar enkele keren per uur te versturen.

Op basis van de sensoren en actuatoren die je gebruikt,
en van de eigenschappen van het "ding" (hoe snel verandert dit?),
kun je dan een inschatting maken van het aantal berichten dat je per uur nodig hebt,
en van hun grootte (in bytes).

  Er zijn ook sensoren en actuatoren die een veel grotere bandbreedte nodig hebben.
  Voorbeelden daarvan zijn camera's en microfoons, displays en luidsprekers.
  In deze module laten we dergelijke sensoren en actuatoren buiten beschouwing:
  "streaming" audio en video vormen een aparte categorie toepassingen.

.. rubric:: latency

De tweede soort eis heeft ook met snelheid te maken.
Hierbij gaat het om de latency: de tijd dat "iets onder water is".
Bij communicatie is de latency de tijd tussen het versturen en het ontvangen van een bericht.
Meer in het algemeen gaat het bij het IoT-toepassingen om de totale reactietijd.
Wat is de minimale reactietijd voor een reactie op een gebeurtenis?
Denk bijvoorbeeld aan een IoT-toepassing voor het besturen van de verlichting in je huis:
als je op de knop drukt om de verlichting in te schakelen,
dan wil je "bijna onmiddellijk" (binnen 200 ms) de lampen zien reageren.
Maar als je de verlichting automatisch of op afstand in- of uitschakelt,
dan mag die reactie misschien wel na enkele seconden plaatsvinden.

Voor IoT-toepassingen met alleen sensoren ("monitoring") is vaak een latency van enkele seconden acceptabel.
Als er actuatoren bij betrokken zijn, maakt het vaak een groot verschil of deze lokaal of op afstand bestuurd worden;
en of deze besturing automatisch of door mensen gebeurt.

.. rubric:: betrouwbaarheid

Sommige fysische processen verlopen langzaam: denk bijvoorbeeld aan de verandering van de temperatuur in een woonkamer.
Dit betekent dat je de sensoren die dit proces bewaken niet zo vaak hoeft uit te lezen.
Bovendien mag je best eens een meting missen: hooguit heeft dat tot gevolg dat de verwarming of de airconditioning een paar minuten later aan gaat.
We kunnen dan voor de communicatie volstaan met "best effort".
Het is niet nodig om de ontvangst van elk bericht te bevestigen.

Eén van de vragen die je als gebruiker moet onderzoeken is dan ook:
hoe betrouwbaar moet de communicatie zijn?
Hoe erg is het als er eens een meting of een aansturing verloren gaat?

.. rubric:: energie en mobiliteit

De "dingen" waar we in het IoT over spreken verschillen sterk.
Soms hebben we te maken met grote apparaten met een permanente stroomvoorziening.
In zo'n geval speelt de grootte, het gewicht en het energieverbruik van de IoT-knoop niet zo'n grote rol.
In andere gevallen hebben we te maken met kleine en mobiele "dingen" (bijvoorbeeld een huisdier):
dan spelen deze factoren wel een grote rol.

Bij mobiele "dingen" speelt de energiezoorziening een grote rol:
hiervoor gebruiken we meestal een batterij.
Bij voorkeur gebruiken we een kleine batterij of accu,
die we vaak hoeven te vervangen of op te laden.
Dit laatste is vooral van belang als we met veel IoT-knopen te maken hebben:
het is niet handig als je regelmatig honderden batterijen in de IoT-knopen in je huis moet vervangen.

Dit geeft de volgende vragen voor de gebruiker:

* hoe groot en zwaar mag de IoT-knoop zijn?
* hoe mobiel moet deze knoop zijn?
* over welk gebied is deze knoop mobiel?
  Hoe groot moet het bereik van een radio zijn om dit "ding" te kunnen volgen?
* hoe groot en zwaar mag een batterij zijn?
* wat moet de minimale levensduur van een batterijlading zijn?
* hoe vaak mag je die batterij opladen of omwisselen?

.. rubric:: veiligheid

.. todo::

  * eisen aan de security (en privacy)

* veiligheid is essentieel - zowel vanwege de veiligheid van de "dingen" als van de privacy van mensen in hun omgeving.
    * vooral bij besturingstoepassingen is dit van belang
    * (opmerking: het IoT kan ook bijdragen aan de fysieke veiligheid...)


.. rubric:: Verschillen tussen IoT en het web

Het IoT is veel heterogener dan het web, en veel minder gestandaardiseerd.
Dit betekent dat we veel verschillende oplossingen en vormen van de IoT-keten tegenkomen.

Deze IoT-keten heeft andere karakteristieken dan de keten voor het web:

* de sensordata van een enkele IoT-knoop bestaan uit weinig bytes - veel minder dan de gemiddelde webpagina, en nog veel minder dan nodig voor audio en video;
* het aantal IoT-knopen in een toepassing kun erg groot zijn - veel groter dan bijvoorbeeld het aantal gebruikers van een website;
* de fysieke wereld stelt soms absolute grenzen aan de latency; in de virtuele wereld is deze latency wat minder van belang;
* de resultaten van het web worden aan mensen gepresenteerd, die op basis daarvan beslissingen nemen;
* de resultaten van het IoT worden soms gebruikt om direct "dingen" te besturen, zonder menselijke tussenkomst (M2M, machine to machine).
* de verschillen tussen "dingen" zijn veel groter dan de verschillen tussen mensen: het IoT is veel heterogener dan het web.
