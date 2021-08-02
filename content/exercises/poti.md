## Potentiometer

In dieser Übung lernen wir ein neues Bauteil kenne. Das Potentiometer, ein Widerstand mit variablem Mittelabgriff. Je nach Stellung des Potentiometers wird die Helligkeit der LED gesteuert (links volle Helligkeit, rechts aus).

Es werden die selben Bauteile benötigt wie im ersten Beispiel. Zusätzlich kommt noch das Potentiometer dazu. Es wird ein PWM-fähiger Pin benötigt. Beim Arduino Micro ist im Gegensatz zum Arduino Uno auch Pin 13 PWM fähig, ebenso wie Pin 11, 10, 9, 6, 5 und 3.

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

![Schaltplan](%assets_url%/circ/led-schematic.png "Schaltplan")
![Schaltplan](%assets_url%/circ/poti-schematic.png "Schaltplan")

## Programm

Über die Kommandozeile wird auch der gelesen Analog-Wert des Potentiometers angezeigt.

Das Javascript Programm befindet sich unter `code/poti.js` 
Es wird unter Linux mit `node ./code/poti.js` unter Windows mit `node code\poti.js` gestartet.

Drücke `Control-D` um das Program zu beenden.

```javascript
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
## Mehr Informationen

* [Johnny-Five Sensor Beispiele](http://johnny-five.io/examples/sensor/)
* [Johnny-Five Sensor API](http://johnny-five.io/api/sensor)
