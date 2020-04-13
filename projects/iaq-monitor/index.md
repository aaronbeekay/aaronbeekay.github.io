---
layout: project
project-slug: iaq-monitor
title: I made an air quality sensor
---

# title 

Amidst the Summer of Funemployment in 2019, I put together an air quality sensor to try and quantify my concern about whether the warehouse ceiling was shedding concrete dust into my lungs.

The sensor measures the amount of airborne dust (particulate matter) and breaks its measurement into three buckets: particles smaller than 1um, particles smaller than 2.5um, and particles smaller than 10um. The last two are often referred to as [PM2.5 and PM10](https://www.epa.gov/pm-pollution/particulate-matter-pm-basics).)

(a little more about health effects here - why to care about PM2.5)

# Design
**this section needs edited**

![!Two CAD renders of the sensor next to a picture of the finished device](/assets/images/iaq_with_cad.jpg)

When I designed the housing, I imagined it being able to hang from a cord or sit on somebody's desk as a personal monitor. The idea was that there would be a second power port at the bottom of the device, so you could plug it in when it was sitting on your desk without having the cable sprout out of the top of the thing. 

I ended up only making one prototype of this design (instead of the 3 I originally thought I'd make), so I didn't really test desk mode extensively. This housing doesn't have the bottom power port. 

I think I've been affected by the [plethora](https://www.apple.com/homepod/) [of](https://store.google.com/product/google_home) [fabric-clad](https://www.amazon.com/dp/B0794LMHLY) [cylindrical](https://www.amazon.com/dp/B07FZ8S74R) [tech](https://www.jbl.com/bluetooth-speakers/JBL+Flip+4.html) [products](https://www.amazon.com/Anker-Soundcore-Bluetooth-Waterproof-technology/dp/B07PPNY861/) lately, but I like the way the housing came out. The oval holes come out fine on my 3D printer without support material (which would have to be poked/pulled out of every hole by hand). 

In practice, I like the "indicator ring" much better than a single status LED. Right now, sitting at my desk about 25 feet from the sensor without my glasses on, the indicator light on the USB charger in the adjacent outlet is too blurred to see, but I can still clearly read the ring on the sensor as a green light. The sensor hangs a couple feet away from the surface of the column, so you can see it from most angles.

## Construction
This thing is all hacked together out of perfboard. I printed a carrier for the electronics that fits inside the housing:

![Exploded view](/assets/images/iaq_exploded.png)

Putting this together didn't require too many connections, so I assembled the perfboard bits ad-hoc on the bench with an eye towards compactness. When the electronics were working, I just measured what I'd made and made a carrier with mounting holes in the right places.

### Airflow
The sensor has a tiny little internal fan that draws air in, and it exhausts it out of the four circular ports on the same side.

![Cutaway view of sensor with airflow indicated](/assets/images/iaq_airflow_cutaway.png)

I added this little airwall in the carrier to try and keep air from recirculating, but I have no idea if it's effective at all.

### Indicator ring
<video autoplay loop src="/assets/images/iaq_v1_animated.mp4"></video>

The indicator ring is another printed part with six RGB LEDs mounted inside. I'm pleased with how the lens came out. It's got a concave internal face, to try and get the light to fade slightly at the top and the bottom of the ring, but I don't really think you can tell in person.

![Section view of LED indicator ring](/assets/images/iaq_lens_section.png)

I was excited to play with [OSH Park's flexible PCB fabrication service](https://docs.oshpark.com/services/flex/), so the idea was to print a single [flex PCB](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fyic-assm.com%2Fwp-content%2Fuploads%2F2018%2F06%2F04.jpg&f=1&nofb=1) that had all six LEDs on it, wrapped around a mounting surface in the bottom, and terminated in a plug that could connect directly to the main board. I never got around to doing this, although I still am excited about it.

(pic of led assembly jig?)

For the prototype here, I just hacked together six discrete RGB LED boards with hookup wire, and it works OK. I printed an assembly jig for the LEDs that holds them at the same spacing as the wire distance between the LEDs when they're installed in the sensor. This was a nice idea, but I still put them together pretty sloppy.

### Sensor
![A single PMS5003 sensor sitting on a perfect white background for no clear reason](/assets/images/iaq_pms5003.jpg)

I used a PMS5003 sensor as the heart of this thing - it is cheap ([$15 shipped on AliExpress](https://www.aliexpress.com/item/32618735056.html)), has good community support (there are tutorials on SparkFun and Adafruit, and open-source libraries for reading its output), and reasonably compact. It looks a lot like a clone of Honeywell's [HPM series PM sensors](https://sensing.honeywell.com/honeywell-sensing-particulate-hpm-series-sell-sheet-007608).

![Schematic diagram of sensor internals](/assets/images/iaq_operation_diagram_hpm.jpg)

<small><em>This diagram lifted from the Honeywell HPM datasheet without permission.</em></small>

The PMS500x sensors are laser-diffraction sensors: they shine a laser through the outside air that's been sucked into the sample chamber by the built-in fan. Particles get hit by the laser and scatter the beam, and larger particles scatter the beam differently from smaller particles, so a sensor on the other side of the sample chamber can measure the scattered laser light and estimate the quantity and size distribution of the particles in the air.

These particular sensors are certainly not calibrated or anything, and I don't know a good way to calibrate them or obtain an accurate reference sample with the equipment I have here. When I do get more of these online, I plan to do a consistency test by leaving several sensors running side-by-side and comparing their measurements - but that test only measures the relative accuracy (from sensor to sensor), not the absolute accuracy (the "true" number of particles in the air). Good enough for the time being.

### Microcontroller
This is the first or second project I built on the [Particle Photon](https://docs.particle.io/photon/) and Particle's [Device Cloud](https://www.particle.io/device-cloud/). I was very into the idea of a platform that takes care of network connectivity, device flashing/updates, and cloud messaging in an integrated way. Particle's stuff is fun to use, but since this project I've discovered [ESPHome](https://esphome.io), and it's been life-changing. 

This project works fine with the Particle software - but the firmware still hangs up occasionally when it loses wireless connectivity, requiring a power cycle to fix. I haven't dug into this too much, but I expect it's some dumb bug of my own making. 

The easy fun way to program the Particle devices is to use their in-browser IDE to write "sketches", Arduino-style, with a main loop function. They provide you a "Device OS" API to manage system functions like wireless link and power states. This OS provides some RTOS-like task functionality, where you can define independent tasks that run periodically, and the OS handles multitasking and executing the right task at the right time. Great - but when I was building my Particle projects, I was underwhelmed by the maturity of this multitasking functionality. Obviously I've overlooked something writing the firmare for these things.

# Usage
I hung the sensor from a column in the center of the warehouse in *(date)*. It has been online since then with minimal downtime.

The sensor transmits its measurements every so often over [MQTT](http://mqtt.org/faq). My home automation server, running [Home Assistant](https://www.home-assistant.io), has a sensor configured to represent the physical sensor, and the HomeAssistant sensor's values are set by the MQTT messages.

There is no offline-sensor detection built in here, which is something I'd rather have - I occasionally find that the sensor has been offline for half a day without my noticing. The measurement data is lost in this case.

# Results
{% include plots/pm25_before_after_filter.html %}

- form factor
  - usage: hanging or tabletop
  - stationary in house
- construction
- sensor/MCU
  - sensor: PMS5003
  - MCU: Particle Photon
    - but I don't like it
- LED indicator
  - gif of it spinning
  
# Usage
- placement in house
  - house layout features: ceiling-mounted shop heater, overhead door, no interior walls
- 

# Results
- Typical day
  - Raw values
  - Histogram/distribution
- Month or so
- Before and after air filter installation
