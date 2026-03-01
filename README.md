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
- ESP32 DevkitC v4 controller or another ESP controller with enough addressable pins, SPI and I2C bus
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

## Manual

MORE LATER!!

