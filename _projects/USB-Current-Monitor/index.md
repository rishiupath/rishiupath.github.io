---
layout: post
title: USB Current Monitor
description: Designed and manufactured 4 USB cable monitoring boards that enables logging USB current and voltage using an external power analyzer. I created this for testing USB-C electronic devices during my time as a hardware engineer co-op at NETGEAR.
skills: 
  - Altium Designer
  - LPKF Protomat Circuit Board Plotter
  - Circuit Pro
  - Soldering (wires, SMD components)

main-image: /USB_Current_Monitor_realbot.jpg
---

## Design

{% include image-gallery.html images="USB_Current_Sensor_Schematic.png, USB_Current_Sensor_pcb.png, USB_Current_Sensor_3d_bot.png, USB_Current_Sensor_3d_top.png" height="400" %} 

The current sensing capability is based on the Kelvin (4-wire) resistance measurement method. This method separates the measurement path from the current-carrying path, reducing voltage loss in the sensing circuit. Additionaly a low-pass RC filter is used to filter out any high frequency noise coming from the power supply. Large copper pours were used to further minimize voltage loss in the current path.  

Mechanical stability was considered by adding holes to secure the cable to the board with zip ties. Additional pads were included to measure USB voltage and to easily probe the true value of the resistor.



## Manufacturing

{% include image-gallery.html images="" height="400" %} 

I independently manufactured the board using an LPKF Protomat circuit board plotter. The whole manufacturing process gave me a better understanding of how PCBs are made and how Gerber files translate to the physical board. I can definitely use this experience to inform decision decisions in future projects. Checkout a timelapse of the board being plotted!

To assemble the components, I used a soldering iron and heat gun...


## Resistor Calibration
{% include image-gallery.html images="USB_Current_Monitor_plot.png" height="400" %} 

A real world resistor does not have exactly its listed resistance value. In order to have more accurate test results, I measured the real value of the 10mOhm resistor by applying a ramped current using a power analyzer from 0-3A and plotting the measured voltage over this time. I then found the slope of the best fitted line to determine the resistance value. By measuring voltage and current over a range of different values noise, measurement error, and temperature effects are reduced.

## Final Product

{% include image-gallery.html images="USB_Current_Monitor_realclose.jpg, USB_Current_Monitor_realbot.jpg, USB_Current_Monitor_realtop.jpg" height="400" %} 

To make the board easy to use for the user, I made sure to add clear labels for wires and the resistance value. While also following standard and good practices such as different cable legnths between power and ground wirews to avoid shorts. The end-product is a big improvement over the previous testing board used, making future testing easier and more accurate. 
