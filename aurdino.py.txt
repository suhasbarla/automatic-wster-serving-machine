#define in1 10
#define in2 9
#define in3 8
#define in4 7
//#define enA  6
//#define enB 6
#define relay 13
#define ls 11
#define rs 12
const int trigPin1=2;
const int echoPin1=3;
const int trigPin2=4;
const int echoPin2=5;
int M1_Speed = 80; // speed of motor 1
int M2_Speed = 80; // speed of motor 2
int LeftRotationSpeed = 100; // Left Rotation Speed int RightRotationSpeed = 100; // Right Rotation Speed long duration1;
long duration2;
int distance1;
int distance2;
void setup()
{
Serial.begin(9600);
  pinMode(in1,OUTPUT);
  pinMode(in2,OUTPUT);
  pinMode(in3,OUTPUT);
  pinMode(in4,OUTPUT);
  pinMode(relay,OUTPUT);
  pinMode(ls,INPUT);
  pinMode(rs,INPUT);
 //   pinMode(enA,OUTPUT);
  //  pinMode(enB,OUTPUT);
// pinMode(A0, OUTPUT); // initialize Left sensor as an input //pinMode(A1, INPUT); // initialize Right sensor as an input
  pinMode(trigPin1,OUTPUT);
  pinMode(echoPin1,INPUT);
  pinMode(trigPin2,OUTPUT);
  pinMode(echoPin2,INPUT);
}
void loop() {
 digitalWrite(trigPin1,LOW);
 delayMicroseconds(2);
 digitalWrite(trigPin1,HIGH);
 delayMicroseconds(10);
 digitalWrite(trigPin1,LOW);
 duration1 = pulseIn(echoPin1,HIGH);
 distance1=duration1*0.034/2;
  Serial.println(distance1);
//horizontal distance
 
   digitalWrite(trigPin2,LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin2,HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin2,LOW);
    duration2 = pulseIn(echoPin2,HIGH);
    distance2=duration2*0.034/2;
    Serial.println( distance2 );
    //vertical distance
    if((distance1 == 44 ||distance1 == 46 ||distance1== 45)&& (distance2 >6)) //we can change the distance 30 and 11 as per our need
    {
        Stop();Serial.println("stop"); digitalWrite(relay,LOW);
        Serial.println("water pump on");//water pump on
    }
    else if((distance1 != 44 && distance1 !=46 && distance1!=45) || (distance2 <6))
    {
        digitalWrite(relay,HIGH); //water pump off Serial.println("water pump off");
        move(); //moves forword
    } 
}
void move() {
int LEFT_SENSOR=digitalRead(ls);
int RIGHT_SENSOR=digitalRead(rs); 
if(RIGHT_SENSOR==0 && LEFT_SENSOR==0)
{
      forward(); //FORWARD
      Serial.println("forword");
}
 
else if(RIGHT_SENSOR==0 && LEFT_SENSOR==1) { 
    right(); //Move Right Serial.println("right");
}
else if(RIGHT_SENSOR==1 && LEFT_SENSOR==0) { 
    left(); //Move Left Serial.println("left");
}
else if(RIGHT_SENSOR==1 && LEFT_SENSOR==1) {
      Stop();  //STOP
      Serial.println("stop");
  }
}
void forward()
{
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
   analogWrite(enA, M1_Speed);
   analogWrite(enA, M2_Speed);
}
void backward()
{
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    analogWrite(enA, M1_Speed);
    analogWrite(enA, M2_Speed);
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    analogWrite(enA, M1_Speed);
 
}
void right()
{
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    analogWrite(enA, LeftRotationSpeed); analogWrite(enA, RightRotationSpeed);

}
void left() {
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    analogWrite(enA, LeftRotationSpeed); analogWrite(enA, RightRotationSpeed);
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    analogWrite(enA, LeftRotationSpeed); analogWrite(enA, RightRotationSpeed);
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, LOW);
}
void Stop() {
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    analogWrite(enA, LeftRotationSpeed); analogWrite(enA, RightRotationSpeed);
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    analogWrite(enA, LeftRotationSpeed); analogWrite(enA, RightRotationSpeed);
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, LOW);
}