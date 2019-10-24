# Veralteter Fallblatt-Code

Veralteter Code zur Steuerung von Fallblattanzeigen der Deutschen Bahn für ESP32-Mikrocontroller bzw. Android-Geräte.

## Code

### ESP32
Der Code im Ordner /esp in diesem Repository ist für die ESP32-Plattform und steuert ein Fallblattmodul mit Hilfe der oben beschriebenen Pins an. Der Microcontroller unterstützt WLAN und Bluetooth, das Fallblattmodul kann per Bluetooth oder durch das Senden von HTTP-Requests an einen Webserver auf dem Controller angesteuert werden.

### Android
Unter /android liegt eine App, die das Modul über seine serielle Bluetooth-Schnittstelle kontaktiert und fernsteuert.

### Befehle
#### Bluetooth (serielle Schnittstelle)

Befehl | Bedeutung | Antwort
------------ | ------- | ------
WHOAREYOU | Ping | SPLITFLAP
FLAP # | Springt zum Blatt # | FLAP # oder INVALID COMMAND
REBOOT | Neustart | REBOOT
SETWIFI [SSID] [PASS] | Verbindet sich mit einem WLAN (Einstellungen bleiben nach dem Neustart erhalten) | WIFI [SSID] [PASS] oder COMMAND EXECUTION FAILED
GETWIFI | Gibt die WLAN-Einstellungen zurück | WIFI [SSID] [PASS]
GETIP | Gibt die IP zurück, die dem Gerät per DHCP zugewiesen wurde | IP [IP] oder IP NONE
PULL | Aktiviert den Pull-Modus (Deaktivierung durch Sprung zu festem Blatt) | PULL
GETPULLSTATUS | Status des Pull-Modus | [B1] [B2] (B1 und B2 sind entweder 0 oder 1, B1 = Pull-Modus aktiv, B2 = WLAN verbunden)
SETPULLSERVER [SRV] [ADR] | Setzt den Server für den Pull-Modus | SERVER [SRV] [ADR]
GETPULLSERVER | Gibt den eingestellten Server für den Pull-Modus zurück | SERVER [SRV] [ADR]
  
Falls keines der obigen Kommandos erkannt werden konnte, wird UNKNOWN COMMAND zurückgegeben. Jedes Kommando muss (nur) mit einem Zeilenumbruch (0x0A) abgeschlossen werden. Genauso werden die Antworten so terminiert.

Der Pull-Modus holt sich eine Textdatei von einem Server und parst diese. Es wird erwartet, dass in dieser Datei
lediglich eine Zahl steht, die das anzusteuernde Blatt angibt. Möchte man z. B. von http://testserver.to/test.php das anzusteuernde Blatt holen, so setzt man [SRV] = testserver.to und [ADR] = /test.php

  
#### HTTP-Server

Seitenaufruf (GET) | Bedeutung
------------ | -------------
/FLAP/# | Springt zum Blatt #
/PULL   | Aktiviert den HTTP-Pull-Modus
/REBOOT | Neustart