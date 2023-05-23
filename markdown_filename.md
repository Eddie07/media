					**SMART-CLOCK v1**

**Kernel device manual**

**Contents:**

1.  **Device installation.**

    1.  **Hardware connection to Raspberry 40 pin extension.**

    2.  **Enable SPI, I2C and DTBO in Raspberry config.**

    3.  **Disable time synchronization on Raspberry.**

    4.  **Module installation.**

    5.  **Removing the module from memory**

2.  **User manual**

    1.  **Main functions**

**2.1.1. Time**

**2.1.2. Timer**

**2.1.3. Alarm**

**2.1.4. Temperature and Pressure**

**2.1.5. Pedometer**

**2.1.6. Game**

**2.1.7. Options**

**2.1.8. Making of screenshots**

**2.1.9. Display BMP files**

**2.1.10. Notifications bar**

**1.Device installation**

1.  **Hardware connection to Raspberry 40 pin extension:**


<a href="url"><img src="https://github.com/Eddie07/media/blob/main/blob/display_connection.png?raw=true" align="left" height="48" width="48" ></a>
__Connect TFT Module (SPI):__

VDD3.3-\> 3,3v (pin 1)

CS -\> SPI0_CE0 (pin 24)

SCL -\> SPI0_SCLK (pin 23)

SDA -\> SPI0_MOSI (pin 19)

RS -\> GPIO_23 (pin 16)

RST -\> GPIO_24 (pin 18)

GND -\> GROUND (pin 25)

![](media/image2.png){width="3.725in" height="2.9231463254593177in"}2)
Connect bmp280 sensor (I2C):

VIN -\> 3,3v (pin 1)

GND -\> GROUND (pin 25)

SCL -\> SCL1 i2c (pin 5)

SDA -\> SDA1 i2c (pin 3)

![](media/image3.png){width="4.044834864391951in"
height="2.9305555555555554in"}3) Connect mpu6050 sensor (I2C):

VCC-\> 3,3v (pin 1)

GND -\> GROUND (pin 25)

SCL -\> SCL1 i2c (pin 5)

SDA -\> SDA1 i2c (pin 3)

AD0 -\> 3,3v (pin 1)

4\) rtc3231 (w/o eeprom) (I2C):

![](media/image4.png){width="4.00625in" height="3.3194444444444446in"}+
-\> 3,3v (pin 1)

\- -\> GROUND (pin 25)

C -\> SCL1 i2c (pin 5)

D -\> SDA1 i2c (pin 3)

5\) Connect hardware button (2 pin):

Pin1 -\> 3,3v (pin 1)

![](media/image5.png){width="3.9106463254593176in"
height="2.8333333333333335in"}Pin2 -\> GPIO_17 (pin 11)

2.  **Enable SPI, I2C and DTBO in Raspberry config:**

you can enable SPI, I2C from the **/boot/config.txt** file. Open a
terminal window and enter:

sudo nano /boot/config.txt

Look for a line that reads **#dtparam=spi=on** and remove
the **\#** symbol. Do the same for **#dtparam=i2c=on** and

**dtparam=i2c_vc=on**. Now add a line with
**dtoverlay=smart_clock.dtbo** so dtbo overlay will be connected at boot
time

Reboot your Pi.

3.  **Disable time synchronization on Raspberry.**

By default, Raspberry is using time-synchronization service while
connected to network.

We need to disable it, because time we use from rtc clock.

[sudo systemctl stop systemd-timesyncd\
sudo systemctl disable systemd-timesyncd]{.mark}

4.  **Module installation**

Building the Module as device-tree overlay:

1)  make \[KDIR=\[Path to your linux kernel\]\]

2)  The module is compiled to **smart_clock.ko**

3)  The device tree overlay will be compiled to **smart_clock.dtbo**

4)  Copy **smart_clock.dtbo** to boot/overlays directory to the device

Inserting into your kernel:

Command:

[sudo insmod smart_clock.ko]{.mark}

Check kernel log if device is properly booted:

[smart_clock: smart_clock: Major = 238 Minor = 0]{.mark}

[smart_clock: gpio_button: found gpio #17 value in DT]{.mark}

[smart_clock: st7735fb: init]{.mark}

[smart_clock: st7735fb: probe started]{.mark}

[st7735fb spi0.0: SPI ok]{.mark}

[smart_clock: st7735fb: Total vm initialized 40960]{.mark}

[smart_clock: bmp280: probing]{.mark}

[smart_clock: bmp280: probed]{.mark}

[smart_clock: ds3231: probing]{.mark}

[smart_clock: ds3231: probed]{.mark}

[smart_clock: ds3231: set time of the day from RTC result 0]{.mark}

[smart_clock: mpu6050: probing]{.mark}

[smart_clock: mpu6050: probed]{.mark}

*if some of devices will fail to start, you will get the message with
hardware name: probe failed. Check hardware connection or config.txt*

*If "set time of the day from RTC" result is other than 0 than your rtc
is not activated or battery is too low, once you setup clock or alarm
this will be fixed automatically.*

1.  **Remove the module from memory**

Command:

[sudo rmmod smart_clock.ko]{.mark}

module will be removed when the last timer is stopped. Check kernel log
if device is properly removed:

[smart_clock: controls unloaded]{.mark}

[smart_clock: sensors unloaded]{.mark}

[smart_clock: gpio_button: device exit]{.mark}

[smart_clock: st7735fb: exiting]{.mark}

[smart_clock: st7735fb: unloaded]{.mark}

[smart_clock: module unloaded]{.mark}

2.  **User manual**

    1.  **Main functions:**

**Smart clock is designed to work as simple smart-watch with
entertainment part. Functions:**

-Time

-Alarm

-Timer

-Temperature and Pressure

-Pedometer (Steps meter)

-Game

-Making of screenshot (BMP format) (2.1.7)

-Display BMP files (2.1.7)

**Customization is also possible through options:**

-24h or AM/PM

-Temperature view in C/F

-Enable or disable the Alarm

**Functions are controlled by one button:**

-click (Short press)

-middle-long (Press and hold about 1 sec)

-long click (Press and hold longer than 1 sec)

**2.1.1. Time**

![](media/image6.tiff){width="2.611111111111111in"
height="2.088888888888889in"}![](media/image7.tiff){width="2.5972222222222223in"
height="2.077777777777778in"}

Hold the button longer than 1 sec to enter first view mode.

Current time will be loaded from "rtc"

Clock view can be customized by showing time in format 24h or AM/PM

In **Options** view.

[Adjusting the time:]{.underline}

Use middle-long click to enter adjustments mode.

Adjustment's part will be blinking. Press short the button to adjust.

To switch the adjustments to the next part hold button long click

1^st^ adjustments will be minutes, 2^nd^ -- hours, 3^rd^ day of the
month, 4^th^ the month, 5^th^ year (year is capped to 2050)

*Once you finish the adjustments -- clock value will be automatically
stored in rtc.*

Use long click to switch mode to next mode: **Timer**

**2.1.2. Timer**

![](media/image8.tiff){width="2.7118055555555554in"
height="2.1694444444444443in"}

Timer can be **started and stopped** by short pressing of the button.

**To reset** the timer -stop timer and use middle-long click.

Timer can be **run in background** while view is switched. Use
long-press while timer is running to switch the view while timer is
running in background. The notification of running timer will be shown
in notification bar.

Switch to the next view **Alarm** press long press button.

**2.1.3. Alarm**

![](media/image9.tiff){width="2.8055555555555554in"
height="2.2444444444444445in"}

The alarm can be turned on and off in **Options** view.

When Alarm is triggered the message "ALARM !!!" will appear blinking on
screen, to exit- short press the button.

[Adjusting the alarm:]{.underline}

Use middle-long click to enter adjustments mode.

Adjustment's part will be blinking. Press short the button to adjust.

To switch the adjustments to the next part hold button long click

1^st^ adjustments will be minutes, 2^nd^ -- hours

*Once you finish the adjustments -- alarm values will be automatically
stored in rtc.*

![](media/image10.tiff){width="2.6006944444444446in"
height="2.0805555555555557in"}![](media/image11.tiff){width="2.6006944444444446in"
height="2.0805555555555557in"}**2.1.4. Temperature and air pressure.**

Temperature can be shown in C or F. Check **Options** for the option.

Switch the next view **Pedometer** press long-press button.

**2.1.5. Pedometer.**

![](media/image12.tiff){width="2.6006944444444446in"
height="2.0805555555555557in"}

Pedometer or Step meter shows steps (or vertical shakes) from the device
start.

To reset the pedometer short press the button.

Switch the next view **Game** press long-press button.

**2.1.6. Game**

![](media/image13.tiff){width="2.5868055555555554in"
height="2.0694444444444446in"}

Besides of pedometer the mpu6050 is also used for the control of the
game "SNAKE".

Tilt the device left, right, up, down to control the snake and eat the
fruit.

Once snake eats herself or touches the white merges -- the game will be
over.

To Restart game short press the button.

Switch the next view **Options** press long-press button.

**2.1.7. Options**

![](media/image14.tiff){width="2.517361111111111in"
height="2.013888888888889in"}

[Adjusting the options:]{.underline}

Use middle-long click to enter adjustments mode.

Adjustment's part will be blinking. Press short the button to adjust.

To switch the adjustments to the next part hold button long click

*Note: Once you finish the adjustments -- alarm values will be
automatically stored in rtc.*

**2.1.8. Making of screenshot (BMP format)**

Use terminal in your Raspberry to type the command:

[cat dev/smart-clock]{.mark}

to get file stream of bmp image of your current screen.

Example:

[cat dev/smart-clock \> screen.bmp]{.mark}

will create new file screen.bmp with the screenshot from your device.

**2.1.9. Display BMP files.**

Use terminal in your Raspberry to print to device the command:

[cat screen.bmp \> dev/smart-clock]{.mark}

The file will be displayed on screen.

*Note: Only files with resolution less than 160x128 are supported.
Otherwise, you will receive the error message.*

**2.1.9. Notifications bar.**

When alarm is enabled in options -- the icon "Alarm" will be shown is
notification bar on screen.

When timer is running in background -- the icon "Clock" will be blinking
in notification bar on screen.
