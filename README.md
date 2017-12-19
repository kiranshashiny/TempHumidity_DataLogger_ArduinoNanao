# TempHumidity_DataLogger_ArduinoNanao

This git repo tells how to log temperature and humidity sensor data to a SD Card 
 when connected to an Arduino Nano

http://lab.dejaworks.com/arduino-nano-sd-card-connection/

<img width="864" alt="screen shot 2017-12-19 at 3 28 06 pm" src="https://user-images.githubusercontent.com/14288989/34151450-3fc4be16-e4d1-11e7-8708-4710e06caa1b.png">

Arduino Nano Code is
```

/*
  SD card datalogger

 This example shows how to log data from three analog sensors
 to an SD card using the SD library.

 The circuit:
 * analog sensors on analog ins 0, 1, and 2
 * SD card attached to SPI bus as follows:
 ** MOSI - pin 11
 ** MISO - pin 12
 ** CLK - pin 13
 ** CS - pin 4

 created  24 Nov 2010
 modified 9 Apr 2012
 by Tom Igoe

 This example code is in the public domain.

 */

#include <SPI.h>
#include <SD.h>
#include<dht.h>
dht DHT;

#define DHT11_PIN 3

const int chipSelect = 4;

void setup() {
  // Open serial communications and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }


  Serial.print("Initializing SD card...");

  // see if the card is present and can be initialized:
  if (!SD.begin(chipSelect)) {
    Serial.println("Card failed, or not present");
    // don't do anything more:
    return;
  }
  Serial.println("card initialized.");
  
}

void loop() {
  // make a string for assembling the data to log:
  String dataString = "";
  
  int chk = DHT.read11(DHT11_PIN);
  //int sensor = analogRead(analogPin);
  dataString += String(DHT.temperature);
   

  // open the file. note that only one file can be open at a time,
  // so you have to close this one before opening another.
  File dataFile = SD.open("datalog.txt", FILE_WRITE);

  // if the file is available, write to it:
  if (dataFile) {
    dataFile.println(dataString);
    dataFile.close();
    // print to the serial port too:
    Serial.println(dataString);
  }
  // if the file isn't open, pop up an error:
  else {
    Serial.println("error opening datalog.txt");
  }
  delay (1000);
}



```
