
## Roboter Gesicht aus mehreren LED Matrizen

Es lassen sich über den I2C Bus auch mehrere LED Matrizen ansteuern. Dazu besitzt jede Anzeige eine 2-3 stellige Adresse, die über Lötbrücken geändert werden kann. Natürlich muss dann jede Anzeige eine eigene individuelle Adresse bekommen. So läßt sich z.B. aus 4 LED Matrizen ein Gesicht, bestehend aus zwei Augen (2 LED Matrizen) und einem Mund (ebenfalls 2 LED Matrizen) bilden. Damit ergeben sich noch mehr Anzeige Möglichkeiten und der Roboter erhält ein Gesicht um z.B. Gemütszustände anzuzeigen.

![Roboter Gesicht](%assets_url%/robo-face.jpg "Roboter Gesicht")

### Was wird benötigt?

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* 4 x Adafruit 8x8 LED Matrix mit I2C Backpack

### Schaltung

![Verdrahtung](%assets_url%/circ/4xLED-Matrix_Steckplatine.png "Verdrahtung")

### Programm

Das Programm gibt ein Gesicht auf den 4 LED-MAtrizen aus. Es wird gestartet

unter Linux mit: 

```
node ./code/led-matrix-robo-head.js
```

unter Windows mit:

```
node code\led-matrix-robo-head.js
```

```javascript
var five = require("johnny-five");
var board = new five.Board(); 

board.on("ready", function() {

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

  var Eyes = {
    grumpyEye: [
    "00011000",
    "01100110",
    "01100110",
    "11111111",
    "11111111",
    "01111110",
    "01111110",
    "00011000"
    ],

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

    roundBigEye: [
    "00111100",
    "01100110",
    "11000011",
    "11000011",
    "11000011",
    "11000011",
    "01100110",
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

    ovalBigEye: [
    "00000000",
    "00111100",
    "01100110",
    "11000011",
    "11000011",
    "01100110",
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

  eyeLt.clear();
  eyeRt.clear();
  mouthLt.clear();
  mouthRt.clear();
  eyeLt.draw(Eyes.roundEye);
  eyeRt.draw(Eyes.roundEye);
  mouthLt.draw(smileLt);
  mouthRt.draw(smileRt);

  this.repl.inject({
    twinkle: function() {
      eyeLt.draw(Eyes.roundEye);
      eyeRt.draw(Eyes.roundEye);
      eyeLt.draw(Eyes.ovalEye);
      eyeLt.draw(Eyes.closeEye);
      eyeLt.draw(Eyes.roundEye);
    }
  });

  this.repl.inject({
    eyes: function(shape) {
      eyeLt.draw(shape);
      eyeRt.draw(shape);
    }
  });

  this.repl.inject({
    mouth: function(shape) {
      mouthLt.draw(five.Led.Matrix.CHARS[shape]);
      mouthRt.draw(five.Led.Matrix.CHARS[shape]);
    }
  });
  
});
```

### Übungen

Über den Befehl ```eyes("ovalEye")``` läßt sich die Form der Augen ändern. Probiere als Parameter "ovalEye", "roundEye" oder "grumpyEye" aus

Mit dem Befehl ```twinkle()``` zwinkert der Roboter dir zu.

Mit dem Befehl ```mouth("?")``` kannst du den Roboter 2 Fragezeichen als Mund zeichnen lassen

### Weiterführende Links

* [Johnny-Five LED.Matrix API](http://johnny-five.io/api/led.matrix/)
