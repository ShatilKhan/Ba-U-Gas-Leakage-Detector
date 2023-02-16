#include <dht_nonblocking.h>
#define DHT_SENSOR_TYPE DHT_TYPE_11
#include <LiquidCrystal.h>
#include "MQ7.h"

#define A_PIN 8
#define VOLTAGE 5

static const int DHT_SENSOR_PIN = 7;
DHT_nonblocking dht_sensor( DHT_SENSOR_PIN, DHT_SENSOR_TYPE );

MQ7 mq7(A_PIN, VOLTAGE);






// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
  Serial.begin(9600);
  while (!Serial) {
		;	// wait for serial connection
	}

	Serial.println(""); 	// blank new line

	Serial.println("Calibrating MQ7");
	mq7.calibrate();		// calculates R0
	Serial.println("Calibration done!");


lcd.begin(16, 2);
  lcd.print( "T = " );
  lcd.setCursor(10, 0);
  lcd.print( "deg. C" );
  lcd.setCursor(0, 1);
  lcd.print( "H = " );
  lcd.setCursor(10, 1);
  lcd.print( "%" );
  pinMode(10,INPUT);
  pinMode(13,INPUT);
  
  
}

static bool measure_environment( float *temperature, float *humidity )
{
  static unsigned long measurement_timestamp = millis( );

  /* Measure once every four seconds. */
  if( millis( ) - measurement_timestamp > 3000ul )
  {
    if( dht_sensor.measure( temperature, humidity ) == true )
    {
      measurement_timestamp = millis( );
      return( true );
    }
  }

  return( false );
}

/*
 * Main program loop.
 */
void loop( )
{ 
  
  float temperature;
  float humidity;
 //float gasData=mq7.readPpm();
  /* Measure temperature and humidity.  If the functions returns
     true, then a measurement is available. */
  if( measure_environment( &temperature, &humidity ) == true )
  {
   /*
    * Display  changing text.
    */

  
  if(Serial.available()>0)
  {
    char blue=Serial.read();

    switch(blue)
     {
       case 'a':digitalWrite(10,HIGH);break;
       case 'b':digitalWrite(10,LOW);break;
       case 'c':digitalWrite(13,HIGH);break;
       case 'd':digitalWrite(13,LOW);break;
     }
  }

   lcd.print( "T = " );
  lcd.setCursor(10, 0);
  lcd.print( "deg. C" );
  lcd.setCursor(0, 1);
  lcd.print( "H = " );
  lcd.setCursor(10, 1);
  lcd.print( "%" );
  
    lcd.setCursor(4, 0);
    lcd.print( temperature );
    lcd.setCursor(4, 1);
    lcd.print( humidity );
     
    delay(1000);
    lcd.clear();
    delay(100);
    
  
    lcd.setCursor(1,0); 
    if(digitalRead(10)==1)
    {
        lcd.print("CO Level");
    }
    else if(digitalRead(10)==0)
    {
      lcd.print("MQ4 level");
    }
    else if(digitalRead(13)==1)
    {
      lcd.print("MQ2 gas");
    }
    else if(digitalRead(13)==0)
    {
      lcd.print("MQ 6 gas");
    }
    else{
      lcd.print("not selected");
    }


 lcd.setCursor(1, 1);
    lcd.print(mq7.readPpm());
    lcd.setCursor(4,1);
    lcd.print("ppm");
    
     
    
    delay(4000);
    lcd.clear();
    delay(100);
    

   Serial.print("PPM = "); 
   Serial.println(mq7.readPpm());
   if(mq7.readPpm()<9)
     {
        Serial.println("Good quality");
     }
    else{
      Serial.println("Not Good ");
    }
  Serial.println("Temperature is ");
  Serial.print(temperature);
  Serial.println("Humidity is ");
  Serial.print(humidity);
	Serial.println(""); 	// blank new line
	delay(100);

  
   
        
  }
}
