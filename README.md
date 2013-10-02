project1
========
const int sensor1inputPin = 0; //the analog pin the sensor is attached to
const int motor1outputPin = 3; //the pin controlling the transistor
const int ledPin=6;//pin controlling the led

int sensor1RawValue=0;
int sensorMin=1023;
int sensorMax=0;//variable that will hold the raw value from the sensor

int sensor1ConvValue; // variable that will hold the converted output value

void setup()
{
//turn on led to signal the start of the calibration period
  pinMode(6,OUTPUT);
digitalWrite(6,HIGH);

//calibrate the first 5 seconds
while (millis()<5000) {
  sensor1RawValue=analogRead(sensor1inputPin);

//record the maximum sensor value  
  if(sensor1RawValue>sensorMax){
    sensorMax=sensor1RawValue;
  }
 
  //record the minimum sensor value
  if (sensor1RawValue<sensorMin){
    sensorMin=sensor1RawValue;
  }
}
//signal the end of the calibration period
digitalWrite(6,LOW);
  
pinMode(motor1outputPin, OUTPUT);   
pinMode(ledPin, OUTPUT);
Serial.begin(9600);
}


void loop()
{
//read the sensor
sensor1RawValue=analogRead(sensor1inputPin); 
//apply the calibration to the sensor reading
sensor1ConvValue=map(sensor1RawValue,sensorMin,sensorMax,0,255); 
//in case the sensor vaue is outside the range seen during calibration
sensor1ConvValue=constrain(sensor1RawValue,0,255);

if (sensor1RawValue>=150)//if the reading is below 150
{
  
analogWrite(motor1outputPin, 50);//write the value of motor speed
analogWrite(ledPin, 255);//value of led brightness
}
else
{
  analogWrite(motor1outputPin,0);//turn off motor in light
  analogWrite(ledPin,0);//turn off led in light
}
Serial.println(sensor1RawValue);//serial reading in line
}
