# Portable-Mini-Weather-Station
It is a compact device that allows users to measure the surrounding temperature, airpressure and humidity.

#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME280.h> 

Adafruit_BME280 bme; 

#define SCREEN_WIDTH 128 
#define SCREEN_HEIGHT 64 

#define OLED_RESET -1
#define SCREEN_ADDRESS 0x3C 
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

const unsigned char Weather_Logo [] PROGMEM = {
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0xf8, 0x70, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x01, 0xfc, 0x50, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x06, 0x70, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x86, 0x0f, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x1b, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0x02, 0x1b, 0x00, 0x00, 0x00, 0x01, 0x83, 0xf8, 0x30, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x18, 0x00, 0x00, 0xf0, 0x00, 0xcf, 0xfe, 0x60, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x18, 0x00, 0x00, 0xf8, 0x00, 0x5c, 0x07, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x02, 0x1b, 0x00, 0x00, 0xf8, 0x00, 0x30, 0x01, 0x80, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x1f, 0x00, 0x00, 0xf8, 0x00, 0x30, 0x00, 0xc0, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0x02, 0x0f, 0x00, 0x00, 0xf8, 0x00, 0x60, 0x00, 0xc0, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x60, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0x82, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x40, 0x00, 0x60, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0x02, 0x00, 0x00, 0x00, 0xf8, 0x3f, 0x40, 0x00, 0x7f, 0xc0, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0x72, 0x00, 0x00, 0x00, 0xf8, 0x3e, 0x40, 0x00, 0x4f, 0x80, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0xfa, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0xfa, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x60, 0x07, 0xf0, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x02, 0xfa, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x60, 0x1f, 0x3c, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x06, 0xfb, 0x00, 0x00, 0x00, 0xf8, 0x00, 0x30, 0x38, 0x06, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x0c, 0xf9, 0x80, 0xf8, 0x00, 0xf8, 0x00, 0x18, 0x70, 0x03, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x18, 0xf8, 0xc0, 0xf8, 0x00, 0xf8, 0x00, 0x4c, 0x60, 0x01, 0x80, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x10, 0xf8, 0x40, 0xf8, 0x00, 0xf8, 0x01, 0xc7, 0xc0, 0x00, 0x80, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x30, 0xf8, 0x60, 0xf8, 0x00, 0xf8, 0x01, 0x80, 0x80, 0x00, 0xc0, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x31, 0xfc, 0x60, 0xf8, 0x00, 0xf8, 0x00, 0x01, 0x80, 0x00, 0xc0, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x33, 0xfe, 0x20, 0xf8, 0x00, 0xf8, 0x00, 0x01, 0x80, 0x00, 0x78, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x33, 0xfe, 0x20, 0xf8, 0xf8, 0xf8, 0x00, 0x3f, 0x80, 0x00, 0x3e, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x33, 0xfe, 0x60, 0xf8, 0xf8, 0xf8, 0x00, 0x7f, 0x00, 0x00, 0x07, 0x78, 0x00, 0x00, 0x00, 
	0x00, 0x33, 0xfe, 0x60, 0xf8, 0xf8, 0xf8, 0x01, 0xc0, 0x00, 0x00, 0x01, 0xfe, 0x00, 0x00, 0x00, 
	0x00, 0x33, 0xfe, 0x60, 0xf8, 0xf8, 0xf8, 0x03, 0x80, 0x00, 0x00, 0x00, 0x83, 0x80, 0x00, 0x00, 
	0x00, 0x19, 0xfc, 0xc0, 0xf8, 0xf8, 0xf8, 0x03, 0x00, 0x03, 0xc0, 0x00, 0x01, 0xc0, 0x00, 0x00, 
	0x00, 0x0c, 0xf9, 0x80, 0xf8, 0xf8, 0xf8, 0x06, 0x00, 0x0f, 0xf0, 0x00, 0x00, 0xf8, 0x00, 0x00, 
	0x00, 0x07, 0x07, 0x00, 0xf8, 0xf8, 0xf8, 0x04, 0x00, 0x1c, 0x18, 0x00, 0x00, 0x7c, 0x00, 0x00, 
	0x00, 0x03, 0xfe, 0xf8, 0xf8, 0xf8, 0xf8, 0x0c, 0x00, 0x30, 0x0c, 0x00, 0x00, 0x06, 0x00, 0x00, 
	0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0xf8, 0x0c, 0x01, 0x30, 0x06, 0x00, 0x00, 0x03, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0x08, 0x07, 0xe0, 0x06, 0x00, 0x00, 0x03, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0x08, 0x1c, 0x60, 0x03, 0xc0, 0x00, 0x01, 0x80, 0x00, 
	0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0xf8, 0x0f, 0xd8, 0x00, 0x03, 0xe7, 0xff, 0xff, 0x80, 0x00, 
	0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0xf8, 0x00, 0x30, 0x00, 0x00, 0x38, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0xf8, 0x00, 0x70, 0x00, 0x00, 0x18, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0xf8, 0xf8, 0xf8, 0xf8, 0xf8, 0x01, 0xe0, 0x00, 0x00, 0x0c, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x03, 0x00, 0x00, 0x00, 0x06, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x01, 0xff, 0xff, 0xff, 0xff, 0xfc, 0x03, 0x00, 0x00, 0x00, 0x06, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0xff, 0xff, 0xff, 0xff, 0xfe, 0x03, 0xff, 0xff, 0x6f, 0xfe, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x03, 0xff, 0xff, 0xff, 0xff, 0xfe, 0x03, 0xff, 0xff, 0x2f, 0xfe, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
	0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
};

void setup() {


if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
  Serial.println(F("SSD1306 allocation failed"));
  while (1);
}

display.clearDisplay();
display.drawBitmap(7, -3, Weather_Logo, 128, 64, WHITE);
display.display();

if (!bme.begin(0x76)) { 
  Serial.println(F("Could not find a valid BME280 sensor, check wiring!"));
  while (1);
}

delay(1500); 

}

void loop() {


bme.begin(0x76);

float temperature = bme.readTemperature();
float humidity = bme.readHumidity();
float pressure = bme.readPressure() / 100.0F; 

bme.begin(0x77);


display.clearDisplay(); 

display.setTextSize(1); 
display.setTextColor(SSD1306_WHITE);  
display.setCursor(10, 13);      
display.print("TEMP : ");
display.setCursor(48, 13); 

display.print(temperature);

display.setCursor(75, 13);
display.println("    C");
display.drawCircle(95, 12, 1, WHITE);

display.setTextSize(1); 
display.setTextColor(SSD1306_WHITE);  
display.setCursor(10, 27);      
display.print("PRSR : ");
display.setCursor(48, 27); 

display.print(pressure);

display.setCursor(75, 27);
display.println("    hPa");

display.setTextSize(1); 
display.setTextColor(SSD1306_WHITE);  
display.setCursor(10, 43);      
display.print("HMDTY: ");
display.setCursor(48, 43); 

display.print(humidity);

display.setCursor(75, 43);
display.println("    %rh");

display.drawRect(7, 9, 112, 14, SSD1306_WHITE);
display.drawRect(7, 24, 112, 14, SSD1306_WHITE);
display.drawRect(7, 39, 112, 14, SSD1306_WHITE);

display.display(); 

}


