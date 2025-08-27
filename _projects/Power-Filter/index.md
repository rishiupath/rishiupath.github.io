---
layout: post
title: Power Filter PCB
description: Designed a low-pass RLC filter PCB to protect sensitive rover modules from noise in the power distribution network.
skills: 
  - Altium Designer
  - Filters
  - Circuit simulation
main-image: /3Dfilter.png
---

## Design Decisions 

{% include image-gallery.html images="Schemfilter.png, Layoutfilter.png" height="400" %} 

<ul style="color: #e0e0e0;">
  <li>Designed passive RLC low-pass filter to suppress high-frequency noise from motors and switching regulators</li>
  <li>Selected components rated for 12–24 V and 1–2 A operation</li>
  <li>Opted for a simple passive design with no active power required.</li>
  <li>Used thick copper traces and mechanical mounting holes for stability when mounted on rover</li>
</ul>

## Verification 

{% include image-gallery.html images="Spicefilter.png" height="400" %} 

<ul style="color: #e0e0e0;">
  <li>Performed AC small-signal analysis in LTspice to verify the filter cutoff frequency and adjust component values for optimal attenuation</li>
  <li>Ensured the filter attenuates high-frequency noise while maintaining stable DC voltage for downstream electronics</li>
</ul>
