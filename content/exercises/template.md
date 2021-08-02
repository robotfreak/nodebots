Zu Beginn werden die in der Teileliste gennannten Bauteile benötigt.

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/???.js
```

Über die Kommandozeile kann die Schaltung auch über REPL Befehle (read–eval–print-loop) gesteuert werden. Gib dazu nach dem `>>` prompt ??? ein. Drücke `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set

## Schaltplan und Verdrahtung

Der Verdrahtungsplan: 

![Verdrahtung](%assets_url%/circ/???_Steckplatine.png "Verdrahtung")

Der Schaltplan:

![Schaltplan](%assets_url%/circ/???-schematic.png "Schaltplan")

## Programm

Das Javascript Programm befindet sich unter `code/???.js`

```javascript
var five = require("johnny-five");

five.Board().on("ready", function() {

});
```

## Fehlersuche

###  Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```javascript
var board = new j5.Board({port:'COM7'});
```

## Programm erweitern

## Mehr Informationen

* [Johnny-Five ??? Beispiele](http://johnny-five.io/examples/???/)



