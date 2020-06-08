#define F_CPU 8000000
/**
  Quick hack to flip the 5v out from Wideband sensor to 1v input for narrow band only spitronics ECU 
  Runs on Digispark ATTINY85 board with RC low pass filter thrown in on output just in case. 

*/
int ledPin =1 ;
int analogInPin = A1;
int analogOutPin = 1;
float value;
const float MinimumValue = 10.1;
const float MaximumValue = 244.9;

void setup() {


    /*
    Control Register A for Timer/Counter-0 (Timer/Counter-0 is configured using two registers: A and B)
    TCCR0A is 8 bits: [COM0A1:COM0A0:COM0B1:COM0B0:unused:unused:WGM01:WGM00]
    2<<COM0A0: sets bits COM0A0 and COM0A1, which (in Fast PWM mode) clears OC0A on compare-match, and sets OC0A at BOTTOM
    2<<COM0B0: sets bits COM0B0 and COM0B1, which (in Fast PWM mode) clears OC0B on compare-match, and sets OC0B at BOTTOM
    3<<WGM00: sets bits WGM00 and WGM01, which (when combined with WGM02 from TCCR0B below) enables Fast PWM mode
    */
     TCCR0A = 2<<COM0A0 | 2<<COM0B0 | 3<<WGM00;
      /*
    Control Register B for Timer/Counter-0 (Timer/Counter-0 is configured using two registers: A and B)
    TCCR0B is 8 bits: [FOC0A:FOC0B:unused:unused:WGM02:CS02:CS01:CS00]
    0<<WGM02: bit WGM02 remains clear, which (when combined with WGM00 and WGM01 from TCCR0A above) enables Fast PWM mode
    1<<CS00: sets bits CS01 (leaving CS01 and CS02 clear), which tells Timer/Counter-0 to not use a prescalar
    */
     TCCR0B = 0<<WGM02 | 1<<CS00;
       
    /*
    Control Register for Timer/Counter-1 (Timer/Counter-1 is configured with just one register: this one)
    TCCR1 is 8 bits: [CTC1:PWM1A:COM1A1:COM1A0:CS13:CS12:CS11:CS10]
    0<<PWM1A: bit PWM1A remains clear, which prevents Timer/Counter-1 from using pin OC1A (which is shared with OC0B)
    0<<COM1A0: bits COM1A0 and COM1A1 remain clear, which also prevents Timer/Counter-1 from using pin OC1A (see PWM1A above)
    1<<CS10: sets bit CS11 which tells Timer/Counter-1  to not use a prescalar
    */
     TCCR1 = 0<<PWM1A | 0<<COM1A0 | 1<<CS10;
     GTCCR = 1<<PWM1B | 2<<COM1B0;
     
      pinMode(analogInPin, INPUT);
      pinMode(analogOutPin, OUTPUT);
      pinMode(analogInPin, INPUT_PULLUP);
      pinMode(analogOutPin, INPUT_PULLUP);
     
}

void loop () {
  //analogWrite(ledPin, 128);
  int input  = analogRead(analogInPin);
  
  //get the ratio
  float pwm = (float)input/1023;   
  //Invert the value and 
  pwm = (1-pwm)*255;
  //5v in must be 1v out
  pwm = pwm * 1/5;
  
  analogWrite(analogOutPin,(unsigned int)pwm);
}
