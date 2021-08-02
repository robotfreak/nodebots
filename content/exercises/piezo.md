## Piezo Lautsprecher

Ein Piezo ist ein kleiner Lautsprecher, der Signal-Töne ausgeben kann. 

![Piezo](%assets_url%/parts/piezo.png "Piezo")

### Was wird benötigt

* Piezo Lautsprecher
* optional, NPN Transistor BC547, 2k2 Widerstand

### Schaltung

Nicht unbedingt notwendig ist die Transistor Stufe.

![Verdrahtung](%assets_url%/circ/piezo_Steckplatine.png "Verdrahtung")

![Schaltplan](%assets_url%/circ/piezo-schematic.png "Schaltplan")

### Programm

Das Programm gibt eine Melodie aus. Erkennst du die Melodie? Es wird unter Linux mit ```node ./code/piezo.js```, unter Windows mit ```node code\piezo.js``` gestartet.

```javascript
var five = require("johnny-five"),
  board = new five.Board();

board.on("ready", function() {
  // Creates a piezo object and defines the pin to be used for the signal
  var piezo = new five.Piezo(5);

  // Injects the piezo into the repl
  board.repl.inject({
    piezo: piezo
  });

  // Plays the same song with a string representation
  piezo.play({
    // song is composed by a string of notes
    // a default beat is set, and the default octave is used
    // any invalid note is read as "no note"
    song: "e5 e5 - e5 - c5 e5 - g5 - - - g4 - - - c5 - - g4 - - e4 - - a4 - b4 - a#4 a4 - g4 e5 g5 a5 - f5 g5 - e5 - c5 d5 b4 - - c5 - - g4 - - e4 - - a4 - b4 - a#4 a4 - g4 e5 g5 a5 - f5 g5 - e5 - c5 d5 b4 - -",
    beats: 1 / 4,
    tempo: 100
  });

});
```
### Übungen

Es gibt noch mehr Songs. Lade das Programm `j5-songs.js`. Probiere über das Terminal verschiedene Songs aus, z.B. `piezo.play(songs.load('tetris-theme'))` oder `piezo.play(songs.load('mario-intro'))`. Die komplette Liste steht im Verzeichnis `./node-modules/j5-songs/lib/songs`

### weitere Informationen

* [Johnny-Five Piezo Examples](http://johnny-five.io/examples/piezo/)
* [Johnny-Five Piezo API](http://johnny-five.io/api/piezo/)
* [J5-Songs Bibliothek](https://www.npmjs.com/package/j5-songs)
