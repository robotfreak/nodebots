## Roboter Kopf mit Servo Pan Tilt

Mit 2 Servos kann sich der Roboterkopf in 2 Richtungen bewegen, Pan & Tilt (Schwenken und Neigen) ist hierzu der Fachbegriff aus der Videotechnik.

Neben den Servos wird eine Halterung benötigt, um die Servos und den Kopf daran zu befestigen. Der untere Servo schwenkt die Halterung samt oberen Servo und den Kopf. Der ober Servo übernimmt das Neigen des Kopfes

![Servo Pan/Tilt Kopf](%assets_url%/servo-pan-tilt.jpg "Servo Pan/Tilt Kopf")

### Was wird benötigt?

* 2 Standard Servos
* Pan Tilt Halterung, z.B. [Lynx B](http://www.lynxmotion.com/p-287-lynx-b-pan-and-tilt-kit-black-anodized.aspx) von Lynxmotion

### Schaltung

![Verdrahtung](%assets_url%/circ/servo-pan-tilt_Steckplatine.png "Verdrahtung")

![Schaltplan](%assets_url%/circ/servo-pan-tilt_Schaltplan.png "Schaltplan")

### Programm

Mit dem Programm lässt sich der Roboterkopf über die Cursor Tasten steuern . Es wird unter Linux mit ```node ./code/servo-remote.js```
unter Windows mit ```node code\servo-remote.js``` gestartet.

```javascript
var five = require('johnny-five')
var board = new five.Board()

board.on('ready', function () {
  var servoPan = new five.Servo({ pin: 4,
                                  range: [20,160]});
  var servoTilt = new five.Servo({ pin: 12,
                                  range: [20,160]});

  function down() {
    servoTilt.min();
  }

  function up() {
    servoTilt.max();
  }

  function left() {
    servoPan.max();
  }

  function right() {
    servoPan.min();
  }

  function stop() {
    servoPan.center();
    servoTilt.center();
  }

  function exit() {
    servoPan.stop();
    servoTilt.stop();
    setTimeout(process.exit, 1000);
  }

  var keyMap = {
    'up': up,
    'down': down,
    'left': left,
    'right': right,
    'space': stop,
    'q': exit
  };

  var stdin = process.stdin;
  stdin.setRawMode(true);
  stdin.resume();

  stdin.on("keypress", function(chunk, key) {
      if (!key || !keyMap[key.name]) return;      

      keyMap[key.name]();
  });
});
```

### Übungen

Über die Cursor Tasten der Tastatur kann der Roboterkopf bewegt werden.

## Mehr Informationen

* [Johnny-Five Servo Beispiele](http://johnny-five.io/examples/servo/)
* [Johnny-Five Servo API](http://johnny-five.io/api/servo)
