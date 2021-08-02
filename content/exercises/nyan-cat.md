## Nyan Cat Roboter

Die [Nyan Cat](https://www.youtube.com/watch?v=QH2-TGUlwu4) ist ein bekanntes Youtube Video mit über 100 Millionen Klicks. 
Aus der Kombination einiger hier vorgestellter Beispielen lässt sich ein Nyan Cat Roboter bauen.

![Nyan-Cat](%assets_url%/giphy.gif)
### Was wird benötigt?

* 4x Adafruit 8x8 LED Matrix mit I2C Backpack, siehe [Roboter Gesicht aus mehreren LED Matrizen](multi-led-matrix)
* 2 Servos und Pan/Tilt Kopf, siehe [Roboter Kopf mit Servo Pan Tilt](servo-pan-tilt)
* Piezo Lautsprecher, siehe [Piezo Lautsprecher](piezo)  

### Programm

Das Programm ist ein Mix aus den Beispielen led-matrix-robo-head.js, j5-songs.js und servo.js. Die Ablaufsteuerung sorgt dafür, das nach 5 Sekunden der Roboter erwacht, seinen Kopf dreht und aus dem Lautsprecher die Nyan-Cat ertönt.
Dann legt sich der Roboter wieder schlafen.

Das Programm wird unter Linux mit ```node ./code/nyan-cat.js```, unter Windows mit ```node code\nyan-cat.js``` gestartet.

```javascript
var five = require('johnny-five');
var songs = require('j5-songs');
var board = new five.Board(); 

board.on("ready", function() {

  var servoPan = new five.Servo({ pin: 4,
                                  range: [40,60]});
  var servoTilt = new five.Servo({ pin: 12,
                                  range: [40,120]});

  var eyeLt = new five.Led.Matrix({
    addresses: [0x70],
    controller: "HT16K33",
    rotation: 4,
  });

  var eyeRt = new five.Led.Matrix({
    addresses: [0x71],
    controller: "HT16K33",
    rotation: 4,
  });

  var mouthLt = new five.Led.Matrix({
    addresses: [0x72],
    controller: "HT16K33",
    rotation: 4,
  });

  var mouthRt = new five.Led.Matrix({
    addresses: [0x73],
    controller: "HT16K33",
    rotation: 4,
  });

  var Eyes = {
    roundEye: [
    "00111100",
    "01111110",
    "11100111",
    "11000011",
    "11000011",
    "11100111",
    "01111110",
    "00111100"
    ],

    ovalEye: [
    "00000000",
    "00111100",
    "01111110",
    "11100111",
    "11100111",
    "01111110",
    "00111100",
    "00000000"
    ],

    closeEye: [
    "00000000",
    "00000000",
    "00111100",
    "11111111",
    "11111111",
    "00111100",
    "00000000",
    "00000000"
    ],
  };

  var smileLt = [
    "00000000",
    "00000000",
    "11000000",
    "01100000",
    "00110000",
    "00011111",
    "00000111",
    "00000000"
  ];

  var smileRt = [
    "00000000",
    "00000000",
    "00000011",
    "00000110",
    "00001100",
    "11111000",
    "11100000",
    "00000000"
  ];
  var grumpyLt = [
    "00000000",
    "00000000",
    "00000011",
    "00000111",
    "00001100",
    "00011000",
    "00011000",
    "00011000"
  ];

  var grumpyRt = [
    "00000000",
    "00000000",
    "11000000",
    "11100000",
    "00110000",
    "00011000",
    "00011000",
    "00011000"
  ];
 
  var piezo = new five.Piezo(5); 
  // Load a song object 
  var song = songs.load('nyan-melody');

  board.wait(5000, function() {
    servoPan.center();
    servoTilt.max();

    eyeLt.draw(Eyes.closeEye);
    eyeRt.draw(Eyes.closeEye);
    mouthLt.draw(grumpyLt);
    mouthRt.draw(grumpyRt);
  });
 
  board.wait(10000, function() {
    servoPan.min();
    servoTilt.center();
  
    eyeLt.draw(Eyes.roundEye);
    eyeRt.draw(Eyes.roundEye);
    mouthLt.draw(smileLt);
    mouthRt.draw(smileRt);
  });
 
  board.wait(12000, function() {
    // Play it ! 
    piezo.play(song);
    servoPan.sweep({
        range: [40, 60]
    });
  });


  board.wait(45000, function() {
    servoPan.stop();
    eyeLt.draw(Eyes.ovalEye);
    eyeLt.draw(Eyes.closeEye);
    eyeLt.draw(Eyes.roundEye);
  });

  board.wait(48000, function() {
    servoPan.center();
    servoTilt.max();

    eyeLt.draw(Eyes.closeEye);
    eyeRt.draw(Eyes.closeEye);
    mouthLt.draw(grumpyLt);
    mouthRt.draw(grumpyRt);
  });
});
```

