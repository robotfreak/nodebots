Als Nächstes probieren wir es mit zwei Servos. Damit läßt sich z.B. ein Roboterkopf in zwei Ebenen bewegen. Dazu gibt es fertige Halterungen (Pan Tilt Kit), an denen die Servos befestigt werden. 

Zu Beginn werden die in der Teileliste gennannten Bauteile benötigt. Die Servos werden nach Anleitung mit dem Pan/Tilt Kopf zusammengebaut.

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/servo-remote.js
```

Über die Kommandozeile können die Servos mit den Cursor Tasten gesteuert werden. Drücke `q` oder `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* 6 Testkabel (männlich/männlich)
* 2 Servos
* Pan/Tilt Halterung

## Schaltplan und Verdrahtung

Der Verdrahtungsplan: 

![Verdrahtung](%assets_url%/circ/servo-pan-tilt_Steckplatine.png "Verdrahtung")

## Programm

Das Javascript Programm befindet sich unter `code/servo-remote.js`

```javascript
var five = require("johnny-five");

five.Board().on("ready", function() {
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

## Fehlersuche

### Die Verbindung zum Board geht verloren, der Controller resetted sich

Servos können schon recht viel Strom ziehen. Das kann schon zuviel sein für den USB Anschluss. Darüber dürfen maximal 500mA Strom fliessen. Zieht der Servo zu viel Strom, bricht die Stromversorgung zusammen und der Arduino vollzieht einen Neustart.

Abhilfe schafft hier eine eigene Stromversorgung für den Servo. Der Servo benötigt eine Sappnung zwischen 4,8 und 6V.

###  Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```javascript
var board = new j5.Board({port:'COM7'});
```

## Programm erweitern

## Mehr Informationen

* [Johnny-Five Servo Beispiele](http://johnny-five.io/examples/??servo/)

