***********
Toetsvragen
***********

.. bij de inleiding.

Voorkennis
==========

.. mchoice:: vraag-VK-1
   :answer_a: Het IP-adres verandert (en het MAC-adres blijft gelijk)
   :answer_b: Het MAC-adres verandert (en het IP-adres blijft gelijk)
   :answer_c: Het IP-adres en het MAC-adres veranderen beide
   :answer_d: IP-adres en MAC-adres blijven gelijk.
   :correct: a
   :feedback_a: OK! Het IP-adres bevat een netwerk-adres: dat deel verandert in elk geval
   :feedback_b: Nee - het MAC-adres is onafhankelijk van het netwerk
   :feedback_c: Nee - het MAC-adres is onafhankelijk van het netwerk
   :feedback_d: Nee - Het IP-adres bevat een netwerk-adres: dat deel verandert in elk geval

   Als je je computer in een ander lokaal netwerk verbindt, bijvoorbeeld als je van huis naar school gaat,
   wat verandert er dan?

..

.. mchoice:: vraag1_VK-2
   :answer_a: Nee
   :answer_b: Ja
   :correct: b
   :feedback_b: OK! - elk netwerk-interface (WiFi, Ethernet) heeft een eigen IP-adres
   :feedback_a: Nee - elk netwerk-interface (WiFi, Ethernet) heeft een eigen IP-adres

   Kan je laptop-computer verschillende IP-adressen tegelijk hebben?
   (Verklaar je antwoord.)

..

.. mchoice:: vraag-VK-3
  :answer_a: Het internet is een toepassing van het web
  :answer_b: Het web is een toepassing van het internet
  :answer_c: Facebook is een toepassing van het web
  :answer_d: Het web is een toepassing van Facebook
  :correct: b, c
  :feedback_a: Het web-protocol (HTTP) gebruikt TCP (en daaronder IP).
  :feedback_b: Het web-protocol (HTTP) gebruikt TCP (en daaronder IP)
  :feedback_c: Facebook gebruikt het web (HTTP)
  :feedback_d: Facebook gebruikt het web (HTTP)

  Wat is de relatie tussen het internet, het web en Facebook?

..


Inleiding
=========

Voorbeelden
===========

.. mchoice:: vraag-VB-1
   :answer_a: Controller
   :answer_b: IoT-knoop
   :answer_c: Sensor
   :answer_d: Actuator
   :correct: c
   :feedback_a: Met een controller bestuur je een IoT-toepassing
   :feedback_b: Een IoT-knoop kan sensoren bevatten (en actuatoren)
   :feedback_c: Goed!
   :feedback_d: Met een <em>actuator</em> verander je een fysische grootheid

   Hoe heet een opnemer om een fysische grootheid, zoals de temperatuur, van een "ding" te meten?

.. mchoice:: vraag-VB-2
   :answer_a: Het internet is een toepassing van het web
   :answer_b: Het web is een toepassing van het internet
   :answer_c: Het internet of things is een toepassing van het web
   :correct: b
   :feedback_a: Nee - het web is een toepassing van het internet: HTTP maakt gebruik van TCP/IP
   :feedback_b: Ja! Het web is een toepassing van het internet: HTTP maakt gebruik van TCP/IP
   :feedback_c: Nee - het internet of things gebruikt andere protocollen (MQTT etc.) dan het web (HTTP)

   Hoe verhouden het web, het internet en het internet of things zich?
   (Hoe verhouden de verschillende protocollen zich?)

..

.. mchoice:: vraag1-VB-3
   :answer_a: een controller bestuurt de sensoren en actuatoren bij het "ding"
   :answer_b: een IoT-knoop bevat altijd sensoren en actuatoren
   :answer_c: een IoT-knoop bevat altijd een controller
   :answer_d: een IoT-knoop is altijd draadloos verbonden aan het internet
   :correct: a, c
   :feedback_a: Ja.
   :feedback_b: Een IoT-knoop kan ook alleen sensoren of actuatoren bevatten.
   :feedback_c: Ja.
   :feedback_d: Een IoT-knoop kan ook bedraad zijn

   Welke van de onderstaande uitspraken is waar?

..

.. mchoice:: vraag1-VB-4
   :answer_a: Controller
   :answer_b: Actuator
   :answer_c: Radio
   :answer_d: Gateway
   :correct: d
   :feedback_a:
   :feedback_b:
   :feedback_c: Een IoT-knoop kan ook bedraad zijn.
   :feedback_d: Ja.

   Hoe heet de verbinding tussen een IoT-knoop en het internet?

..

.. dragndrop:: dnd-ex-1
  :feedback: Zie: xxx
  :match_1: Controller ||| Besturingscomputer
  :match_2: Sensor     ||| Meet een fysische grootheid
  :match_3: Gateway    ||| Verbindt twee netwerken

  Plaats de termen bij hun beschrijving.

.. ---

..
..

.. dragndrop:: dnd-ex-3
  :feedback: Feedback that is displayed if things are incorrectly matched.
  :match_1: Muziek (mp3, AAC) ||| 200 kbyte/s
  :match_2: Temperatuur       ||| 0,01 byte/s
  :match_3: Video             ||| 5 MByte/s
  :match_4: Spraak (telefoon) ||| 5 kbyte/s

  Welke bitrate hoort (ongeveer) bij welk soort signaal?
  (Neem in het "temperatuur"-geval aan dat dit een kamerthermostaat betreft die elke 2 minuten de temperatuur meet.)


..

.. parsonsprob:: par_1_1
   :language: english

   Plaats de protocollen voor een IoT-toepassing in de juiste volgorde in de "stack".
   -----
   IoT-toepassingsprotocol
   =====
   MQTT
   =====
   Transport Control Protocol (TCP)
   =====
   Internet Protocol (IP)
   =====
   WiFi
   =====
   HTTP #distractor

..

.. mchoice:: vraag1_ya
   :answer_a: WiFi-protocol
   :answer_b: IP-protocol
   :answer_c: TCP/TLS-protocol
   :answer_d: Toepassingsprotocol
   :correct: d
   :feedback_a: Iedereen in hetzelfde WiFi-netwerk gebruikt dezelfde sleutel
   :feedback_b: (??)
   :feedback_c: De versleuteling is tussen de browser en de webserver.
   :feedback_d: De webserver kunnen het bericht niet ontsleutelen.

   Je kunt berichten versleutelen op verschillende protocol-niveaus.
   In welk geval is er sprake van end-to-end encryptie?

..

* end-to-end principe

    * protocol in eindpunten, niet in netwerk
    * bijv. http

..

.. mchoice:: vraag1_xyb
   :answer_a: WiFi
   :answer_b: Internet Protocol (IP)
   :answer_c: Transport Control Protocol (TCP)
   :answer_d: HyperText Transfer Protocol (HTTP)
   :correct: c, d
   :feedback_a: WiFi is een Link-layer protocol in het lokale netwerk
   :feedback_b: IP is het basis netwerk-procotol van het internet - in het netwerk
   :feedback_c: TCP is het end-to-end transport-protocol van het internet
   :feedback_d: HTTP is het end-to-end toepassingsprotocol van het web, op basis van het internet.

   Welke van deze protocollen zijn *end-to-end*, en vind je alleen in de eindpunten,
   en niet in het netwerk?

* een IoT-knoop verstuurt elke minuut het aantal WiFi-gebruikers in de directe omgeving (max.255).

    * wat is de  bitrate?
    * maakt het een groot verschil of de latency 0,1s, 10s of 60s is?
    * is dit een privacy-risico?
    * op welke manieren zou je dit gegeven kunnen gebruiken?
