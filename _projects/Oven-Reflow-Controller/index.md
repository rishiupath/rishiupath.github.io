---
layout: post
title: Oven Reflow Controller
description: Designed an oven reflow controller for SMD soldering using the N7E003 micrcontroller system and A51 assembly language. The project uses a thermocouple for temperature measurement, LCD and push buttons for user input, and plots the live temperature strip chart using python. This was a team project consisting of 6 team members. 
skills: 
  - Assembly programming
  - Python
  - Microcontrollers
  - Finite state machines
  - Sensors

main-image: /Reflowboard.jpg
---

## Design Features 
<ul style="color: #e0e0e0;">
  <li>Implemented PWM control to regulate oven heating</li>
  <li>Used ADC-based push buttons for setting time and temperature</li>
  <li>Designed a finite state machine (FSM) to follow the reflow profile accurately</li>
  <li>Added a safety feature to shut off the oven if temperature exceeds safe limits</li>
</ul>

## Reflow Profile 

{% include image-gallery.html images="Reflowprofile.png" height="400" %} 

The different states of the reflow process are shown here. PREHEAT, SOAK, RAMP TO PEAK, REFLOW, COOL.

## Results

{% include image-gallery.html images="Reflowefm8.jpg" height="400" %} 

This board's SMD components were soldered using the reflow controller!

## Project Showcase
Checkout the project's video and report. 

{% include youtube-video.html id="5nsnfEk_kys&t=231s" autoplay = "false" width= "900px" %}  

<a href="https://drive.google.com/file/d/1Wx4ZJ4D98804qQDofn3fygVI0V8VEnXP/view?usp=sharing" target="_blank" class="button">View Report</a>

