# JavaScript Einführung

Es sind keine Programmierkenntnisse erforderlich um sich durch die Übungen zu arbeiten. Dieses Tutorial zeigt die Grundkenntnisse von JavaScript die benötigt wird um NodeBots zu programmieren.

Für weitere Details siehe die [MDN JavaScript Referenz](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) und die <a href="https://github.com/rwaldron/johnny-five">Johnny-Five</a> Dokumentation.


## Variablen

```javascript
var myVar;
```
    
Use the comma operator to declare multiple variables in a single declaration e.g.

```javascript
var x, y;
```

## Variablen Zuweisung

```javascript
var myVar = 0;
```

## Daten-Typen

```javascript
var myInt = 0;
var myFloat = 2.5;
var myBoolean = true;
var myString = "This is a string";
var myArray = [1,2,3];
var myJSONObject = {
  	myField : "Some value"
};
// Object instantiation using new
var myDate = new Date();
```

## Operatoren

Numerische Operatoren beinhalten + (addition) - (subtraction) / (division) * (multiplication) % (modulo)

```javascript
var x = 4;
x = x + 1; // x is 5
y = x % 2; // y is 1
x++; // x is 6
x--; // x is 5
x += 3; // x is 8
var aString = "The value of x is " + x;
```

Der `this` Operator referenziert den aktuell ausgeführten Kontext

Der `typeof` Operator gibt den Daten-Typ zurück
    
```javascript
typeof aString; // returns "string"
```

## Mathematik

```javascript
Math.floor(myInt);
Math.ceil(myInt);
Math.min(x, y);
Math.random(); // random number between 0 and up to but not equal to 1
Math.PI;
```

## Kommentare

```javascript
// Comment until the end of the line
/* 
 *  Comment block
 */
```

## Ausgabe auf die Konsole

```javascript
console.log("Hello World");
```

## Bedingte Ausführung

Benutze Vergleichs Operatoren < (kleiner als) > (größer als) <= (kleiner oder gleich) >= (größer oder gleich) == (gleich) != (ungleich) 

 ```javascript
if (x > 0) {
    // do something
} else {
    // do something else
}
```

#### Bedingter Operator:

Der bedingte Operator bietet eine kleine Abkürzungsmethode für die Abfrage einer Bedingung (if-then-else): `(Bedingung ? wahr : falsch )` d.h..

```javascript
var myString = "I have " + (x == 1 ? x + "thing" : x + "things");
```

## Schleifen

```javascript
for (var i = 0; i < myArray.length; i++) {
   	console.log(myArray[i]);
}

while (x < 10) {
   	console.log(x++);
}
```
## Funktionen

```javascript
function myIncrementFunction (x) {
  	return x + 1;
}
```

## Aufruf von Funktionen

```javascript
myIncrementFunction(1000);
```

Funktionen die als Teil von Objekten definiert sind, nennt man Methoden. Methoden können wie folgt aufgerufen werden:
   
```javascript
myDate.getFullYear();
```

## Ausnahme Behandlung

Ausnahmen sind Fehler die in der Anwendung auftreten können, und normalerweise die Anwendung beenden. Vvorhersehbare Fehler können angefangen werden, ohne das Programmes zu beenden.

```javascript
try {
    // try to do something
} catch (e) {
    // handle errors
} finally {
    // this block is executed regardless of whether there was an exception
}
```

## Require

Fügt eine Bibliothek zu einem node.js Programm hinzu

```javascript
var five = require("johnny-five");
var myLib = require("some-other-lib");
```

## Behandlung von Events

Benutze `on` für eine Callback Funktion, die aufgerufen wird, wenn ein Event auftritt:

```javascript
var myBoard = new five.Board();
myBoard.on("ready", function() {
  	// code to be executed when the board is ready
});
```

