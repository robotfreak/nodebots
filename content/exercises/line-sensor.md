## Linien Sensor

Nit einem Linien Sensor kann ein Roboter einer Linie auf dem Boden folgen. Wichtig ist dabei das sich die Linie vom Untergrund hervorhebt. Eine schwarze Linie auf weissem Untergrund wäre z.B. ideal.

Ein Linien Sensor besteht üblicherweise aus einem oder mehreren Infrarot Reflexkoppler. Hinter diesem etwas kryptischen Namen versteckt sich eine Art Lichtschranke. In dem Sensor stecken eine Lichtquelle (Infrarot Diode) und ein Lichtempfänger (Foto-Transistor) in einem gemeinsamen Gehäuse. Der Foto-Transistor empfängt das Licht der Infrarot Diode nur, wenn das Licht reflektiert wird. Da dunkle Farben das Licht schlechter reflektieren als helle, hängt der Wert der Ausgangsspannung am Transistor von der Farbe der Oberfläche ab. 

Mit einem einzelnen Sensor kann der Roboter bereits einer Linie folgen, zumindest fast. Es ist mehr ein herumeiern an der Grenze zwischen Linie und Untergrund. Man spricht hier eher einem Kantenfolger als von einem Linienfolger. Der Roboter verliert die Linie, steuert gegen bis er die Linie wiederfindet, verliert sie wieder usw. Deshalb der Eiertanz 

Mehrere Sensoren dieser Art lassen sich zu einem Sensor Array zusammenfassen. Damit kann der Roboter genau erkennen, wie weit weg sich die Linie von der Mittelachse des Roboters befindet und kann somit stärker gegensteuern, wenn sich die Linie unter den äußeren Sensoren befindet.

Normalerweise benötigen Linien-Sensoren zum Anschluss an den Mikrocontroller Analog Eingänge. Falls es analogen Eingängen mangelt, gibt es auch Linien Sensoren mit i2C Anschluss, wie das Sparkfun Line Sensor Array (8x Linen Sensor).

![Sparfun Line Sensor Array](%assets_url%/line-sensor.jpg "Line Sensor Array")

### Was wird benötigt?

* [Sparkfun Line Sensor Array](https://www.sparkfun.com/products/13582)

### Schaltung

![Verdrahtung](%assets_url%/circ/line-sensor_Steckplatine.png "Verdrahtung")


### Programm

Das Sparkfun Line Sensor Array wird leider nicht direkt von der Johny-Five Bibliothek unterstützt. Deshalb findet im Programm die Abfrage des Linien-Sensors direkt mit I2C Funktionen statt. Die Ausgabe der Sensorwerte erfolgt im Binärformat.

```javascript
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function() {

  var register = {
        PULLUPB: 0x06,
        PULLUPA: 0x07,
        DIRB: 0x0E,
        DIRA: 0x0F,
        DATAB: 0x10,
        DATAA: 0x11,
        INTMASKA: 0x13,
        MISC: 0x1E,
        CLOCK: 0x1F,
        RESET: 0x7D
  };

  var address = 0x3E;
  console.log("address: ", address);

  this.i2cConfig();
  // Reset
  this.i2cWriteReg(address, register.RESET, 0x12);
  this.i2cWriteReg(address, register.RESET, 0x34);
  this.i2cReadOnce(address, register.INTMASKA, 2, function(data) {
    var value;
    value = (data[0] << 8) + data[1];
    console.log("read: ", value);
  });
  // Port A all Input
  this.i2cWriteReg(address, register.DIRA, 0xFF);
  // Port B Input 0,1 output, 2..7 Input
  this.i2cWriteReg(address, register.DIRB, 0xFC);
  // Port B IR LED off, FB LED on
  this.i2cWriteReg(address, register.DATAB, 0x01);
  // Port B IR LED on, FB LED off
  //this.wait(1, function() { 
  this.i2cWriteReg(address, register.DATAB, 0x02); 
  //});

  board.loop(1000, function() {
    //this.wait(2, function() { 
      // Port B IR LED on, FB LED on
    this.i2cWriteReg(address, register.DATAB, 0x00); 
    //;
    this.i2cReadOnce(address, register.DATAA, 1, function(data) {
      var value;
      value = data[0];
      console.log("read: ", value.toString(2));
    });
    // Port B IR LED off, FB LED off
    //this.i2cWriteReg(address, register.DATAB, 0x03);  
  });
});
```

### Übungen

Bastle dir eine Linienfolger Vorlage. Dazu eignet sich ein weisses Blatt Papier und schwarzes Klebeband. Klebe eine Linie Klebeband auf das Papier, fertig. Bewege das Papier uner dem Sensor und beobachte die ausgegebenen Sensorwerte

### Mehr Informationen

* [Johnny-Five IR Reflectance Array Beispiel](http://johnny-five.io/examples/ir-reflect-array/)
* [Johnny-Five Linienfolger Beispiel](http://johnny-five.io/examples/line-follower/)
