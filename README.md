# Project
MiniProject

This is MiniProject that I have done fro the Embedded Systems Module. Intelligent Traffic Light Controllers。

Code is below：

#define ECHOPIN 3
#define TRIGPIN 2
char inVal = '0'; 
unsigned int sensorValue = 0;
float d;
int sensorPin = A0;
void setup()
{
  Serial.begin(9600);
  pinMode(ECHOPIN, INPUT);
  pinMode(TRIGPIN, OUTPUT);
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(11, OUTPUT);
}
void loop() 
{ 
  if ( inVal== '0')
  do{
    normal_mode();
    if (Serial.available()>0)
    {
      inVal=Serial.read();
    }
    Serial.println(inVal);

  }
  while(inVal== '0');

  if(inVal=='1')
  do{
    senor_reading();
    pedestrian_mode();
    if (Serial.available()>0)
    {
      inVal=Serial.read();
    }
    Serial.println(inVal);

  }
  while(inVal== '1');

  if ( inVal=='2')
  {  
    do 
    {
      senor_reading();
      if (Serial.available()>0)
      {
        inVal=Serial.read();
      }
      Serial.println(inVal);
      if (sensorValue<60)
      {

        digitalWrite(13,HIGH);
        digitalWrite(12,LOW);
        digitalWrite(11,LOW);
        if(d<10)
        {
          digitalWrite(13,LOW);
          digitalWrite(11,HIGH);
          digitalWrite(12,LOW);
        }
      }
      else 
      {
        normal_mode();
        if (Serial.available()>0)
        {
          inVal=Serial.read();
        }
        Serial.println(inVal);
      }
    }
    while (inVal=='2');
  }

}

void normal_mode(){
  digitalWrite(13, HIGH);   
  delay(2000);              
  digitalWrite(13, LOW);    
  digitalWrite(12, HIGH);   
  delay(1000);              
  digitalWrite(12, LOW);    
  digitalWrite(11, HIGH);
  delay(2000);   
  digitalWrite(11, LOW);    
  digitalWrite(12, HIGH);   
  delay(1000);              
  digitalWrite(12, LOW); 
}


void pedestrian_mode()
{
  if(d < 10 )
  { 

    for(int k=0;k<10;k++)
    {
      tone(8, 262);
      digitalWrite(12, HIGH);   
      delay(100);              
      digitalWrite(12, LOW);    
      delay(100);              
    }
    noTone(8);
  }
  else
  {
    normal_mode(); 
  }
}
void senor_reading()

{
  digitalWrite(TRIGPIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIGPIN,HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIGPIN,LOW); 
  d = pulseIn(ECHOPIN, HIGH);
  d = d/58;
  Serial.print(d);
  Serial.println("cm");
  delay(200);
  sensorValue = analogRead(sensorPin);     
  Serial.println(sensorValue);
  delay(100);
}


