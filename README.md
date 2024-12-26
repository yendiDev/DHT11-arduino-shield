# Temperature and Humidity Sensor Arduino Shield

This Arduino UNO shield integrates a DHT11 temperature and humidity sensor together with a 16x2 LCD display for visualisation. It also includes a potentiometer for controlling the brightness of the LCD, and an LED which can be used in anyway one could imagine 

Below are screenshots of the PCB:

### Schematics:
![Capture](https://github.com/user-attachments/assets/32f90458-eeb4-43b0-b5f8-9ce68dc1e1c9)

### PCB Layout:
Front layer:
![nftgn](https://github.com/user-attachments/assets/b78d88f0-3d1c-4cc4-81cd-c262e0d0fada)

Bottom layer:
![dfgdf](https://github.com/user-attachments/assets/1153c214-48d4-48a6-ad9d-fb16cffc69aa)

### 3D view of PCB:
![hrfdtg](https://github.com/user-attachments/assets/e3d410b4-0563-4027-9722-c77b716a6459)

![gdxf](https://github.com/user-attachments/assets/a860aac7-8438-447a-882d-ae05c1cf10e5)

![gdbff](https://github.com/user-attachments/assets/086ff8c9-2b7b-40a0-8274-11ef5eb6a52e)

![gdfxdx](https://github.com/user-attachments/assets/ef264607-ed77-494f-818d-7c2ff4586870)

![gdrf](https://github.com/user-attachments/assets/af1ea2f7-c73c-4426-b60e-db77eae82edc)


## Programming the Shield

```C++
#include <DHT.h>
#include <LiquidCrystal.h>

// pin definitions
#define DHT_PIN 2          // dht11 sensor pin
#define LED_PIN 13         // led indicator pin
#define DHTTYPE DHT11      // dht11 sensor type

// lcd pins
const int rs = 11, en = 12;
const int d4 = 5, d5 = 4, d6 = 3, d7 = 2;

// initialize components
DHT dht(DHT_PIN, DHTTYPE);
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
  // initialize serial communication
  Serial.begin(9600);
  
  // initialize the dht sensor
  dht.begin();
  
  // set up the lcd
  lcd.begin(16, 2);  // 16x2 lcd
  
  // initialize led pin
  pinMode(LED_PIN, OUTPUT);
  
  // initial lcd message
  lcd.print("Temp & Humidity");
  lcd.setCursor(0, 1);
  lcd.print("Initializing...");
  
  // wait for sensor to stabilize
  delay(2000);
}

void loop() {
  // read temperature and humidity
  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();
  
  // check if reads failed and exit early (will try again)
  if (isnan(humidity) || isnan(temperature)) {
    lcd.clear();
    lcd.print("Sensor Error!");
    digitalWrite(LED_PIN, HIGH);  // led on indicates error
    delay(2000);
    return;
  }
  
  // clear led if reading successful
  digitalWrite(LED_PIN, LOW);
  
  // update lcd
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperature, 1);
  lcd.print((char)223);  // degree symbol
  lcd.print("C");
  
  lcd.setCursor(0, 1);
  lcd.print("Humidity: ");
  lcd.print(humidity, 1);
  lcd.print("%");
  
  // also send to serial monitor
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.println("%");
  
  // wait before next reading
  delay(2000);
}
```
