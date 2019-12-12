# iobroker.sonos_api
How to Use the sonos API

Nach Vorarbeit des Users skokarl im ioBroker-Forum

https://forum.iobroker.net/topic/22888/gel%C3%B6st-sonos-http-api-installation-f%C3%BCr-newbies-dummies-und-mich

---

## Installation

Mit Putty auf dem der kOnsole vom ioBroker Server Broker einloggen
und folgende Befehle der Reihe nach eingeben


```
wget https://github.com/jishi/node-sonos-http-api/archive/master.zip
unzip master.zip
cd node-sonos-http-api-master
npm install --production
```
---

## Start des Servers

```
npm start
```
Putty erst einmal offen lassen !!

### Einfügen in den Autostart
```
nano /etc/systemd/system/sonosapi.service
```

Dazu eine Datei `sonosapi.service` mit folgendem Inhalt anlegen

```
[Unit]
Description=Sonos HTTP API Daemon
After=syslog.target network.target

[Service]
Type=simple
ExecStart=/usr/bin/node /home/$user$/node-sonos-http-api-master/server.js
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Den Pfad zu server.js bitte anpassen!

Anschließend den Service starten mit `sudo systemctl enable sonosapi.service`

---

## Erster Test

Aufruf der IP des ioBroker Servers mit Port 5005

http://192.168.xxx.xxx:5005/

In Putty tauchen jetzt zwei Meldungen auf, diese erst einmal ignorieren.

1.) settings.json wird nicht gefunden ( ignorieren )
Wenn keine settings.json gefunden wird, wird die default Einstellung genommen.
In den Settings könnten spezielle Einstellungen verändert werden, wie z.B der Port 5005

2.) http server listening on 0.0.0.0
Der Server lauscht also an allen IPs

##TTS-Befehle
Man kann verschiedene Sprachen und Stimmen, sowie verschiedene TTS-Engines verwenden:

* voicerss
* Microsoft Cognitive Services (Bing Text to Speech API)
* AWS Polly
* Google (default)
* macOS say command

Bei den meisten ist eine Anmeldung und ein Key notwendig, Google ist ohne API-Key zu nutzen.

Der Aufruf eines TTS über Google läuft wie folgt:

```
/[Room name]/say/[phrase][/[language_code]][/[announce volume]]
/sayall/[phrase][/[language_code]][/[announce volume]]
```

[Room Name] ist der Name eines in dem Sonos-Controller vergebenen Lautsprechers
[phrase] ist der zu sprechende Text
[language_code] ist die gewünschte Sprache, im deutschsprachiegen Raum also `de`
[announce volume] ist die gewünschte Lautstärke. 

---

## Weitere Funktionen

### Grouping
Diese Funktion bindet zwei (oder mehr) Lautsprecher zu einer Gruppe. Der Befehl `/Kitchen/join/Office` fügt den Lautsprecher "Kitchen" dem Lautsprecher "Office" in einer Gruppe hinzu. Um die Gruppe wieder zu verlassen wird der Befehl `/Kitchen/leave` verwendet oder der Lautsprecher einer anderen Gruppe hinzugefügt.


Die Api enthält jede Menge weitere Funktionen, auf die hier nicht weiter eingegangen wird.

* play
* pause
* playpause (toggles playing state)
* volume (parameter is absolute or relative volume. Prefix +/- indicates relative volume)
* groupVolume (parameter is absolute or relative volume. Prefix +/- indicates relative volume)
* mute / unmute
* groupMute / groupUnmute
* togglemute (toggles mute state)
* trackseek (parameter is queue index)
* timeseek (parameter is in seconds, 60 for 1:00, 120 for 2:00 etc)
* next
* previous
* state (will return a json-representation of the current state of player)
* favorite
* favorites (with optional "detailed" parameter)
* playlist
* lockvolumes / unlockvolumes (experimental, will enforce the volume that was selected when locking!)
* repeat (on/off)
* shuffle (on/off)
* crossfade (on/off)
* pauseall (with optional timeout in minutes)
* resumeall (will resume the ones that was pause on the pauseall call. Useful for doorbell, phone calls, etc. Optional timeout)
* say
* sayall
* saypreset
* queue
* clearqueue
* sleep (values in seconds)
* linein (only analog linein, not PLAYBAR yet)
* clip (announce custom mp3 clip)
* clipall
* clippreset
* join / leave (Grouping actions)
* sub (on/off/gain/crossover/polarity) See SUB section for more info
* nightmode (on/off, PLAYBAR only)
* speechenhancement (on/off, PLAYBAR only)
* bass/treble (use -10 thru 10 as value. 0 is neutral)

Weitere Informationen auf github:
https://github.com/jishi/node-sonos-http-api#usage
