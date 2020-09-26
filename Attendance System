#include <Adafruit_Fingerprint.h>
#include<LiquidCrystal.h>
#include <SD.h>
#include <SPI.h>
#include <DS3231.h>
LiquidCrystal lcd(8,7,5,4,6,9);
SoftwareSerial mySerial(2, 3);/* Pour le data logger */
#define CS_PIN 10/* Definissons la pin du cs */
String Nom,idt;
int id=0;
int state3,state1,state2,memoire, compte1;
int compte=1;
File monFichier;
DS3231  monRTC(SDA, SCL);
byte smiley[8] = {
  B00000,
  B10001,
  B00000,
  B10001,
  B01110,
  B00000,
};
Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);
void setup()  {
while (!Serial);
pinMode(A1,INPUT_PULLUP);
pinMode(A2,INPUT_PULLUP);
pinMode(A3,INPUT_PULLUP);
lcd.begin(16,2);
Serial.begin(9600);
pinMode(A0,OUTPUT);
delay(100);
finger.begin(57600);
monRTC.begin();
if (SD.begin());
lcd.createChar(0,smiley);

memoire=HIGH;
monFichier = SD.open("DONNEES.csv", FILE_WRITE);
if(monFichier){
monFichier.println();
monFichier.close();

}

}
void chatbot(){
  lcd.clear();
  lcd.setCursor(2,0);
  
  lcd.print("Please Scan");
  
  lcd.setCursor(2,1);
  lcd.print("your finger");
}
void loop(){
  state3=digitalRead(A3);
  state1=digitalRead(A1);
  state2=digitalRead(A2);
  if(state3==0){
   SelectionEnroll();
      delay(300);
      
   }

  chatbot();
  getFingerprintIDez();
  delay(50); //don't need to run this at full speed.  
  }
  


uint8_t getFingerprintID() {
  uint8_t p = finger.getImage();
  switch (p) {
    case FINGERPRINT_OK:
    break;
    default:
    return p;
  } // OK success!

  p = finger.image2Tz(); // OK converted!
  p = finger.fingerFastSearch();    
  return finger.fingerID;
}// returns -1 if failed, otherwise returns ID #

int getFingerprintIDez() {
  uint8_t p = finger.getImage();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.image2Tz();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.fingerFastSearch();
  if (p != FINGERPRINT_OK)  return -1;
  
monFichier = SD.open("DONNEES.csv", FILE_WRITE);  // On enregistre la donnée ,Maximum 8 caractères avant le .csv
   
  if (monFichier) {
switch(finger.fingerID){
  case 1:
 Nom="User 1"; // index droit
 idt="GT0000A1";
 break;
 case 2:
 Nom="User 2"; // index droit
 idt="14SGRT42";
 break;
 case 3:
 Nom="User 3"; // majeur gauche
 idt="58NTRUFJ";
 break;
 case 4:
 Nom="User 4"; // majeur gauche
 idt="T876FKN5";
 break;
 case 5:
 Nom="User 5";
 idt="7RVG75T9";
 break;
 case 6:
 Nom="User 6";
 idt="RGJB0011";
 break;
 case 7:
 Nom="User 7";
 idt="93GPVOVP";
 break;
 case 8:
 Nom="User 8";
 idt="AKJ58LF5";
 break;
 case 9:
 Nom="User 9";
 idt="AF8KFJ7R";
 break;
 case 10:
 Nom="User 10";
 idt="A6LM2DR7";
 break;
 case 11:
 Nom="User 11";
 idt="2GD8LKFI";
 break;
 case 12:
 Nom="User 12";
 idt="QD215DTN";
 break;

 
}

// On demande l'heure exacte au module Real Time Clock.
 
String Heure = String(monRTC.getTimeStr());
String Date  = String(monRTC.getDateStr());

// On met en forme la donnée au formar csv, c'est-à dire chaque paramètre séparé par une virgule.

String DONNEES = Nom + "," + idt + "," + Date + "," + Heure ;

    
    tone(A0,1300);
    delay(250);
    noTone(A0);
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print(Nom);
    lcd.setCursor(0,1);
    lcd.print("ID:"+idt);
    delay(1000);
    monFichier.println(DONNEES);
    monFichier.close();
    } 
    delay(600);
    }
uint8_t getFingerprintEnroll() {
  
  int p = -1;
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Waiting.."); 
  while (p != FINGERPRINT_OK) {
    p = finger.getImage();
    switch (p) {
    case FINGERPRINT_OK:
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Img. taken"); 
    delay(1000);  
    break;
    case FINGERPRINT_NOFINGER:
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Waiting..");
    break;
    default:
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Error");
    delay(2000); 
    break;
    }
  }

  // OK success!

  p = finger.image2Tz(1);
  switch (p) {
    case FINGERPRINT_OK:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Img. taken");
  delay(2000); 
   break;
  default:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Error"); 
  delay(2000);
   return p;
  }
  
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Remove it");
  delay(2000);
  p = 0;
  while (p != FINGERPRINT_NOFINGER) {
    p = finger.getImage();
  }
  
  p = -1;
  lcd.setCursor(0,0);
  lcd.print("Place the same");
  while (p != FINGERPRINT_OK) {
    p = finger.getImage();
    switch (p) {
    case FINGERPRINT_OK:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Img. taken");
  delay(2000);
      break;
    case FINGERPRINT_NOFINGER:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Place the same");
      break;
    default:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Error");
      break;
    }
  }

  // OK success!

  p = finger.image2Tz(2);
  switch (p) {
    case FINGERPRINT_OK:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Img. converted");
  delay(2000);
    break;
    default:
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Error");
  delay(2000);
    return p;
  }// OK converted!
  p = finger.createModel();
  if (p == FINGERPRINT_OK) {
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Matched!");
  delay(2000);
  } else {
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Error");
    return p;
  }   
  
  
  p = finger.storeModel(id);
  if (p == FINGERPRINT_OK) {
  lcd.setCursor(0,1);
  lcd.print("Stored");
  delay(2000);
  

  } else {
  lcd.clear();
  lcd.setCursor(0,1);
  lcd.print("Error");
  delay(2000);
    return p;
  }
  
}

void SelectionEnroll(){
  while(1) 
   { 
   lcd.setCursor(0,0);
   lcd.print("Enroll of ID:");
    lcd.setCursor(0,1); 
     lcd.print(compte);
     lcd.print("              "); 
     if(digitalRead(A1) == 0) 
     { 
       compte++; 
       if(compte>15) 
       compte=1; 
       delay(300); 
     } 
     else if(digitalRead(A2) == 0) 
     { 
  id=compte; 
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Enroll of ID#");
  lcd.print (id);
  delay(2000);  
  while (! getFingerprintEnroll() );
  return;
           
     }
     
     }
}


