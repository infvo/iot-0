*************
LoRaWan-keten
*************

.. figure:: IoT-publieke-gateway-1.png
  :width: 600px
  :align: center

  LoRaWan-keten met publieke gateway

* lora: best-effort pakketcommunicatie
* pakket:
* adressering:
* payload: zo compact mogelijk; binaire codering
* volgnummers: (detectie van verloren pakket)
* gateway:
    * protocol-conversie
    * metadata
    * onderhandeling met IoT-knoop
* server:
    * application:
    * device: (IoT-knoop)
* toepassing: (bijv. dashboard)
    * communicatie via mqtt
    * hoe gebeurt authenticatie van gebruiker?
