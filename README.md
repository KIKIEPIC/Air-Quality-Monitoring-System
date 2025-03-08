
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <SoftwareSerial.h>
#include "PMS.h"


LiquidCrystal_I2C lcd(0x27, 20, 4); 


SoftwareSerial pmsSerial(2, 3);
PMS pms(pmsSerial);
PMS::DATA data;

const int ledGreen = 6;   
const int ledYellow = 7;  
const int ledRed = 8;     
const int buzzerPin = 9; 

void setup() {
  Serial.begin(9600);       
  pmsSerial.begin(9600);    
  lcd.init();              
  lcd.backlight();        
  

  pinMode(ledGreen, OUTPUT);
  pinMode(ledYellow, OUTPUT);
  pinMode(ledRed, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  

  digitalWrite(ledGreen, LOW);
  digitalWrite(ledYellow, LOW);
  digitalWrite(ledRed, LOW);
  digitalWrite(buzzerPin, LOW);
  

  lcd.setCursor(0, 0);
  lcd.print("Air Quality Monitor");
  delay(2000);
  lcd.clear();
}

void loop() {
 
  if (pms.read(data)) {
   
    float pm2_5 = data.PM_AE_UG_2_5;
    
   
    Serial.print("PM2.5: ");
    Serial.print(pm2_5);
    Serial.println(" ug/m3");
    
    
    lcd.setCursor(0, 0);
    lcd.print("PM2.5: ");
    lcd.print(pm2_5);
    lcd.print(" ug/m3    ");
    
    
    digitalWrite(ledGreen, LOW);
    digitalWrite(ledYellow, LOW);
    digitalWrite(ledRed, LOW);
    digitalWrite(buzzerPin, LOW);
    
    
    if (pm2_5 <= 35) {
      digitalWrite(ledGreen, HIGH);
      digitalWrite(buzzerPin, HIGH);
    }
    else if (pm2_5 > 35 && pm2_5 <= 75) {
      digitalWrite(ledYellow, HIGH);
    }
    else {
      digitalWrite(ledRed, HIGH);
      digitalWrite(buzzerPin, HIGH);
    }
    
    delay(1000); 
  }
}
