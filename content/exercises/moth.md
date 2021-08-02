## Roboter Motte

Ein Roboter der die hellste Stelle eines Raumes sucht, die Motte.

### Was wird benötigt?

* 2 LDR Sensoren, siehe [Licht-Sensor](ldr)
* Roboter Chasssis mit 2 Motoren und 2-fach Motor-Treiber, siehe [2-fach Motor Treiber](dual-motor)

### Schaltplan

![Verdrahtung](%assets_url%/circ/moth_Steckplatine.png)

### Programm

```javascript
var five = require("johnny-five");

var board = new five.Board();

var farRange = 40 // distance in cm
var midRange = 25 // distance in cm
var nearRange = 14 // distance in cm

board.on("ready", function() {

  var leftLDRVal =  0;
  var rightLDRVal = 0;
  var ThresholdVal = 200;

  // Create the LDR sensors 
  ldrLt = new five.Sensor({
    pin: "A2",
    freq: 100
  });

  ldrRt = new five.Sensor({
    pin: "A3",
    freq: 100
  });

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

  function right() {
    leftWheel.forward(speed);
    rightWheel.reverse(speed);
    console.log("right");
  }
  
  function checkLight() {
    if (leftLDRVal - rightLDRVal >= ThresholdVal) {
      right();
    }
    else if (rightLDRVal - leftLDRVal  >= ThresholdVal) {
      left();
    }
    else {
      forward();
    }
  }

  ldrLt.on("change", function( err, value) {
    console.log("Left : ", this.value);
    leftLDRVal = this.value;
    checkLight();
  });

  ldrRt.on("change", function( err, value) {
    console.log("Right: ", this.value);
    rightLDRVal = this.value;
    checkLight();
  });
});
```

### Übungen

Wie könnte man das Programm ändern, das aus der Motte eine Kakerlake wird. Kleiner Tip, Kakerlaken meiden das Licht.
Die Lösung findest du im Programm `code/cockroach.js`
