---
layout: post
title: Rover Motor Emergency Remote Stop
description: Designed and manufactured 4 USB cable monitoring boards that enable logging USB current and voltage using an external power analyzer. Created for testing USB-C electronic devices during my time as a hardware engineering co-op at NETGEAR.
skills: 
  - Altium Designer
  - LPKF Protomat Circuit Board Plotter
  - Circuit Pro
  - Soldering (wires, SMD components)

main-image: /USB_Current_Monitor_realbot.jpg
---

## Design

{% include image-gallery.html images="DRS_ALL.png" height="500" %} 

The board includes three user-accessible terminals. The main terminal is for USB current measurement, which uses the Kelvin (4-wire) resistance measurement method. This separates the measurement path from the current-carrying path, reducing voltage loss in the sensing circuit. A low-pass RC filter is also included to reduce high-frequency noise from the power supply.  

The other two terminals are used for measuring USB voltage and for passing current through the resistor to determine its true resistance value. 

{% include image-gallery.html images="USB_Current_Sensor_pcb.png" height="500" %} 

Large copper pours were used to minimize voltage loss, while the surrounding board area acts as a continuous ground plane. 

{% include image-gallery.html images="USB_Current_Sensor_3d_bot.png, USB_Current_Sensor_3d_top.png" height="400" %} 

Mechanical stability was improved by adding holes to secure the cable to the board with zip ties. Additional pads were included to measure USB voltage and to easily probe the true value of the resistor.

## Manufacturing

I independently manufactured the board using an LPKF Protomat circuit board plotter. This process gave me a strong understanding of PCB fabrication and how Gerber files translate into a physical board. I can apply this knowledge to make better design decisions in future projects. 

For assembly, I used a soldering iron and heat gun to place capacitors, resistors, and through-hole wire connections.

Check out a timelapse of the board being plotted:

<iframe src="https://drive.google.com/file/d/1PQRJObD1RQtk5_p99KKtV14KKnC7CuuY/preview" width="640" height="480"></iframe>

## Resistor Calibration

{% include image-gallery.html images="USB_Current_Monitor_plot.png" height="400" %} 

Real-world resistors do not exactly match their nominal values. To improve measurement accuracy, I characterized the 10 mΩ resistor by applying a ramped current (0–3 A) using a power analyzer and measuring the resulting voltage.  

The resistance was determined from the slope of the best-fit line of voltage versus current. Using multiple data points reduces the impact of noise, measurement error, and temperature effects.

## Final Product

{% include image-gallery.html images="USB_Current_Monitor_realclose.jpg, USB_Current_Monitor_realbot.jpg, USB_Current_Monitor_realtop.jpg" height="400" %} 

To improve usability, I added clear labels for all connections and the resistance value. I also followed best practices, such as using different wire lengths for power and ground to reduce the risk of accidental shorts.  

The board provides accurate and reliable current measurements and is used to test USB-C charging systems across a wide range of devices. It is a significant improvement over the previous testing setup, making future validation easier and more consistent.
