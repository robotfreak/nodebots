## Servo

Ein Servo ist ein kleiner Getriebemotor mit eingebauter Elektronik. Im Gegensatz zu einem Getriebemotor dreht sich der Servo nicht kontinuierlich, sondern nur innerhalb eines bestimmten Winkels (180°). Der gewünschte Winkel wird dem Servo vom Mikrocontroller über den Servo Steuer Pin  mitgeteilt. Ein Impuls mit einer bestimmten Länge (zwischen 1..2ms)  wird vom Servo in den entsprechenden Winkel von 0..180° umgesetzt.

![Servo](%assets_url%/parts/servo.png "Servo")

### Schaltung

![Verdrahtung](%assets_url%/circ/servo_Steckplatine.png "Verdrahtung")

![Schaltplan](%assets_url%/circ/servo_Schaltplan.png "Schaltplan")

### Programm

Das Programm lässt den Servo für 4 Sekunden schwenken. Es wird unter Linux mit `node ./code/servo.js` unter Windows mit `node code\servo.js` gestartet.

Drücke `Control-D` um das Program zu beenden.

```

```javascript
var five = require("johnny-five");
var board = new five.Board();
 

board.on("ready", function() {
  var range = [60, 120];   
  var servo = new five.Servo({
    pin: 4,
    range: range,
    center: true 
  });
  var lap = 0;

  servo.sweep();

  board.wait(4000, function() {
    servo.stop();
  });
  
  this.repl.inject({
    servo: servo
  });

});
```

### Übungen

NAch dem der Servo gestoppt hat, kann man über das Terminal den Servo steuern. Probiere `servo.center()`, `servo.min()` und `servo.max()` aus.

## Mehr Informationen

* [Johnny-Five Servo Beispiele](http://johnny-five.io/examples/servo/)
* [Johnny-Five Servo API](http://johnny-five.io/api/servo)
