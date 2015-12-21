/*
 * 21-12-2015: prova interruzione timer
 *
 *
 */


#include "Pwm.h"
#include "Energia.h"
#include "Timer.h"

volatile static int conteggio = 0;
volatile static int movMot = 0;


void blink(){    //funzione che viene attivata sull'interrupt, interruzione timer A1
	conteggio++;
	movMot++;

}

Timer TA1;
Pwm P;
PwmServo SERVO; //non compatibile con Serial

void setup() {
  // put your setup code here, to run once:
    pinMode(GREEN_LED, OUTPUT);
    digitalWrite(GREEN_LED, HIGH);

    TA1.begin(100); //ta1 a 100ms
    TA1.attach(blink); //registra la routine di servizio
    // il timer genera l'interruzione
    TA1.enableInt();

    SERVO.setup(F_CPU, 50); //F_CPU = 25MHz, FPWM = 50Hz
    SERVO.pinSetting(P1_2);
    SERVO.pinValue(P1_2, 0);

    ///PWM MOTORI
    P.begin(125);  //periodo da 125us
    P.pinSetting(P2_4);
    P.pinSetting(P2_5);
    P.pinValue(P2_4, 0.0);
    P.pinValue(P2_5, 50.0);
}

void loop() {
  // put your main code here, to run repeatedly: 
  if (conteggio >= 5){
	  P4OUT ^= BIT7;
	  conteggio = 0;
  }
  if(movMot >= 20){
	  SERVO.pinValue(P1_2, -45);

  }
  else{
	  if((movMot > 20) && (movMot<= 40)){
	  SERVO.pinValue(P1_2, 45);
      }
        else

        	movMot = 0;
  }

}
