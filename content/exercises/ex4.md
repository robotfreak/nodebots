# Mehrere Taster

In dieser Übung erweitern wir den vorhandenen Taster um zwei weitere Taster. Das besondere dabei, es wird weiterhin nur ein Eingangs Pin benötigt. Der Zustand der Taster wird über verschiedene LED Zustände angezeigt.

Es werden neben Bauteile aus der vorhergehenden Übung werden zwei weitere Taster und zwei weitere Widerstände benötigt. Die Taster werden über ein Widerstandsnetzwerk am Analog Eingang A0 angeschlossen, die LED wie vorher auch an Pin 13. 

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/04-code-3xbutton.js`
```

Drücke `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* 5mm LED 
* 3x Taster 10x10mm
* 2,2 kOhm Widerstand (rot-rot-rot)
* 330 Ohm Widerstand (orange-orange-braun)
* 2 x 680 Ohm Widerstand (blau-grau-braun)

## Schaltplan und Verdrahtung

Die Verdrahtung:

![Verdrahtung](%assets_url%/circ/04-LED-3xButton_Steckplatine.png "Verdrahtung")

Der Schaltplan:

![Schaltplan](%assets_url%/circ/led-schematic.png "Schaltplan")
![Schaltplan](%assets_url%/circ/3xbutton-schematic.png "Schaltplan")

## Programm

Das Javascript Programm befindet sich unter `code/04-code-3xbutton.js`

```javascript
var five = require("johnny-five");

five.Board().on("ready", function() {

  var button = new five.Button("A0");
  var led = new five.Led(13);

  button.on("up", function(){
    led.on();
  });

  button.on("down", function(){
    led.off();
  });
});
```
	
## Fehlersuche

### Taste funktioniert nicht

Die Tasten geben manchmal schlechten Kontakt, wenn sie auf das Steckbrett gesteckt werden. VIelleicht noch einmal fest auf die Taste drücken, damit der Kontakt besser wird 

### LED leuchtet nicht

Dioden sind gepolte Bauelemente. D.h. sie funktionieren nur in eine Richtung. Versuche die LED um 180° zu drehen (keine Sorge, die LED geht nicht kaputt, wenn sie falsch gepolt eingebaut wurde).


###  Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```javascript
var board = new j5.Board({port:'COM7'});
```

## Programm erweitern

## Mehr Informationen

* [Johnny-Five Taster Beispiele](http://johnny-five.io/api/button/)
