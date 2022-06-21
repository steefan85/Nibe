# Nibepi mit MQTT

## Voraussetzungen
- NibePi ist eingerichtet und läuft
- ein MQTT-Server ist eingerichtet und läuft

## Register lesen und via MQTT senden

### NibePi Register einrichten
- NibePi UI aufrufen: http://nibepi:1880/ui/
- Bei den drei Waagerechten Linien unter "Datahantering" können die Modbus-Register definiert werden (dies kann auch mit dem Nibe Modbus-Manager erfolgen), dazu die Regsiter mit einem Haken auswählen
- Links unter "Register Management" sind alle verfügbaren Register gelistet
- Rechts unter "Data Management" sind die ausgewählten Register aufgelistet; auf diese hat man lesend und ggf. schreibend Zugriff via NibePi

![Datahantering](https://user-images.githubusercontent.com/24730529/174718889-8dfa3f8d-10b9-4181-bd74-88a958c8b901.jpg)

![Register](https://user-images.githubusercontent.com/24730529/174720787-4e0be017-cc19-4724-bd19-00249ef7b804.jpg)

- Für dieses Beispiel verwenden wir 40072, BF14 Flow. Wir merken uns die Registernummer

### Node-red einrichten
- NodeRED auf dem Pi aufrufen: http://nibepi:1880/
- Idealerweise einen neuen Flow anlegen, hierzu auf das "+" klicken

![Flow_neu](https://user-images.githubusercontent.com/24730529/174719831-dd8ab4f5-6766-423e-8c2d-1e14ec32b5a1.jpg)

#### Nibe-input Node hinzufügen

- Unter "Eingabe" den Nibe-input Node suchen und in den Flow hinein ziehen.
- 
![Node-Red_Nibeinput](https://user-images.githubusercontent.com/24730529/174719787-1bd9734c-9373-4183-b574-8dff601bc22d.jpg)

- Den neu erstellten Node doppelt klicken.
  - Die Register-Nummer konfigurieren (40072, siehe oben).
  - Den Server belassen wir auf Standard, in meinem Fall /dev/tty/AMA0
- Jetzt haben wir lesend Zugriff auf das Register.

![Nibe_input](https://user-images.githubusercontent.com/24730529/174720111-e494026d-99fd-4358-bf91-9b4727ba1665.jpg)

#### MQTT-output Node hinzufügen

- Unter "Network" den MQTT-output Node suchen und in den Flow hinein ziehen.

![Node-Red_MQTToutput](https://user-images.githubusercontent.com/24730529/174720567-17b2b452-201a-4132-9d59-a29dad523447.jpg)

- Im Flow den neu erstellten Node mit dem Nibe-input Node verbinden.
- Den neu erstellen Node doppelt klicken.

![MQTT_out_Server_konf](https://user-images.githubusercontent.com/24730529/174721757-c5048518-bbbc-4d9f-a48a-da038ba9f69f.jpg)

- Den Server konfigurieren, hierzu unter "Server" auf den Stift klicken
- Unter "Verbindung" muss euer MQTT-Server eingetragen werden, IP-Adresse und Port.
- Der Name kann frei festgelegt werden.

![MQTT_out_Server](https://user-images.githubusercontent.com/24730529/174721156-26ebe1e3-71b7-47a3-8263-b15207173b5c.jpg)

- Unter "Sicherheit" müssen die MQTT-Zugangsdaten hinterlegt werden

![MQTT_Verbindung](https://user-images.githubusercontent.com/24730529/174721597-d5636118-4faf-4275-a782-4b084cf7952e.jpg)

- "Aktualisieren klicken.
- Zurück in "Properties" muss jetzt noch das MQTT-Topic festgelegt werden
- Der "Name" kann frei gewählt werden.

![MQTT_out_Topic](https://user-images.githubusercontent.com/24730529/174721984-81b79b79-70b9-48db-8981-5cec0e252f8e.jpg)

### Deploy

- Die Konfiguration ist jetzt abgeschlossen. Es sollte etwa so aussehen (der güne Debug-Node ist nur für Testzwecke)

![Flow_40072](https://user-images.githubusercontent.com/24730529/174722263-0f2dff89-aa1a-47e7-8275-1c18151e435c.jpg)

- Damit der Flow ausgeführt wird, müsst ihr oben rechts in der Ecke noch auf "deploy" klicken.

![Deploy](https://user-images.githubusercontent.com/24730529/174722364-94de6dfa-5695-4a21-86ff-26717cd0a4e7.jpg)

## Register via MQTT schreiben

- Sofern das entsprechende Register geschrieben werden kann (R/W) funktioniert dieses ebenfalls in NodeRED
- Der Ablauf ist ähnlich; es werden allerdings der "mqtt in" sowie der "Nibe output" Node benötigt

![MQTT_send](https://user-images.githubusercontent.com/24730529/174723273-80b1b664-30b3-4714-8088-3f96fc986f96.jpg)


