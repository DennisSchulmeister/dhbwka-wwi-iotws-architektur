Deviceseitiger Softwarestack
============================

Dieses Verzeichnis enthält die Quellcodes und Softwarekomponenten für die IoT-Devices.
Jede Komponente besitzt ihren eigenen Docker-Container, so dass sie mit dem Balena CLI
bzw. der Balena Cloud auf die Devices verteilt werden kann.

 * `redis`: Zentraler Redis-Server
 * `sensor`: Python-Programm zum Auslesen der Sensordaten und Ablage in Redis
 * `startstopbutton`: Python-Programm zum Starten und Stoppen der Sensormessungen
 * `mqtthandler`: Python-Programm zum Versand der Sensordaten via MQTT
 * `grafana`: Lokales Grafana-Dashboard zur Überwachung der Devices

Wichtiger Hinweis
=================

Der `startstopbutton` verwendet standardmäßig den GPIO-Pin 2. Wenn Sie diesen
Pin beispielsweise zur Anbindung eines I²C-Sensors nutzen wollen, denken Sie
daran, den Pin des `startstopbutton` zu ändern oder den Service gänzlich zu
deaktivieren. Ansonsten kann es passieren, dass der Befehl `i2cdetect` lauter
nicht vorhandene Sensoren erkennt und der Sensor nicht funktioniert.

So sollte die Ausgabe von `i2cdetect` richtig aussehen:

```
pi@raspberrypi ~ $ sudo i2cdetect -y 1
 0 1 2 3 4 5 6 7 8 9 a b c d e f
00: -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- 68 -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

So sieht sie aus, wenn der Pin durch den `startstopbutton` belegt ist:

```
root@f2bb9fb94eb9:/usr/src/app# i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
10: 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f
20: 20 21 22 23 24 25 26 27 28 29 2a 2b 2c 2d 2e 2f
30: 30 31 32 33 34 35 36 37 38 39 3a 3b 3c 3d 3e 3f
40: 40 41 42 43 44 45 46 47 48 49 4a 4b 4c 4d 4e 4f
50: 50 51 52 53 54 55 56 57 58 59 5a 5b 5c 5d 5e 5f
60: 60 61 62 63 64 65 66 67 68 69 6a 6b 6c 6d 6e 6f
70: 70 71 72 73 74 75 76 77
```
