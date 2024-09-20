# PWM Clock
Use multimeter (voltage) as a clock. Creates a voltage with PWM signal from RPi Pico based on current time.

The basic code can be 

```
import time
from machine import Pin, PWM

PWM_PIN = 21 # For Raspberry Pi Pico

pwm_pin = PWM(Pin(PWM_PIN, mode=Pin.OUT)) # Attach PWM object on the LED pin
pwm_pin.freq(1_000)


def time_to_pwm(hour, minute, second=0):
    k = 2.43 if hour > 19 else 2.439;
    return round((hour*10000 + minute * 100 + second)/240000*(1<<16) * k/3.3)

def set_time(hour, minute, second=0):
    duty = time_to_pwm(hour, minute, second)
    pwm_pin.duty_u16(duty) # For Pi Pico

while True:
    tm = time.gmtime()
    print(f'{tm[3]:02}:{tm[4]:02}:{tm[5]:02}  - {tm}')
    set_time(tm[3],tm[4])
    time.sleep(60-tm[5])
```

It lacks some better calibration and even with fixed multimeter range to 5V it has decimal point on wrong position.

The solution is to add an charge pump circuit to step up the voltage to 24V max. and use multimeter set to fix range.
Creating a charge pump can be easy as to to use some diodes and caps. See 
https://www.youtube.com/watch?v=dMSOaJuzrdY
https://www.youtube.com/watch?v=ep3D_LC2UzU

for details.

With building it basics of programming and even electoronic circuits is learned.

