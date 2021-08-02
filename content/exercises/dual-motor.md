## 2-fach Motor Treiber

Die einfachste Möglichkeit für einen Roboter, der sich in alle Richtungen bewegen kann, besteht darin 2 Motoren zu verwenden. Diese Antriebsart nennt sich 'differantial wheeled robot' oder auch 'two wheel drive (2WD)'.  Damit die Moteren unabhängig voneinander angesteuert werden können, ist auch ein 2-fach Motor Treiber notwendig.

Motor-Treiber gibt es in vielen Ausführungen als komplette integrierte Schaltung inklusive Ansteuer Logik und Schutzdioden. 

### Schaltung

Der VN2SP30 Motor Treiber ist für größere Roboter (>3kg) gedacht. Für kleine Roboter (<1kg) tuts es auch ein L298 oder L293D Motor Trieber. Die Ansteuerung ist aber identisch. Je Motor werden 2 Ausgangs Pin für die Drehrichtung und ein PWM-fähiger Ausgangs Pin für die Geschwindigkeit.

Insgesamt 6 Pins ist schon recht viel für einen Roboter. Mit einem kleinen Schaltungs Trick läßt sich die Zahl auf 4 Pins reduzieren. Es reicht nämlich ein Pin für die Drehrichtung aus. Das zweite Signal ist lediglich dias invertierte erste Signal. Eine Transistorstufe als Inverter übernimmt diesen Job. 

![Verdrahtung](%assets_url%/circ/dual-motor-driver_Steckplatine.png "Verdrahtung")

![Schaltplan](%assets_url%/circ/dual-motor-driver_Schaltplan.png "Schaltplan")

### Programm

Das Programm wird unter Linux mit `node ./code/motor-remote.js` unter Windows mit `node code\motor-remote.js` gestartet

```javascript
var five = require("johnny-five");

var board = new five.Board();

board.on("ready", function() {
  console.log('ready');

  var rightWheel = new five.Motor({
    pins: { pwm: 11, dir: 8 }
  });

  var leftWheel = new five.Motor({
    pins: { pwm: 6, dir: 7 }
  });

  var speed = 100;

  function reverse() {
    leftWheel.reverse(speed);
    rightWheel.reverse(speed);
  }

  function forward() {
    leftWheel.forward(speed);
    rightWheel.forward(speed);
  }

  function stop() {
    leftWheel.stop();
    rightWheel.stop();
  }

  function left() {
    leftWheel.reverse(speed);
    rightWheel.forward(speed);
  }

  function right() {
    leftWheel.forward(speed);
    rightWheel.reverse(speed);
  }

  function exit() {
    leftWheel.stop();
    rightWheel.stop();
    setTimeout(process.exit, 1000);
  }

  var keyMap = {
    'up': forward,
    'down': reverse,
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
      board.wait(1000, function() {
        stop();
      });
    });
});
```

### Übungen

Mit den Cursor Tasten der Tastatur kann der Roboter gesteuert werden. 

Ein weiteres Programm names `dual-motor.js` funktioniert ähnlich. Zusätzlich kann mit den Zahlen 1...9 die Geschwindigkeit eingestellt werden.
