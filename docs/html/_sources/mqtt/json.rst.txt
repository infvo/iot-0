****
JSON
****

In dit hoofdstuk gebruiken we MQTT voor het bewaken en besturen van een IoT-knoop.
De communicatie tussen de IoT-knoop en de toepassing (app), via de MQTT-broker,
vindt plaats in de vorm van JSON-berichten.
Het JSON-formaat is voor het IoT wat HTML is voor het web.
We leggen hieronder eerst de principes van JSON uit.
In de opdracht gebruik je JSON voor het aansturen van de IoT-knoop.

JSON staat voor "JavaScript Object Notatie".
Een JSON-document is een tekstdocument: een leesbare vorm van een JavaScript-object.
Een object bestaat uit een aantal *naam: waarde-paren*.
Een waarde kan een enkelvoudige waarde zijn, bijvoorbeeld een getal, een string, of een boolean.
Het kan ook een samengestelde waarde zijn: een object of een array.
In JavaScript gebruiken we de notatie: ``naam: waarde``.
In JSON staat de naam tussen dubbele quotes: ``"naam": waarde``.

We geven in de onderstaande voorbeelden de JavaScript-objecten en de bijbehorende JSON-notatie.

.. code-block:: javascript

  {temp: 21, press: 1015, id: "e4c7"}

.. code-block:: json

  {"temp":123,"press":1012, "id": "e4c7"}

.. todo::

  * meer JSON-voorbeelden

Je kunt een JSON-document eenvoudig omzetten in een JavaScript-object, en omgekeerd:

* ``obj = JSON.parse(str);``
* ``str = JSON.stringify(obj);``

Niet alle JavaScript-objecten kun je in JSON omzetten:

* objecten met functie-waarden
* objecten met onderlinge verwijzingen die een lus (cykel) vormen.

Veel programmeertalen hebben functies om JSON-objecten te verwerken.

* Python: https://docs.python.org/3/library/json.html
* Arduino: https://arduinojson.org/

.. admonition:: JSON in het web

  Ook het web gebruikt het JSON-formaat, als onderdeel van AJAX.
  JavaScript-functies in een app communiceren vanuit de browser met de server,
  met (naar verhouding) kleine hoeveelheden data.
  Deze communicatie gebruikt het compacte en leesbare JSON-formaat, in plaats van complete HTML-documenten.

.. rubric:: Links

* referentie: https://www.json.org/
* referentie: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
* tutorial: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON
