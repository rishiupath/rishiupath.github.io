---
layout: post
title: Power Filter PCB
description: Designed low-pass filter for sensitive modules on rover to protect them from noise from the power distribution network. 
skills: 
  - Altium Designer
  - Filters
main-image: /3Dfilter.png
---

## Design Decisions 

{% include image-gallery.html images="Schemfilter.png, Layoutfilter.png" height="400" %} 

Component Selection: high power capabilities needed, componenets needed to handle 12-24V and 1-2A
- Simple solution, no active power for device needed
- large thick traces used
- mounting holes for mechanical stability and fixing

## Verification 

{% include image-gallery.html images="Spicefilter.png, Layoutfilter.png" height="400" %} 

Verified filter cutoff frequency on LTspice, and figured out ideal component values



