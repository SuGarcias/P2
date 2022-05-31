# PRACTICA 2 : INTERRUPCIONES
## Parte B: Interrupción por timer

### CÓDIGO:

```cpp
#include <Arduino.h>
volatile int interruptCounter;
int totalInterruptCounter;
hw_timer_t * timer = NULL;
portMUX_TYPE timerMux = portMUX_INITIALIZER_UNLOCKED;
void IRAM_ATTR onTimer() {
portENTER_CRITICAL_ISR(&timerMux);
interruptCounter++;
portEXIT_CRITICAL_ISR(&timerMux);
}
void setup() {
Serial.begin(115200);
timer = timerBegin(0, 80, true);
timerAttachInterrupt(timer, &onTimer, true);
timerAlarmWrite(timer, 1000000, true);
timerAlarmEnable(timer);
}
void loop() {
  if (interruptCounter > 0) {
    portENTER_CRITICAL(&timerMux);
    interruptCounter--;
    portEXIT_CRITICAL(&timerMux);
    totalInterruptCounter++;
    Serial.print("An interrupt as occurred. Total number: ");
    Serial.println(totalInterruptCounter);
  }
}

```

### FUNCIONAMIENTO:
En esta parte de la practica 2,dentro del loop, el propio programa ejecutará una interrupción por timer (cada determinado tiempo).
En concreto, nuestro programa hace una interrupción cada segundo.
Cada vez que se produce una interrupción (cada segundo), se muestra por pantalla el texto: "An interrupt as occurred. Total number: ", el cual indica que ha habido una interrupción y el la suma total de interrupciones hasta el momento.


```cpp
void loop() {
  if (interruptCounter > 0) {
    portENTER_CRITICAL(&timerMux);
    interruptCounter--;
    portEXIT_CRITICAL(&timerMux);
    totalInterruptCounter++;
    Serial.print("An interrupt as occurred. Total number: ");
    Serial.println(totalInterruptCounter);
  }
}
```
Al ejecutar por pantalla, como ya hemos dicho, muestra que ha habido una interrupción y la suma de estas hasta el momento.

![alt text](captura.JPG)