# stepper-motor
control by means of arduino uno stepper motor

// Помогите сделать управления скоростью шагового двигателя с помощью потонцеометра 
// ХАРАКТЕРИСТИКИ:
// Потонцеометр 10 Ком
// драйвер L298N
//  Help make the speed control of the stepper motor with a tachometer
// CHARACTERISTICS:
// Potometer 10 Kom
// driver L298N





#include <Stepper.h>
const int stepsPerRevolution = 200; // шагов на оборот вашего двигателя
//инициализировать библиотеку степпера на выводах с 8 по 11:
Stepper myStepper(stepsPerRevolution, 8,9,10,11);


// задаем константы
const int buttonPin1 = 2;     // номер входа, подключенный к кнопке1
const int buttonPin2 = 4;     // номер входа, подключенный к кнопке2

// переменные
int buttonState1 = 0;         // переменная для хранения состояния кнопки
int buttonState2 = 1;         // переменная для хранения состояния кнопки
 


void setup() {

   //инициализировать последовательный порт:
   Serial.begin(9600);

     // инициализируем пин, подключенный к кнопке1, как вход
  pinMode(buttonPin1, INPUT);   
    // инициализируем пин, подключенный к кнопке2, как вход
  pinMode(buttonPin2, INPUT);   
   }
   
void loop() {

  // считываем значения с входа кнопки1
  buttonState1 = digitalRead(buttonPin1);
  
  // считываем значения с входа кнопки2
  buttonState2 = digitalRead(buttonPin2);


    // считываем значение потенциометра:
  int sensorReading = analogRead(A2);
  // подгоняем считанное значение под диапазон от «0» до «100»:
  int motorSpeed = map(sensorReading, 0, 1023, 0, 200);

  
    // проверяем нажата ли кнопка
  // если нажата, то buttonState будет HIGH:
  if (buttonState1 == HIGH) {
   //шаг один оборот в одном направлении:
   Serial.println("clockwise");
       myStepper.setSpeed(motorSpeed);
   myStepper.step(stepsPerRevolution);
    }

    if (buttonState2 == HIGH) {
   //шаг один оборот в другом направлении:
   Serial.println("counterclockwise");
       myStepper.setSpeed(motorSpeed);
   myStepper.step(-stepsPerRevolution);
  }
   
}
