# **Communication protocol (Inter-Integrated Circuit): I2C or "Bus I2C" or " standar I2C")**

## Display LCD 1602 with I2C module:

We are going to use a 1602 LDC display  which is based on the Hitachi HD44780 controller

- *Hitachi HD44780 Datasheet:* 
https://www.sparkfun.com/datasheets/LCD/HD44780.pdf


A recommended alternative to avoid the use of many cables is to use a module that allows access to the LCD via the I2C bus.

This module is built in and soldered to the board. The module used is the PCF8574.

This LCD-I2C module PCF8574 Datasheet (https://www.ti.com/lit/ds/symlink/pcf8574.pdf) can be connected to any LCD based on the Hitachi HD44780 Controller and reduces the number of cables required to two. It can be purchased together with the LCD as well as individually.

<br>
### The connection is simple. We power the module from our RPico by means of:
1.	GND 
2.	VBUS 
3.	SDA y SCL RPico Pin which the corresponding PCF8574 pins

<br>

---
# Discovering the I2C address of our LCD display

- I chose the following connections to connect the display 

1. I2C0 SDA (GPIO 1) 
2. I2C0 SCL (GRPIO 2 from RPico) 
3. Connect the corresponding SDA and SCL pins of display

**Introducing variables** 
- Object I2c corresponds  al display LCD

        I2c = (0, sda = sada, scl = scl ,freq = freq)

1. 0 = First argument and it corresponds to the number of I2C bus of the RPico
2. Sda = Second argument correspond to Data
3. Scl = Third argument correspond to time / clock
4. Freq = forth argument correspond to an integer number which sets the maximum speed for SCL


- Use of scan() method which scans all the I2C directions in the bus, specify when we create the object 
- Obtain the I2C direction in hexadecimal using:

        address = hex.(i2c.scan()[0])


- Then, we print the result:

        print(“The I2C address is: ” , address)


**MicroPython code:**

            from machine import Pin, I2C
            from utime import sleep

            scl = Pin(1)
            sda = Pin(0)
            freq = 400000

            i2c = I2C(0,sda=sda,scl=scl,freq=freq)
            address = hex(i2c.scan()[0])
            print("The I2C address is: " , address)

            Console >>>
            The I2C address is:  0x27
            >>> 
            This is the I2C address of the LCD display


![link](img/WhatsApp%20Image%202022-12-17%20at%2021.00.48.jpeg)

---

# Use of frameworks and "Hello World" in display
- Use of frameworks to control the LCD 1602 display with module I2C

- Framework took from : https://github.com/T-622/RPI-PICO-I2C-LCD


1. Download zip
2. Introduce the followings archives in RPico by using      MicroPython :
    1.	lcd_api.py
    2.	pico_i2c_lcd.py

If this was done correctly, we should find these two files in the bottom Raspberry Pi Pico tab.

This indicates that the files are inside the RPico's flash memory and now can be used.



- *In addition to the above code, should introduce the LCD object:*

        #I2C from the RPico.       
        i2c = I2C(0,sda = sda, scl = scl, freq = freq)

        #LCD object
        lcd = I2cLcd(i2c, I2C_ADDR, I2C_NUM_ROWS, I2C_NUM_COLS)

Arguments:

1. The first argument contains all the characteristics of the I2C bus described above. 
2. The second argument contains the address of the display I2C_ADDR = hex(i2c.scan()[0])
3.	The third argument is the number of rows I2C_NUM_ROWS = 2
4.	The fourth argument is the number of cold= I2C_NUM_COLS = 16


**MicroPython code**
            
            from lcd_api import LcdApi
            from pico_i2c_lcd import I2cLcd
            from machine import Pin, I2C
            from utime import sleep

            sda = Pin(0)
            scl = Pin(1)
            freq = 400000
                        
            # Define the i2C we will use from the RPico.       
            i2c = I2C(0,sda = sda, scl = scl, freq = freq)

            I2C_ADDR = 0x27
            I2C_NUM_ROWS = 2
            I2C_NUM_COLS = 16

            # LCD object 
            lcd = I2cLcd(i2c, I2C_ADDR, I2C_NUM_ROWS, I2C_NUM_COLS)

            while(True):
                lcd.clear()
                lcd.move_to(0,0)
                lcd.putstr("Hello World")
                sleep(5)


![link](img/WhatsApp%20Image%202022-12-17%20at%2021.56.22.jpeg)

---
# Now applying the potentiometer

from machine import Pin, ADC, I2C
from utime import sleep
from lcd_api import LcdApi
from pico_i2c_lcd import I2cLcd

potentiometer = ADC(26)
conversion_factor = 3.3 / (65535)

scl = Pin(1)
sda = Pin(0)
freq = 400000

i2c = I2C(0,sda=sda,scl=scl,freq=freq)

I2C_ADDR = 0x27
I2C_NUM_ROWS = 2
I2C_NUM_COLS = 16

lcd = I2cLcd(i2c,I2C_ADDR,I2C_NUM_ROWS,I2C_NUM_COLS)

while True:
    voltage = potentiometer.read_u16() * conversion_factor
    lcd.clear() 
    lcd.move_to(0,0)
    lcd.putstr("Voltage: ") 
    lcd.move_to(0,1) 
    lcd.putstr(str (voltage))
    sleep(10)


### ***Complete circuit:***
![link](img/WhatsApp%20Image%202022-12-17%20at%2022.19.53.jpeg)

---
### ***With 3.3 voltage:***

![link](img/WhatsApp%20Image%202022-12-17%20at%2022.18.23.jpeg)

---
### *** With 1.59 voltage:***

![link](img/WhatsApp%20Image%202022-12-17%20at%2022.17.40.jpeg)
