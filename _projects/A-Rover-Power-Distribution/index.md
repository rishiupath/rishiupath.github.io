---
layout: post
title: Rover Power Distribution System
description: Designed, assembled, and deployed a 3-board intelligent power distribution system for UBC Rover, regulating and distributing 24V, 18V, 12V, and 5V rails with high-current protection, current sensing, and STM32-based monitoring.
skills:
- Altium Designer
- Power Electronics
- High-Current PCB Design
- Buck Converters
- STM32
- ADC, UART, CAN FD
- PCB Assembly and Reflow
main-image: /PDB_ALL.png
---

## Purpose

The rover contains motors, computers, cameras, networking equipment, sensors, and other electronics that require several different supply voltages while drawing currents ranging from milliamps to tens of amps. I worked on the redesign of the rover's central power distribution system to safely distribute power from its **7S3P LiPo battery** while providing voltage regulation, circuit protection, current sensing, and system monitoring. The design uses **custom DC-DC buck converters** to generate the rover's regulated 18 V, 12 V, and 5 V rails, **Hall-effect current sensors** for isolated current measurement, and onboard **STM32 microcontrollers** to sample voltage and current through their ADCs and support **UART** and **CAN FD** communication.


The system is split across **3 custom power distribution boards (PDBs)** supporting the rover's 24V, 18V, 12V, and 5V electrical systems.

I took the boards through the complete hardware development cycle: **schematic design, PCB layout, fabrication, component placement, reflow assembly, bring-up, testing, and final integration into the rover**. The completed system was deployed at competition in **August 2026**, where it successfully powered the rover throughout operation with zero faults.

Safety was a major consideration throughout the entire design process. The system was designed around the [CIRC Rover Safety Requirements](https://circ.cstag.ca/2026/rules/safety/), which informed decisions including fuse sizing, connector and conductor ratings, high-current PCB routing, thermal derating, circuit isolation, and documentation.

The main design goals were:

<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Safely distribute high-current battery power to independently protected output channels</li>
  <li>Generate regulated 18V, 12V, and 5V rails from the rover's battery</li>
  <li>Ensure connectors, PCB traces, wiring, and protection devices are appropriately rated for each load</li>
  <li>Measure rail voltage and current for system monitoring and debugging</li>
  <li>Meet all CIRC electrical safety requirements</li>
  <li>Create a modular system, capable of handling current and future needs</li>
  <li>Produce hardware robust enough for field operation and competition use</li>
</ul>

---

## System Architecture

{% include image-gallery.html images="PDB_Power_Architecture.png" height="450" %}

The rover's electrical system begins with the LiPo battery and battery management system, followed by the emergency-stop system and central power distribution boards. The PDBs then regulate and distribute power to all downstream electronics.

Rather than placing every voltage rail on a single large board, the system is divided into three boards based on voltage and load requirements:

<div style="overflow-x: auto; margin-bottom: 16px;">
  <table style="width: 100%; border-collapse: collapse; color: #e0e0e0;">
    <thead>
      <tr>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Board</th>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Output Rails</th>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Current Rating</th>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Power Conversion</th>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Primary Loads</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding: 8px;">PDB1</td>
        <td style="padding: 8px;">24 V</td>
        <td style="padding: 8px;">100 A</td>
        <td style="padding: 8px;">Direct battery distribution</td>
        <td style="padding: 8px;">Drivetrain, arm, peripherals</td>
      </tr>
      <tr>
        <td style="padding: 8px;">PDB2</td>
        <td style="padding: 8px;">18 V / 12 V</td>
        <td style="padding: 8px;">20 A per rail</td>
        <td style="padding: 8px;">LM5145 buck converters</td>
        <td style="padding: 8px;">Computers, cameras, motors, USB</td>
      </tr>
      <tr>
        <td style="padding: 8px;">PDB3</td>
        <td style="padding: 8px;">5 V</td>
        <td style="padding: 8px;">20 A</td>
        <td style="padding: 8px;">LM5145 buck converter</td>
        <td style="padding: 8px;">Raspberry Pis, networking, servos</td>
      </tr>
    </tbody>
  </table>
</div>

Each board contains Hall effect current sensors and an STM32 microcontroller to provide voltage and current monitoring and communication with the rover's onboard computing system. 

---

## Safety-Driven Design
I regularly referenced the CIRC electrical safety requirements while developing the system and incorporated them directly into component selection, power architecture, PCB layout, and protection circuitry.

CIRC requires each electrical circuit to have independent circuit protection and places limits on protection ratings based on both the expected load and the current-carrying capacity of the smallest conductor or connector in the circuit. This meant fuse selection had to be considered alongside connector ratings, wire gauge, PCB copper width, and downstream load requirements.

Thermal performance was also considered when evaluating high-current components. CIRC specifies that the rover may operate in 40 °C outdoor temperatures and that internal enclosure temperatures can be approximately 20 °C higher. Current sensors and other high-current components were therefore evaluated at elevated operating temperatures rather than relying only on room-temperature ratings.

Throughout the design process, I created detailed system-level block diagrams documenting the power source, protection devices, regulators, wiring, connectors, and downstream loads. Updated diagrams were frequently submitted to the **CIRC Safety Judges for review and feedback before proceeding with fabrication**, allowing safety concerns to be addressed while the design could still be modified.

This iterative safety review influenced decisions throughout all three boards and helped ensure that the final electrical architecture was both competition compliant and robust enough for field operation. 

---

## PDB1 — 24 V High-Current Distribution

{% include image-gallery.html images="PDB1_Block.png" height="400" %}

PDB1 distributes the battery's nominal 24 V output directly to the rover's highest-power systems. Unlike PDB2 and PDB3, the primary rail does not require voltage conversion, allowing the board to support the much higher current demands of the drivetrain and robotic arm. The board was designed to handle up to 100 A of continuous load, requiring careful consideration of the entire high-current path.

Key high-current design considerations included:

<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;"> <li><strong>Heavy copper stackup:</strong> The 4-layer PCB uses the manufacturer's highest available copper weights, with 2 oz copper on the external layers and 1 oz copper on the internal layers.</li> <li><strong>Low-impedance return paths:</strong> Both internal layers are continuous ground planes, providing short return paths throughout the board.</li> <li><strong>Large polygon pours:</strong> High-current nets use large copper pours rather than conventional traces to maximize available conductor width.</li> <li><strong>Parallel top and bottom copper:</strong> High-current pours are duplicated on the top and bottom layers and connected with dense via stitching, allowing both copper layers to share the load current.</li> <li><strong>IPC-2221 current sizing:</strong> High-current copper paths were sized using IPC-2221 calculations based on expected current and allowable temperature rise.</li> <li><strong>Connector selection:</strong> XT90, XT60, and XT30 connectors were selected based on the expected current of each downstream circuit rather than using a single connector type throughout the board.</li> <li><strong>Wire gauge selection:</strong> Higher-current connections use appropriately sized wiring, including 10 AWG for the robotic arm, while lower-current loads use smaller-gauge wiring matched to their expected current.</li> <li><strong>Independent circuit protection:</strong> Every downstream output is individually fused based on its load, connector, wiring, and PCB current capacity.</li> </ul>

The board is divided into sections for the rover's robotic arm, drivetrain, peripherals, and spare outputs. High-current load groups are monitored using upstream current sensors, while every individual downstream circuit remains independently fused.

Current monitoring is performed using TMCS1123 Hall-effect current sensors, selected for the higher currents present on the 24 V system. The sensors are rated for up to 70 A of continuous current at a 60 °C ambient temperature, allowing the sensing architecture to be evaluated against the elevated temperatures expected inside the rover.

### Schematics

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
  <img src="/projects/A-Rover-Power-Distribution/PDB1_Power.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB1_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB1_Connectors.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB1_Layout.png, PDB1_Layout_back.png" height="450" %}

<p style="color: #b0b0b0; font-size: 14px; margin-top: 6px;">
  <strong>4-Layer Stackup:</strong> Top — 2 oz Signal/Power, Inner 1 — 1 oz GND, Inner 2 — 1 oz GND, Bottom — 2 oz Signal/Power
</p>


### PCB 3D

{% include image-gallery.html images="PDB1_3D.png, PDB1_3D_back.png" height="400" %}

---

## PDB2 — 18 V & 12 V Power Conversion

{% include image-gallery.html images="PDB2_Block.png" height="400" %}

PDB2 generates and distributes the rover's **18 V and 12 V rails**.

Two **LM5145 synchronous buck converter** stages generate the 18 V and 12 V supplies. Each regulator is designed for up to **20 A**, allowing the board to support the rover's computing and peripheral systems.

The 18 V rail supplies systems including the rover's Intel NUC and NVIDIA Jetson, while the 12 V rail supplies cameras, fans, USB hardware, and motor drivers.

Each rail has its own **TMCS1108 Hall-effect current sensor**, allowing total current consumption to be measured independently. Each downstream circuit is then individually fused based on its expected load and the current-carrying capacity of the associated wiring and connectors.

### Power Regulation

{% include image-gallery.html images="PDB2_Power.png" height="400" %}

Designing the regulators involved selecting the switching frequency, inductors, MOSFETs, feedback network, compensation components, and input/output capacitance while accounting for efficiency, thermal performance, and transient response.

PCB layout was particularly important around the switching regulators. High-current switching loops were kept compact, while sensitive feedback and measurement circuitry was routed away from noisy switching nodes.

Protection remained part of the regulator design as well. The regulator output, downstream connectors, wiring, and individual fuses were considered together to prevent any section of the power path from being exposed to a current above its safe continuous rating.

### Schematics

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
  <img src="/projects/A-Rover-Power-Distribution/PDB2_Power.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB2_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB2_Connectors.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB2_Layout.png, PDB2_Layout_back.png" height="450" %}

<p style="color: #b0b0b0; font-size: 14px; margin-top: 6px;">
  <strong>4-Layer Stackup:</strong> Top — 1 oz Signal/Power, Inner 1 — 0.5 oz GND, Inner 2 — 0.5 oz GND, Bottom — 1 oz Signal/Power
</p>

### PCB 3D

{% include image-gallery.html images="PDB2_3D.png, PDB2_3D_back.png" height="400" %}

---

## PDB3 — 5 V Power Conversion

{% include image-gallery.html images="PDB3_Block.png" height="400" %}

PDB3 provides the rover's regulated **5 V rail**, powering lower-voltage electronics including Raspberry Pis, networking hardware, servos, and other embedded systems.

Like PDB2, the board uses an **LM5145 synchronous buck converter** to efficiently step the rover's battery voltage down to the required rail voltage.

A **TMCS1108 Hall-effect current sensor** measures total 5 V rail consumption, while the rail is distributed across multiple independently fused output channels.

### Schematics

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
  <img src="/projects/A-Rover-Power-Distribution/PDB3_Power.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB3_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/A-Rover-Power-Distribution/PDB3_Connectors.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB3_Layout.png, PDB3_Layout_back.png" height="450" %}

### PCB 3D

{% include image-gallery.html images="PDB3_3D.png, PDB3_3D_back.png" height="400" %}

---

## Fabrication, Assembly & Bring-Up

After completing schematic design and PCB layout, the three boards were fabricated and I **personally assembled the hardware**, including component placement and reflow soldering.

{% include image-gallery.html images="PDB_Assembly.png" height="400" %}

Bring-up was performed incrementally to reduce the risk of damaging the boards or downstream rover electronics. Power rails were validated before connecting loads, followed by testing of the STM32s, voltage-sensing circuitry, current sensors, and communication interfaces.

This process allowed hardware and firmware issues to be isolated before the boards were installed into the rover.

{% include image-gallery.html images="PDB_Complete.png" height="400" %}

The completed boards were then integrated into the rover and tested with the actual downstream electrical systems they were designed to power.

{% include image-gallery.html images="PDB_Integrate.png" height="400" %}
---

## Embedded Firmware

I developed STM32 firmware to bring up and validate the boards' monitoring and communication hardware.

Raw ADC measurements are converted into rail voltage and current values using the known voltage-divider ratios and current-sensor sensitivities. I also developed and tested UART communication between boards and performed standalone CAN communication testing during development.

Firmware development was performed incrementally so that each hardware subsystem could be validated independently before system integration.

<ol style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Validate STM32 programming and board-level power</li>
  <li>Verify ADC voltage measurement</li>
  <li>Verify current-sensor ADC measurement</li>
  <li>Establish UART communication between boards</li>
  <li>Bring up and test CAN communication hardware</li>
  <li>Integrate CAN FD telemetry with the rover's onboard computing system</li>
</ol>

The first five stages were used during board development and bring-up. Due to the competition timeline, the final **CAN FD integration with the rover's onboard compute was not completed before the August 2026 competition**, so the PDB telemetry system was not used during competition.

Importantly, the communication system is supplementary to the boards' primary power-distribution function. The regulators, protection circuitry, and power outputs operate independently of CAN FD, allowing the completed hardware to safely power the rover even without telemetry integration.

---

## Competition Deployment

The completed power distribution system was installed in the rover and used during our **August 2026 competition**.

All three custom PDBs operated successfully throughout the event, providing the required **24 V, 18 V, 12 V, and 5 V rails** to the rover's drivetrain, robotic arm, computers, networking equipment, cameras, sensors, and other electronics.

**No downstream devices experienced issues receiving power from the PDB system during competition.**

The remaining development work is primarily firmware-focused, with the next step being completion of the **CAN FD interface and integration of voltage/current telemetry with the rover's onboard computing system**.
