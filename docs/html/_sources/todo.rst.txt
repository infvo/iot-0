****
ToDo
****

.. todo::

  MQTT1:

  * naamgeving?
  * aanpassen van HTML-title
  * toevoegen van een "clear"-button voor subscribe-venster
  * publish-to topic: actuators ipv. sensors
  * geen automatische clear van publish-venster (evt. ook clear-button).
  * publish-venster kan kleiner.
  * kunnen we subscribe-venster automatisch laten scrollen?

.. todo::

  Knoop-simulator:

  * check met welke frequentie de knoop-simulator de sensorgegevens verstuurt.
  * worden deze direct verstuurd bij het veranderen van een sensor?
      * dat is niet de bedoeling: alleen bij het ontvangen van een actuator-bericht,
        of bij het indrukken van een knop moeten de sensorwaarden verstuurd worden.
  * meesturen van een tijd of een volgnummer
  * toevoegen van lichtniveau aan simulator


.. todo::

  * duidelijk(er) maken dat mqtt niet voorkomt dat verschillende knopen onder dezelfde ID/hetzelfde topic publiceren.
    (er is een mqtt-client-id, maar hoe kun je die achterhalen, in de verschillende contexten?)

.. todolist::
