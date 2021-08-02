# LED weitere Funktionen

In dieser Übung lernen wie ein paar weitere Funktionen um eine LED anzusteuern.

Es werden die selben Bauteile benötigt wie im vorangegangenen Beispiel. Auch der Aufbau identisch. Es wird lediglich ein PWM-fähiger Pin benötigt. Beim Arduino Micro ist im Gegensatz zum Arduino Uno auch Pin 13 PWM fähig, ebenso wie Pin 11, 10, 9, 6, 5 und 3. 

**Vor dem Zusammenbau ist es notwendig das Arduino Board vom PC zu trennen. Das ist ebenso immer notwendig, wenn Bauteile entfernt oder dazugefügt werden.**

Wenn alles zusammenbaut ist, kann der Arduino wieder mit dem PC verbunden werden. Danach wird das Programm aus dem `node-ardx-de` Verzeichnis über die folgende Kommadozeile gestarted:

```shell
node code/02-code-led-pwm.js`
```

Über die Kommandozeile kann die LED auch über REPL Befehle (read–eval–print-loop) gesteuert werden. Gib dazu nach dem `>>` prompt `led.stop()` ein um die LED Animation zu stoppen. Danach kannst du Befehle wie  `led.on()`, `led.off()` ausprobieren. Drücke `Control-D` um das Program zu beenden.

## Teileliste

* Arduino Micro
* Steckplatine
* Drahtbrücken Set
* 5mm LED 
* 680 Ohm Widerstand (blau-grau-braun)

## Schaltplan und Verdrahtung

Die Verdrahtung:

![Verdrahtung](%assets_url%/circ/01-LED_Steckplatine.png "Verdrahtung")

Der Schaltplan:

![Schaltplan](%assets_url%/circ/led-schematic.png "Schaltplan")

## Programm

Das Javascript Programm befindet sich unter `code/02-code-led-pwm.js`

```javascript
var five = require("johnny-five"),
    button, led;

five.Board().on("ready", function() {

  led = new j5.Led(13);

  led.pulse();

  // make myLED available as "led" in REPL

  this.repl.inject({
  	led: led
  });
	  
  // try "on", "off", "toggle", "brightness",
  // "fade", "fadeIn", "fadeOut", 
  // "pulse", "strobe", 
  // "stop" (stops strobing, pulsing or fading)
});
```

## Fehlersuche

### LED leuchtet nicht

Dioden sind gepolte Bauelemente. D.h. sie funktionieren nur in eine Richtung. Versuche die LED um 180° zu drehen (keine Sorge, die LED geht nicht kaputt, wenn sie falsch gepolt eingebaut wurde).


###  Das Programm meldet 'No USB devices detected'

Stelle sicher, dass das Arduino mit dem Computer über USB verbunden ist.

### es funktioniert immer noch nicht

Es kommt vor das Johnny-Five nicht mit dem Arduino über den USB COM Port kommunizieren kann. Stelle sicher, das die Arduino IDE beendet wurde. Wenn das Probelm immer noch besteht, kann man das Programm so ändern, dass Johnny-Five den richtigen USB COM Port verwendet:

```javascript
var board = new five.Board({port:'COM7'});
```

## Programm erweitern

### Fade In Fade Out:

Die LED lässt sich langsam ein und wiederausblenden mit myLed.fadeIn(): bzw. myLed.fadeOut():

```javascript
led.fadeIn(500);
```

 bzw. myLed.fadeOut():
 	
```javascript
led.fadeOut(500);
```

## Mehr Informationen

* [Johnny-Five LED Beispiele](http://johnny-five.io/examples/led/)
* [Wikipedia PWM](https://de.wikipedia.org/wiki/Pulsweitenmodulation)
