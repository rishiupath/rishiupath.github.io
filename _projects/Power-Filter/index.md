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

- Designed passive RLC low-pass filter to suppress high-frequency noise from motors and switching regulators 
- Selected components rated for 12–24 V and 1–2 A operation
- Opted for a simple passive design with no active power required.
- Used thick copper traces and mechanical mounting holes for stability when mounted on rover

<ul style="color: #e0e0e0;">
  <li>Designed passive RLC low-pass filter to suppress high-frequency noise from motors and switching regulators</li>
  <li>Selected components rated for 12–24 V and 1–2 A operation</li>
  <li>Opted for a simple passive design with no active power required.</li>
  <li>Used thick copper traces and mechanical mounting holes for stability when mounted on rover</li>
</ul>

## Verification 

{% include image-gallery.html images="Spicefilter.png" height="400" %} 

-Performed AC small-signal analysis in LTspice to verify the filter cutoff frequency (~10 kHz) and adjust component values for optimal attenuation
-Ensured the filter attenuates high-frequency noise while maintaining stable DC voltage for downstream electronics


