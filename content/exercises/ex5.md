# Potentiometer

In dieser Übung lernen wir ein neues Bauteil kenne. Das Potentiometer, ein Widerstand mit variablem Mittelabgriff. Je nach Stellung des Potentiometers wird die Helligkeit der LED gesteuert (links volle Helligkeit, rechts aus).

Es werden die selben Bauteile benötigt wie im ersten Beispiel. Zusätzlich kommt noch das Potentiometer dazu. Es wird ein PWM-fähiger Pin benötigt. Beim Arduino Micro ist im Gegensatz zum Arduino Uno auch Pin 13 PWM fähig, ebenso wie Pin 11, 10, 9, 6, 5 und 3.

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder hinzugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestartet:

```
node code/05-code-poti.js`
```

Über die Kommandozeile wird auch der gelesen Analog-Wert des Potentiometers angezeigt. Drücke `Control-D` um das Programm zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* 5mm LED
* 680 Ohm Widerstand (blau-grau-braun)
* 10 kOhm Potentiometer (linear)

## Schaltplan und Verdrahtung

Die Verdrahtung:

![Verdrahtung](%assets_url%/circ/05-LED-Poti_Steckplatine.png "Verdrahtung")

Der Schaltplan:

![Schaltplan](%assets_url%/circ/led-schematic.png "Schaltplan") ![Schaltplan](../../images/circ/poti-schematic.png "Schaltplan")

## Programm

Das Javascript Programm befindet sich unter `code/05-code-poti.js`

```
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var led = new five.Led(13);
  // Create a new generic sensor instance for
  // a sensor connected to an analog (ADC) pin
  var sensor = new five.Sensor("A5");

  // When the sensor value changes, log the value
  sensor.on("change", function() {
    led.brightness(this.value/4);
    console.log(this.value);
  });
});
```

## Fehlersuche

### LED leuchtet nicht

Dioden sind gepolte Bauelemente. D.h. sie funktionieren nur in eine Richtung. Versuche die LED um 180° zu drehen (keine Sorge, die LED geht nicht kaputt, wenn sie falsch gepolt eingebaut wurde).

### Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```
var board = new five.Board({port:'COM7'});
```

## Programm erweitern

## Mehr Informationen

* [Johnny-Five Sensor Beispiele](http://johnny-five.io/examples/sensor/)