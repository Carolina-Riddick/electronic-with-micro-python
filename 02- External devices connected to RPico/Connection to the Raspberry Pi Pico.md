## External devices connection to the Raspberry Pico.

- First I will set:

1.	Standard LEDs. Red oval Led
2.	Standard LEDs. Green oval Led
3.	Tactile Switches
4.	Buzzer module


## STEP 1 and 2 = For the first and second step we connect the ligths 

**MicroPython code:**

    from machine import Pin
    from utime import sleep

    tactile_switches = Pin(13, Pin.IN, Pin.PULL_DOWN)
    led_red = Pin(15, Pin.OUT)
    led_green = Pin(14, Pin.OUT)
    buzzer = Pin(11,Pin.OUT)

    while True:
        if tactile_switched.value() == 1:
            print("Tactile switched on!")
            led_red.value(1)
            led_green.value(1)
            buzzer.value(1)      
            sleep(1)
            led_red.toggle()
            led_green.toggle()
            buzzer.toggle()

![link](img/WhatsApp%20Image%202022-12-05%20at%2006.00.47.jpeg)

- Now with the light on !

![link](img/WhatsApp%20Image%202022-12-05%20at%2006.01.21.jpeg)

---
## STEP 3. We add the sensor - PIR HC-SR501: Infrared to my previous devices, so it would be:

  1. Standard LEDs. Led oval network
  2. Standard LEDs. Green oval Led
  3. Tactile Switches
  4. Buzzer module
  5. sensor - PIR HC-SR501:

--> The PIR sensor HC-SR501: It is an Infrared sensor that allows us to detect the presence or movement of an object and sends a certain signal to our microcontroller.

***External elements connected and the specifications***

1. Standard LEDs. Led oval network
   1. GP 15 with its corresponding resistor
   2. Male-Male Dupon cable to the negative power line and then to the corresponding GND (pin 23).
2. Standard LEDs. Green oval Led
   1. GP 14 with corresponding resistor
   2. Dupon male-male cable to the negative power line.

3. Tactile switche
   1. Pin 13 GPIO.
   2. Dupon Male-Male cable to positive power line.
   3. Pin 36 of Rpico 3V3 (OUT)

4.	Buzzer module
    1. VCC = Pin GP 11
    2. I/O = Pin 33 "ground" GND
    3. GND = Pin 38 "ground" GND

5. sensor - PIR HC-SR501: Infrared. 
We need 3 male - female Dupon cables 
   1. Female header on the GND pin of the sensor to the GND of the RPico on the breadboard Pin GND 3)
   2. Female header on the Data Output pin of the sensor to the GP 7 of the RPico.
   3. Female header on the sensor power supply pin to RPico 5 volt pin 40 (identified on the pinout as VBUS).


### *VBUS = Pin connected to the RPico's micro usb port and takes the 5 volt power line from the port before converting it to 3.3 volts which is the voltage to power the RP2040 from the RPico.*

<br>

*About the potentiometer...*

- We place the potentiometer "corresponding to time" of the sensor completely to the left. 
- Set the potentiometer "corresponding to distance" of the sensor all the way to the right.
- The jumper in the "one-shot" position.

**MicroPython code:**

    from machine import Pin
    from utime import sleep

    tactile_switches = Pin(13, Pin.IN, Pin.PULL_DOWN)
    led_red = Pin(15, Pin.OUT)
    led_green = Pin(14, Pin.OUT)
    buzzer = Pin(11,Pin.OUT)
    sensor_pir = Pin(7, Pin.IN, Pin.PULL_DOWN)

    led_red.value(0)
    buzzer.value(0)

    while True:
        if tactile_switches.value() == 1:
            print("Tactile switched on!")
            led_green.value(1)
            sleep(2)
        else:
            led_green.value(0)

        if sensor_pir.value() == 1:
            print("Detected movement")
            led_red.value(1)
            sleep(1)
            buzzer.off()
        else:
            buzzer.off()
            led_red.value(0)

![link](img/WhatsApp%20Image%202022-12-06%20at%2004.30.37%20(1).jpeg)


![link](img/WhatsApp%20Image%202022-12-06%20at%2004.30.37.jpeg)


***Video showing how it works***

[label](img/WhatsApp%20Video%202022-12-06%20at%2004.37.09.mp4)