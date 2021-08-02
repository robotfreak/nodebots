# Taster 

In dieser Übung lernen wie ein Eingabe Element kennen, den Taster. In der folgenden Übung wird der Zusatnd eines Tasters abgefragt und der Zustand über die LED und die Konsole angezeigt.

Es werden neben Bauteile aus den beiden vorhergehenden Übungen ein Taster und ein Pull Up Widerstand benötigt. Der Taster wird am Analog Eingang A0 angeschlossen, die LED wie vorher auch an Pin 13. 

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/03-code-button.js
```

Drücke `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* 5mm LED 
* Taster 10x10mm
* 680 Ohm Widerstand (blau-grau-braun)
* 2,2 kOhm Widerstand (rot-rot-rot)

## Schaltplan und Verdrahtung

Die Verdrahtung:

![Verdrahtung](%assets_url%/circ/03-LED-Button_Steckplatine.png "Verdrahtung")

Der Schaltplan:

![Schaltplan](%assets_url%/circ/led-schematic.png "Schaltplan")
![Schaltplan](%assets_url%/circ/button-schematic.png "Schaltplan")

## Programm

Das Javascript Programm befindet sich unter `code/03-code-button.js`

```javascript
var five = require("johnny-five"),
    button, led;

five.Board().on("ready", function() {

  button = new five.Button({
    pin: "A0",
    invert: true  // Taster ist active low
  });
 
  led = new five.Led(13);

  button.on("up", function(){
    led.off();
    console.log("up");
  });

  button.on("hold", function(){
    console.log("hold");
  });

  button.on("down", function(){
    led.on();
    console.log("down");
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
