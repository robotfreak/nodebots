## Ultraschall Sensor

Ultraschall Sensoren funktionieren nach dem Prinzip der [Echoortung](https://de.wikipedia.org/wiki/Echoortung). Dabei werden über einen speziellen Lautsprecher kurze Pulse von Schallwellen im Ultraschall Bereich ausgesendet und über ein Mikrofon wieder empfanegen. Aus der gemssenen Laufzeit, die das Siganl braucht (nach dem Senden bis zum Empfang) kann die Entfernung zu einem Objekt bestimmt werden.  

Ultraschall Sensoren gibt es mit verschiedenen Anschluss Möglichketiten. Die meisten Sensoren benötigen ein oder zwei digital Pins zur Ansteuerung, wie der Parallax Ping, Devantech SR-04 SR-05 oder der preiswerte China Klon HC-SR04. Dabei wird ein Trigger Signal ausgegeben und die Pulslänge über den Echo Eingang gelesen. Leider wird  das Messen von Pulslängen ( pulseIn() Funktion) über das Standard Firmata Sketch nicht unterstützt. Es gibt allerdings eine angepasste [Firmata Version](http://johnny-five.io/api/proximity/#pingfirmata), auf die ich aber an dieser Stelle nicht weiter eingehen möchte.

Deshalb kommt hier ein Sensor mit analoger Ansteuerung zum Einsatz, der Maxbotix EZ0 (MB1000). Der Sensor kann Objekte im Bereich 15..645cm erkennen.

![Ultraschall-Sensor](%assets_url%/parts/ultraschall.png "Ultraschall-Sensor")

### Schaltung

![Verdrahtung](%assets_url%/circ/ultraschall-sensor_Steckplatine.png "Verdrahtung")

![Schaltplan](%assets_url%/circ/ultraschall-sensor_Schaltplan.png "Schaltplan")

### Programm

Das Programm gibt den Wert des Sensors im Terminal (und optional auf der 7-Segment Anzeige) aus. Es wird gestartet

unter Linux mit: 

```
node ./code/ultrasonic.js
```

unter Windows mit:

```
node code\ultrasonic.js
```
```javascript
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {
  var proximity = new five.Proximity({
    controller: "MB1000",
    pin: "A4"
  });

  proximity.on("data", function() {
    console.log("Entfernung: ");
    console.log("  cm  : ", this.cm);
    console.log("-----------------");
  });

  proximity.on("change", function() {
    console.log("Das Objekt has sich bewegt");
  });
});
```

### Übungen

Bewege deine Hand vor dem Sensor hin und her und beobachte den ausgegebenen Sensor Wert.

### Mehr Informationen

[Johnny-Five Ultraschall Sensor Beispiel](http://johnny-five.io/examples/proximity-mb1000/)
[Johnny-Five Sonar API](http://johnny-five.io/api/sonar)
[Johnny-Five Proximity API](http://johnny-five.io/api/proximity)
