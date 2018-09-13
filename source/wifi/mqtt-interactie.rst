Publish-subscribe
=================

MQTT is een zogenaamd *publish-subscribe* protocol.
De communicatie vindt plaats tussen clients, met tussenkomst van een broker (makelaar).
De broker heeft alleen als taak om berichten door te sturen:

* een client kan zich bij de broker abonneren (*subscribe*) op de berichten van een bepaald onderwerp (*topic*);
* een client kan een bericht van een bepaald onderwerp publiceren (*publish*).
  De broker stuurt dit bericht dan door naar alle clients zich op dit onderwerp geabonneerd hebben.
* een client kan zowel *publisher* als *subscriber* zijn.

Een client "pusht" een bericht naar de broker;
de broker "pusht" dit bericht naar de geabonneerde clients.
Publish-subscribe is een *symmetrische push*-interactie.

  Bij een client-server interactie zoals HTTP ligt het initiatief bij de client.
  Een web-client (browser) "trekt" (*pull*) de documenten van de server.

Publish-subscribe geeft een *losse koppeling* tussen de clients:
de clients communiceren via een topic, en weten niet welke clients aan dat topic gekoppeld zijn.
In het bijzonder weet een publisher niet welke clients op het topic geabonneerd zijn.

  .. figure:: MQTT.png
     :width: 500 px
     :align: center

     MQTT: publish-subscribe

In de figuur publiceert client A bericht M met topic T.
De broker stuurt dit bericht door (*push*) naar clients B en D,
die zich eerder met "subscribe(T)" op topic T geabonneerd hebben.
Client C heeft zich niet geabonneerd op topic T, en ontvangt bericht M dus niet.

Op een volgend moment kan client B een bericht publiceren met een topic waar bijvoorbeeld A, C en D op geabonneerd zijn.
Client B is dan zowel *publisher* als *subscriber*.

Ook in andere contexten wordt de Publish-Subscribe-interactie gebruikt,
onder andere met allerlei soorten *Message brokers*.

Zie:

* https://en.wikipedia.org/wiki/Publish–subscribe_pattern
* https://en.wikipedia.org/wiki/Message_broker
