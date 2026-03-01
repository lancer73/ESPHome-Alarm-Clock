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
| Substitution | Purpose |
|:-------------|:--------|
| chip_orient and reverse_enable | this allows you to rotate the display if you mount it upside down, or just want to put the alarm clock in an upside down position (attach it to a shelf) |
| hide_button_sensor | change this to false so you can read out the voltage with each button. With these values change the display_min/max (button with the square), onoff_min/max (button with the circle), up_min/max and down_min/max values. Take at least 0.1V of margin above and below the detected value. Make sure button voltage settings do not overlap. |
| baseline | this is the baseline setting for the CCS811 sensor. When first powering up make sure there is an empty string here. Put the alarm clock in a well ventilated position for a few hours and then look at the controller logs to find the baseline of the sensor and copy that value in. |
| outdoor_temp_sensor | Put the entity_id of an outdoor temperature sensor here. The outdoor temperature will be show when the alarm is snoozed. This will help to pick the right outfir for today.|
| weekdays | You can change weekdays to your local language. This sunstitution is just the first 2 letters of all days in the week, starting at Sunday. |

*Do nor forget to set all the passwords and encryption keys in the configuration. Search for text between [] for everything you need to replace.*
| <figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/upsidedown.jpg" width="400px"></figure> | <figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/set_alarm.jpg" width="400px"></figure> |
|-----------|-----------|

## Manual
### Manual operation
#### The buttons
| Symbol | Name | Purpose |
|:-------|:-----|:--------|
| Square | Display | With the display button you can cycle through the pages. Pressing it once will lead to the environmental sensors, pressing it twice will lead to the alarm for the next day, pressing it again for the day after etc. Pressing it 8 times will lead back to the time display. The display will revert back to time automatically  (30s timeout by default). If the display is off, this button will turn on the display first |
| Circle | On/Off | This button either toggles the display on or off when showing time or environment parameters or enable/disable the alarm time shown in the display. By double pressing it (slowly) you can set an alarm time once, meaning the alarm will be disabled for the next week automatically. The red led will show if an alarm is enabled and the amber led will show if it is enabled once. If you have no leds, the status will be shown using the right topmost pixel of the display (red) and the pixel below that (amber).
| Up | Up | When the time or environment parameters are shown, the up button will be forwarded to Home Assistant to control room lights or whatever you choose. When showing an alarm time, it will increase the alarm time by 5 minutes. If you keep the button pressed the time will be counting up in steps of 5 minutes.
| Down | Down | Same effect as the up button, but decreasing in time |

#### The display
When you first power up the alarm clock it will show its IP address so you can find the web interface. After the timeout period it will show the time. If an alarm is set within 24 hours the red led or right topmost pixel will light up.

It is possible to turn on the blue led or the right bottemmost pixel from Home Assistant to indicate there is a message. What the message is, is up to you. Could be a signal to put the trash out today, that your favourite GP driver has won in the night race or anything else. I use it to indicate if the alarm clock of a family member has been set.

The amber led will light up of the TVOC reading is above the warning threshold. If you have no leds, it will be the most right 3rd pixel from above.

If the display shows an alarm time, the amber led will indicate if an alarm has been enabled once.

#### When the alarm goes off
When the alarm goes off, the display will invert. Either Home Assistant will arrange the audible alarm e.g. by turning on a remotely controlled speaker, or you can use the built in buzzer. If the connection to Home Assistant has been severed the buzzer will sound anyway. You can use any button to snooze the alarm. The alarm will turn off by itself after 90 minutes by default.

### Operation from Home Assistant or Web
Home Assistant and the web interface expose many more configurations and parameters. 
#### Alarms
There is an alarm time for every day of the week. The time is valid for that day only. There are two alarm switches for every day. The first will make the alarm go off on that day, the second, when enabled, will disable the alarm for that that after it has sounded. If you turn the once switch on, the alarm switch will be enables as well. If you switch off the alarm switch the once switch will be turned off as well.

There are shortcut buttons to turn off all alarms at once (starting a holiday) or enabling all weekday alarms (at the end of a holody).

#### Display on/off
It is possible to switch the display on and off using the physical buttons. This behavior can be tuned further. The thinking behind this is that some people are distubed hy the alarm display at night, either from the brightness, or because the can see the time. If you can't sleep, seeing the time doesn't necessarity help.

You can set a time at which the display should be turned off and a time when it should be turned on again. This is combined with the "Automatic display on/off" switch, which must be enabled for the set times to have any effect. If the display is off and the time is in the "off" window, the display and in/off buttons will turn on the display and automatically turn it off again after the timeout period.

The following configurations will also influence the display state:
| Setting | Effect |
|:--------|:-------|
| Display off after alarm | Keeps the display on for a number of minutes after the alarm has been snoozed. (Only valid in the "off window") The display will turned off after set minutes to not disturb other occupants in the bed |
| Display on before alarm | Turns the display on set minutes before the alarm sounds. Useful if you typically wake up before the alarm time. If the display is still of you can close your eyes again, if it is on it might be best to disable the alarm and get up |

#### Other sensors and settings
| <figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/light_set_alarm.jpg" width="400px"></figure> | <figure><img src="https://github.com/lancer73/ESPHome-Alarm-Clock/blob/60f0b53d30204f8d0f9722055e9881dc94b70db0/images/light_temp.jpg" width="400px"></figure> |
|----------|-----------|
  
| Sensor | Meaning |
|:-------|:--------|
| Illuminance | Illuminance measured by the sensor. May be used for automatic control of lights for instance. |
| Left button | Binary sensor indicating the state of the down button. This way the press of a button on the alarm clock can turn on the lights. |
| Right button | Binary sensor indicating the state of the up button |
| After last alarm | Number of minutes after the last alarm went off. For automations that need to be kicked off after the alarm time, such as pre-heating the car |
| Alarm countdown | Number of minutes before the alarm. For automations that need to be kicked off before the alarm, such as turning on th ecoffee maker or setting the heating in the bathroom |

| Switch | Meaning |
|:-------|:--------|
| Alarm | Will turn on when the alarm goes off. To be used in automation to turn on remote speakers or lights when the alarm goes off. Can also be switched on from home assistant. E.g. to synchronise alarms |
| Alert now | Will invert the diplay and sound a siren using the piezo buzzer. For connecting the alarm clock to a fire alarm, or just to get a teenager out of bed. Alarm can be turned off using a button on the clock. |
| Message | Will turn on/off the message led as a remined for something, like putting the trash cans out or something else. |
| Buzzer with alarm | When the switch is off, the buzzer will only sound if the connection with Home Assistant has been interrupted. Turn this off if you have an automation to pick up the state of the alarm switch. |
| Up/Down toggles display | Turn this on if you don't want any automations connected to the up/down buttons. If this switch is on up/down toggles the display on/off when the display shows normal time. |

| Numbers | Meaning |
|:--------|:--------|
| TVOC warning | Warning level when the TVOC warning led must be turned on. |
| Alarm timeout in minutes | After this time has expired the alarm will be turned off automatically. |
| Display timeout | Number of seconds after the last keypress after which the clock must return to normal time, or turn off again in the "off" window. |
| Maximum brightness at | Number of lux at which the display and leds have maximum brightness |
| Minimum brightness at| Number of lux at which the display has minimum brightness |






