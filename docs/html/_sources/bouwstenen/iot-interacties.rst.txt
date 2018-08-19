Interacties
===========

Client-server (HTTP)
--------------------

.. figure:: IoT-client-server-0.png
   :width: 500 px
   :align: center

   Client-server interactie (HTTP)

Deze client-server interactie is een "pull"-interactie:
de browser (client) heeft het initiatief, en "trekt" de HTML-documenten van de server.

* gevolg: server moet altijd bereikbaar zijn; client hoeft alleen gedurende de interactie verbonden te zijn.

Publish-subscribe (MQTT)
------------------------

.. figure:: MQTT.png
   :width: 500 px
   :align: center

   Publish-subscribe interactie (MQTT)

* publish/subscribe als "push" interactie
* broker moet altijd bereikbaar zijn; client niet noodzakelijk.
* (broker kan evt. berichten bufferen: client ontvangt dan achterstallige berichten?)
