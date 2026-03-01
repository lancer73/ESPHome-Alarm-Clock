# ESPHome-Alarm-Clock
Alarm clock built in ESPhome with the following features:
- MAX7219 matrix display
- User configurable display behavior for light level and on/off
- One alarm per weekday
- Sensors to let Home Assistant handle the "waking up"
- Automatic fallback to piezo buzzer
- Sensors for automations before and after the alarm time
- Alarm can be triggered from HA for normal and emergency situations
- Switchable led from HA to convey a message

There are two different versions of the alarm clock. A full version with additional leds and environmental sensors and a light version. 3D print designs are included for both. The normal version has buttons at the top and sensors in the back, the light version has buttons on the front and a ligh sensor on the top.
| <figure><img src="https://github.com/user-attachments/assets/9150e299-e841-41b1-9b82-796ef6b81ac7" align="left" width="400px"></figure> |<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/light.jpg" align="right" width="400px"></figure> |
|-----------------|-----------------|

## Parts list
<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/buttons.jpg" align="right" width="250px"></figure>
<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/sheet_plastic.jpg" align="right" width="250px"></figure>

With these parts you can build the "light" version.
- Transparent black filament for 3D print of front
- Colored filament for housing
- Piece of sheet plastic for illumination sensor window (see picture)
- 4x pushbutton (see picture)
- 5x 2k2 resistor
- KY-12 Piezo buzzer
- BH1750 Illumination sensor
- MAX7219 4x8x8 matrix display
- ESP32 DevkitC v4 controller or another ESP controller with enough addressable pins, SPI and I2C bus and an analog-digital converter.
- A piece of electrical tape to cover the led on the controller board (unless you want a "shine in the dark" alarmclock.
- USB cable with USB-A and USB-micro connector
- USB charger

For the full version you need these parts as well:
- CCS811 Gas sensor (TVOC/eCO2 monitoring)
- BMP180 Temperature sensor (for more accurate TVOC metering)
- 3x led in various colors
- 3x resistor in 220 to 600 ohm range (depending on max wanted brightness)

## 3D print
The print files are included in the repository. The normal version comes in three parts, the ligh version in two. I was able to print all parts without supports on a Creality K1C printer. The normal version in the picture was printer with supports, hence the lines below two of the buttons. Print the bodies standing on their backsides.

The other parts can be printed in any color or material. Be aware that if your print in a light color, the led of the controller will shine through. Do not forget to tape off the led.

All parts contain recesses to house the various parts, except for the controller itself. I leave the controller "free floating" in the housing to not push to much stress on the cabling. 

## Assembly
<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/Alarm%20Clock_schema.png"></figure>

The picture above shows the wiring of the alarm clock. Most of it is straightforward. All sensors use the same I2C bus. The led connections are straightforward as well, Use a resistor in the 220 to 600 ohm range, depending on the maximum brightness that is needed from the led. The leds are controlled via the ledc component of ESPhome. The illumination sensor will be used to set the brightness of the leds dynamically.

The buttons are connected in parallel. The resistance will determined by the button that has been pressed. This is measured by an ADC pin of the controller. With no buttons pressed the resistance is indefinite leading to a detected voltage of about 0V. Pressing the top button leads to a total resistant of 4k4 ohms, measured in between the resistors leading to about 1.6V. The lowest button has all 5 resistors in serial connection. The resistor on the red plus line is there to limit the voltage on the ADC pin.

### Pins used
| Pin | Purpose |
|:-----|:---------|
| 3v3 | Buttons, BH1750, CCS811, BMP180 |
| 5v  | MAX7219 display, KY-12 buzzer |
| GND | Everything leads to GND... |
| GPIO5 | CS connection of MAX7219 |
| GPIO18 | SPI CLK pin of MAX7219 |
| GPIO21 | SDA pin of I2C bus leading to BH1750, CCS811, BMP180|
| GPIO22 | SCL pin of I2C bus leading to BH1750, CCS811, BMP180|
| GPIO23 | SPI MOSI pin for MAX7219 |
| GPIO25 | Red led |
| GPIO26 | Amber led |
| GPIO27 | Blue led |
| GPIO32 | Piezo buzzed, controlled through ledc component to enable tunes & music |
| GPIO33 | Buttons |

### Putting it together
Personnally I like to do everything with Dupont connectors as most builds get taken apart eventually and the parts re-used. When the setup has been tested, the parts can be glued into place. Remeber to put the buttons in first before you solder them as they need to in through a hole. I used hot glue to put everything in place. 

The light alarm clock body has a large hole in the back allowing the micro USB connector to pass (the normal version has a slot). After the cable is in, put the cover in place and hot-glue it in.

<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/inside_light_display.jpg" align="left" width="400px"></figure>
The picture on the left shows the MAX7219 display glued in place in the front panel (light version). Make sure the panel fits snugly and that is is completely flat. You will see in the front when it is not. The yellow heat shrink tubing covers the resistors. (this is actually the third one I built. Got tired of crimping on Dupont connectors).

6 dots of hot glue keep everything in place.
<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/inside_light.jpg" align="right" width="350px">
Another picture showing the inside of the light version. BH1750 and KY12 are glued in place as is the inset in the connector entry hole in the back. Cut a piece of sheet plastic to size and put it in the recess first, before putting in the BH1750. I used some super glue to gluw the window into place first.

The front display will slide into the housing. It is a tight fit, but fit it does. No glue needed to secure it. The same goes for the back panel of the normal version.

With everything in place you can create the config in ESPHome. Copy the contents of alarm_clock.yaml and adapt where needed. 

Most relevant configurations:
- chip_orient and reverse_enable, this allows you to rotate the display if you mount it upside down, or just want to put the alarm clock in an upside down position (attach it to a shelf)
- hide_button_sensor, change this to true so you can read out the voltage with each button. With these values change the display_min/max (button with the square), onoff_min/max (button with the circle), up_min/max and down_min/max values. Take at least 0.1V of margin above and below the detected value. Make sure button voltage settings do not overlap.
- baseline, this is the baseline setting for the CCS811 sensor. When first powering up make sure there is an empty string here. Put the alarm clock in a well ventilated position for a few hours and then look at the controller logs to find the baseline of the sensor and copy that value in.
- outdoor_temp_sensor Put the entity_id of an outdoor temperature sensor here. The outdoor temperature will be show when the alarm is snoozed. This will help to pick the right outfir for today.
- You can change weekdays to your local language. This sunstitution is just the first 2 letters of all days in the week, starting at Sunday.
<figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/upsidedown.jpg" width="400px">







## Manual

MORE LATER!!

