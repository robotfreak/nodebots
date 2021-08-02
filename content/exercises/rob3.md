Ein Roboter ohne Sensoren ist kein richtiger Roboter. Erst durch Sensoren kann der Roboter seine Umgebung wahrnehmen und zum Beispiel Hindernisse zu erkennen und ihnen auszuweichen.

Der Sgarp GP2Y0A21YK ist ein Distanzsensor, der die Entfernung zu einem Hindernis in Form einer analogen Ausgangsspannung anzeigt. Der Sensor besteht aus einer Infrarot Sendediode und einem Empfänger Chip. Zur Berechnung der Entfernung dient nicht etwa die Intensität des empfangen Lichtes, sondern der Winkel, in dem der empfangene Lichtstrahl empfangen wird. Über das sogenannte Triangulations Verfahren kann anhand des Abstabd zwischen Sende Diode und Emfänger Chip und dem gemessenen Winkel, der Abstand zum Hindernis bestimmt werden.


Zu Beginn werden die in der Teileliste gennannten Bauteile benötigt.

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/ir-sensor.js
```

Über die Kommandozeile kann die Schaltung auch über REPL Befehle (read–eval–print-loop) gesteuert werden. Gib dazu nach dem `>>` prompt ??? ein. Drücke `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* Infrarot Entfernungssensor (z.B. Sharp GP2D120XJ00F)

## Schaltplan und Verdrahtung

Der Verdrahtungsplan: 

![Verdrahtung](%assets_url%/circ/IR-Sensor_Steckplatine.png "Verdrahtung")

## Programm

Das Javascript Programm befindet sich unter `code/ir-sensor.js`

```javascript
var five = require("johnny-five");

five.Board().on("ready", function() {
  var digits = new five.Led.Digits({
    controller: "HT16K33",
  });
  var proximity = new five.Proximity({
    controller: "GP2D120XJ00F",
    pin: "A4"
  });

  proximity.on("data", function() {
    digits.print(this.cm);
    console.log("Entfernung (cm): ", this.cm);
  });
});
```

## Fehlersuche

### Die Sensorwerte schwanken sehr stark

Obwohl sich kein Hindernis in der Nähe befindet schwankt der Sensorwert sehr stark.

Die Ursache liegt an Spannungseinbrüchen, da bei den Messungen des Sensors (Einschalten der Sende IR Diode) große Stromspitzen auftreten können. Abhilfe schaffen KOndensatoren, die direkt am Sensor zwischen die Stromversorgungspins gelötet werden. Ein 100nF und parallel dazu ein 0..100µF Elko sorgen für saubere Pegel. 

###  Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```javascript
var board = new j5.Board({port:'COM7'});
```

## Programm erweitern

## Mehr Informationen

* [Johnny-Five Distanzsensoren Beispiele](http://johnny-five.io/examples/proximity/)



