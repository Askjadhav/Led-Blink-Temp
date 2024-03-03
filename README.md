//# Led-Blink-Temp
#include <TimerOne.h>

const int ledPin = 13; // Pin connected to the LED
bool ledState = LOW; // Variable to store the current LED state
bool isTemperatureHigh = false; // Variable to track temperature condition

void setup() {
  pinMode(ledPin, OUTPUT); // Initialize the LED pin as an output
  
  Timer1.initialize(1000); // Set up a timer with an interval of 1000 microseconds (1ms)
  Timer1.attachInterrupt(timerISR); // Attach the timer ISR (Interrupt Service Routine)
}

void loop() {
  int sensorValue = analogRead(A0);
  // Convert the analog reading (which goes from 0 - 1023) to a voltage (0 - 5V):
 float voltage = sensorValue * (5.0 / 1023.0);
  float temperature = voltage*100;
  // Your temperature reading code goes here
 // float temperature = readTemperature(); // Read temperature from sensor (you need to replace this with actual temperature reading code)

  // Check temperature condition
  if (temperature >= 30) {
    isTemperatureHigh = true;
  } else {
    isTemperatureHigh = false;
  }
}

void timerISR() {
  if (isTemperatureHigh)
   {
    toggleLED(500); // Toggle LED with 500ms delay if temperature is greater than or equal to 30°C
  } else
   {
    toggleLED(250); // Toggle LED with 250ms delay if temperature is less than 30°C
  }
}

void toggleLED(int delayTime)
{
  ledState = !ledState; // Toggle the LED state
  digitalWrite(ledPin, ledState); // Update the LED state
}
