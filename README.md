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
<figure>
  <img src="https://github.com/user-attachments/assets/9150e299-e841-41b1-9b82-796ef6b81ac7">
  <figcaption><i>Fully built alarm clock with the Spleen font (not the custom built font)</i></figcaption>
</figure>

## Parts list
With these parts you can build the "light" version.
- Transparent black filament for 3D print of front
- Colored filament for housing
- Piece of sheet plastic for illumination sensor window
- 4x pushbutton
- 5x 2k2 resistor
- KY-12 Piezo buzzer
- BH1750 Illumination sensor
- MAX7219 4x8x8 matrix display
- ESP32 DevkitC v4 controller or another ESP controller with enough addressable pins, SPI and I2C bus
- A piece of electrical tape to cover the led on the controller board (unless you want a "shine in the dark" alarmclock.

For the full version you need these parts as well:
- CCS811 Gas sensor (TVOC/eCO2 monitoring)
- BMP180 Temperature sensor (for more accurate TVOC metering)
- 3x led in various colors
- 3x resistor in 220 to 600 ohm range (depending on max wanted brightness)



## 3D print

## Assembly

## Manual

MORE LATER!!

