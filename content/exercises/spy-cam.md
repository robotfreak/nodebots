## Spy-Cam

Ein Roboter der ferngesteuert werden kann und dabei ein Live Bild zum Web Browser streamt. Die Kamera kann über den Servo Kopf geneigt und geschwenkt werden.

### Was wird benötigt?

* 2 Servos und Pan/Tilt Kopf, siehe [Roboter Kopf mit Servo Pan Tilt](servo-pan-tilt)
* Roboter Chasssis mit 2 Motoren und [2-fach Motor-Treiber](dual-motor)
* Raspberry Pi mit Pi Camera
* [mjpeg-streamer](https://github.com/jacksonliam/mjpg-streamer) zum Streamen des Videos

### Schaltplan

![Verdrahtung](%assets_url%/circ/spy-cam_Steckplatine.png)

### Programm

Über die Cursor Tasten kann der Roboter bewegt werden. Die Kamera kann mit den Tasten 'a' und 'd' geschwenkt, 'w' und 'x' geneigt und mit 's' in die Mittenposition gesteuert werden. Die Live Video Ausgabe erfolgt im Browser unter der Adresse 'http://localhost:8080/javascript-simple.html'

Es wird unter Linux mit `node ./code/spy-cam.js` unter Windows mit `node code\spy-cam.js` gestartet.

```javascript
var five = require("johnny-five");

var board = new five.Board();

board.on("ready", function() {
  console.log('ready');

  var servoPan = new five.Servo({ pin: 4,
                                  range: [60,120]});
  var servoTilt = new five.Servo({ pin: 12,
                                  range: [60,120]});

  var tiltVal = 90;
  var panVal = 90

  function tiltDown() {
    if (tiltVal > 60) tiltVal -= 5;
    servoTilt.to(tiltVal);
  }

  function tiltUp() {
    if (tiltVal < 120) tiltVal += 5;
    servoTilt.to(tiltVal);
  }

  function panLeft() {
    if (panVal < 120) panVal += 5;
    servoPan.to(panVal);
  }

  function panRight() {
    if (panVal > 60) panVal -= 5;
    servoPan.to(panVal);
  }

  function center() {
    servoPan.center();
    servoTilt.center();
  }

  var rightWheel = new five.Motor({
    pins: { pwm: 6, dir: 7 }
  });

  var leftWheel = new five.Motor({
    pins: { pwm: 11, dir: 8 }
  });

  var speed = 100;

  function reverse() {
    leftWheel.reverse(speed);
    rightWheel.reverse(speed);
    console.log("backward");
  }

  function forward() {
    leftWheel.forward(speed);
    rightWheel.forward(speed);
    console.log("forward");
  }

  function stop() {
    leftWheel.stop();
    rightWheel.stop();
    console.log("stop");
  }

  function left() {
    leftWheel.reverse(speed);
    rightWheel.forward(speed);
    console.log("left");
  }

  function leftForward() {
    leftWheel.forward(speed);
    rightWheel.stop(0);
    console.log("left forward");
  }
  function leftReverse() {
    leftWheel.reverse(speed);
    rightWheel.stop(0);
    console.log("left backward");
  }
  function right() {
    leftWheel.forward(speed);
    rightWheel.reverse(speed);
    console.log("right");
  }
  function rightForward() {
    rightWheel.forward(speed);
    leftWheel.stop();
    console.log("right forward");
  }
  function rightReverse() {
    rightWheel.reverse(speed);
    leftWheel.stop();
    console.log("right backward");
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
    'w': tiltUp,
    'x': tiltDown,
    'a': panLeft,
    'd': panRight,
    's': center,
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
