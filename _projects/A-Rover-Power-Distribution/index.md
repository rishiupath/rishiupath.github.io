---
layout: post
title: Intelligent Rover Power Distribution System
description: Designed, assembled, and deployed a 3-board intelligent power distribution system for UBC Rover, regulating and distributing 24 V, 18 V, 12 V, and 5 V rails with high-current protection, current sensing, and STM32-based monitoring.
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

UBC Rover contains motors, computers, cameras, networking equipment, sensors, and other electronics that require several different supply voltages while drawing currents ranging from milliamps to tens of amps. I worked on the redesign of the rover's central power distribution system to safely distribute power from its LiPo battery while providing voltage regulation, circuit protection, current sensing, and electrical monitoring.

The system is split across **3 custom power distribution boards (PDBs)** supporting the rover's 24 V, 18 V, 12 V, and 5 V electrical systems.

I took the boards through the complete hardware development cycle: **schematic design, PCB layout, fabrication, component placement, reflow assembly, bring-up, testing, and final integration into the rover**. The completed system was deployed at competition in **August 2026**, where it successfully powered the rover throughout operation with no power-delivery issues to downstream devices.

Safety was a major consideration throughout the entire design process. The system was designed around the [CIRC Rover Safety Requirements](https://circ.cstag.ca/2026/rules/safety/), which informed decisions including fuse sizing, connector and conductor ratings, high-current PCB routing, thermal derating, circuit isolation, and documentation.

The main design goals were:

<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Safely distribute high-current battery power to independently protected output channels</li>
  <li>Generate regulated 18 V, 12 V, and 5 V rails from the rover's battery</li>
  <li>Ensure connectors, PCB traces, wiring, and protection devices are appropriately rated for each load</li>
  <li>Measure rail voltage and current for system monitoring and debugging</li>
  <li>Provide onboard microcontrollers for electrical monitoring and future telemetry integration</li>
  <li>Support a flexible set of rover loads while meeting CIRC electrical safety requirements</li>
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
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Power Conversion</th>
        <th style="text-align: left; padding: 8px; border-bottom: 1px solid #555;">Primary Loads</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding: 8px;">PDB1</td>
        <td style="padding: 8px;">24 V</td>
        <td style="padding: 8px;">Direct battery distribution</td>
        <td style="padding: 8px;">Drivetrain, arm, peripherals</td>
      </tr>
      <tr>
        <td style="padding: 8px;">PDB2</td>
        <td style="padding: 8px;">18 V / 12 V</td>
        <td style="padding: 8px;">LM5145 buck converters</td>
        <td style="padding: 8px;">Computers, cameras, motors, USB</td>
      </tr>
      <tr>
        <td style="padding: 8px;">PDB3</td>
        <td style="padding: 8px;">5 V</td>
        <td style="padding: 8px;">LM5145 buck converter</td>
        <td style="padding: 8px;">Raspberry Pis, networking, servos</td>
      </tr>
    </tbody>
  </table>
</div>

Each board contains an **STM32 microcontroller**, voltage measurement circuitry, current sensing, and independently fused output channels. The microcontrollers were included to provide local monitoring and support communication with the rover's onboard computing system.

<div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 16px;">
  <img src="/projects/POWER-DISTRIBUTION/PDB1_Block.png" style="width: 100%; height: auto;" />
  <img src="/projects/POWER-DISTRIBUTION/PDB2_Block.png" style="width: 100%; height: auto;" />
  <img src="/projects/POWER-DISTRIBUTION/PDB3_Block.png" style="width: 100%; height: auto;" />
</div>

---

## Safety-Driven Design

Safety was treated as a design constraint from the beginning rather than something checked after the boards were complete. I regularly referenced the CIRC electrical safety requirements while developing the system and incorporated them directly into component selection, power architecture, PCB layout, and protection circuitry.

CIRC requires each electrical circuit to have independent circuit protection and places limits on protection ratings based on both the expected load and the current-carrying capacity of the smallest conductor or connector in the circuit. This meant fuse selection had to be considered alongside connector ratings, wire gauge, PCB copper width, and downstream load requirements.

Thermal performance was also considered when evaluating high-current components. CIRC specifies that the rover may operate in 40 °C outdoor temperatures and that internal enclosure temperatures can be approximately 20 °C higher. Current sensors and other high-current components were therefore evaluated at elevated operating temperatures rather than relying only on room-temperature ratings.

Throughout the design process, I created detailed system-level block diagrams documenting the power source, protection devices, regulators, wiring, connectors, and downstream loads. Updated diagrams were frequently submitted to the **CIRC Safety Judges for review and feedback before proceeding with fabrication**, allowing safety concerns to be addressed while the design could still be modified.

This iterative safety review influenced decisions throughout all three boards and helped ensure that the final electrical architecture was both competition compliant and robust enough for field operation.

---

## PDB1 — 24 V High-Current Distribution

{% include image-gallery.html images="PDB1_Block.png" height="400" %}

PDB1 distributes the battery's approximately 24 V output directly to the rover's highest-power systems. Unlike PDB2 and PDB3, the primary rail does not require voltage conversion, allowing the board to focus on **high-current distribution, protection, sensing, and monitoring**.

The board is divided into sections for the rover's **robotic arm, drivetrain, peripherals, and spare outputs**. High-current load groups are monitored using upstream current sensors, while every individual downstream circuit remains independently fused.

### High-Current Design

PDB1 carries substantially more current than the other boards, requiring the entire power path to be considered as a system. Fuse ratings, connector ratings, wire gauge, current-sensor capacity, PCB copper width, and expected load current were evaluated together.

The robotic arm uses higher-current XT90 connections and 10 AWG wiring, while lower-current channels use XT30 connectors and 18 AWG wiring. High-current PCB traces were sized using IPC-2221 calculations and checked against the expected continuous load current.

Current monitoring is performed using **TMCS1123 Hall-effect current sensors**, selected for the higher currents present on the 24 V system. The sensors are rated for up to 70 A continuous current at a 60 °C ambient temperature, allowing their capability to be evaluated under the elevated temperatures expected inside the rover.

### Schematics

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
  <img src="/projects/POWER-DISTRIBUTION/PDB1_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/POWER-DISTRIBUTION/PDB1_Sensing.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB1_Layout.png" height="450" %}

### PCB 3D

{% include image-gallery.html images="PDB1_3D.png" height="400" %}

---

## PDB2 — 18 V & 12 V Power Conversion

{% include image-gallery.html images="PDB2_Block.png" height="400" %}

PDB2 generates and distributes the rover's **18 V and 12 V rails**. Because both rails are derived from the main battery, this board combines high-current power distribution with custom switching regulators.

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
  <img src="/projects/POWER-DISTRIBUTION/PDB2_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/POWER-DISTRIBUTION/PDB2_Sensing.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB2_Layout.png" height="450" %}

### PCB 3D

{% include image-gallery.html images="PDB2_3D.png" height="400" %}

---

## PDB3 — 5 V Power Conversion

{% include image-gallery.html images="PDB3_Block.png" height="400" %}

PDB3 provides the rover's regulated **5 V rail**, powering lower-voltage electronics including Raspberry Pis, networking hardware, servos, and other embedded systems.

Like PDB2, the board uses an **LM5145 synchronous buck converter** to efficiently step the rover's battery voltage down to the required rail voltage.

A **TMCS1108 Hall-effect current sensor** measures total 5 V rail consumption, while the rail is distributed across multiple independently fused output channels.

Although the rail voltage is lower, the design still needs to support several digital and electromechanical loads simultaneously while maintaining a stable 5 V supply during changing load conditions.

### Power Regulation

{% include image-gallery.html images="PDB3_Power.png" height="400" %}

The same safety philosophy used on the higher-voltage boards was applied to PDB3. Individual output fuses were selected around downstream load requirements, while connector, conductor, and PCB current capacities were checked to ensure the protection device remained the intentional weakest point in the circuit.

### Schematics

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
  <img src="/projects/POWER-DISTRIBUTION/PDB3_MCU.png" style="width: 100%; height: auto;" />
  <img src="/projects/POWER-DISTRIBUTION/PDB3_Sensing.png" style="width: 100%; height: auto;" />
</div>

### PCB Layout

{% include image-gallery.html images="PDB3_Layout.png" height="450" %}

### PCB 3D

{% include image-gallery.html images="PDB3_3D.png" height="400" %}

---

## Protection & Power Monitoring

A major goal of the redesign was to make the rover's power system both **safer and easier to debug**.

Every downstream circuit is protected by an onboard fuse selected based on its expected load as well as the current capacity of the associated connectors, wiring, and PCB traces. This ensures that excessive current is interrupted before exceeding the safe continuous rating of the power path.

The three boards also include circuitry for distributed electrical monitoring. Each contains an STM32 microcontroller with analog measurement circuitry for monitoring rail voltage and current.

The measurement system includes:

<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>TMCS1123 current sensors for the high-current 24 V system</li>
  <li>TMCS1108 current sensors for the 18 V, 12 V, and 5 V rails</li>
  <li>Resistor-divider networks for ADC voltage measurement</li>
  <li>STM32 ADCs for local voltage and current acquisition</li>
  <li>UART and CAN FD hardware interfaces for communication and future integration with the rover's onboard compute</li>
</ul>

The measurement hardware provides visibility into the electrical state of the rover during development and hardware bring-up, while the communication interfaces provide a path for future integration into the rover's central telemetry system.

{% include image-gallery.html images="PDB_Telemetry_Block.png" height="400" %}

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

## Fabrication, Assembly & Bring-Up

After completing schematic design and PCB layout, the three boards were fabricated and I **personally assembled the hardware**, including component placement and reflow soldering.

Bring-up was performed incrementally to reduce the risk of damaging the boards or downstream rover electronics. Power rails were validated before connecting loads, followed by testing of the STM32s, voltage-sensing circuitry, current sensors, and communication interfaces.

This process allowed hardware and firmware issues to be isolated before the boards were installed into the rover.

{% include image-gallery.html images="PDB_Assembly.png" height="400" %}

The completed boards were then integrated into the rover and tested with the actual downstream electrical systems they were designed to power.

---

## Design Challenges

### High-Current PCB Layout

The 24 V board distributes power to some of the rover's largest electrical loads, requiring significantly wider copper paths than a typical embedded PCB.

Instead of sizing PCB traces in isolation, I considered the complete current path: expected load current, fuse rating, sensor capacity, copper width, connector rating, wire gauge, and operating temperature. This was particularly important because the safety of the circuit is determined by its lowest-rated component.

### High-Current Switching Regulators

The 18 V, 12 V, and 5 V systems required efficient conversion from the rover battery while supporting high-current loads.

The LM5145 regulator stages required careful component selection and PCB placement to minimize high-frequency switching loops, manage thermal performance, maintain stable feedback, and prevent switching noise from interfering with the board's analog measurements.

### Measurement Across Multiple Voltage Rails

The boards monitor rails ranging from 5 V to the full battery voltage while the STM32 ADC operates from 3.3 V.

Each voltage-sensing circuit therefore required a different resistor-divider ratio to keep the ADC input within its safe range while maintaining useful measurement resolution. Current-sensor outputs also had to be converted in firmware using the sensitivity of each sensor.

### Safety Compliance

The power distribution system is one of the rover's most safety-critical electrical subsystems, so design decisions could not be based only on whether the circuit functioned electrically.

Fuse ratings, conductor capacity, connector capacity, thermal derating, insulation, and system-level fault behavior all had to be considered against the CIRC safety requirements.

Maintaining detailed block diagrams throughout the design also forced the complete power path to be reviewed at a system level. Sending these diagrams to CIRC Safety Judges for feedback before fabrication provided an additional review step before committing the designs to hardware.

### Hardware Bring-Up

Because the system directly interfaces with the rover battery and several expensive downstream devices, first power-on had to be approached carefully.

I brought up the hardware in stages, verifying the power-conversion circuitry and individual rails before connecting downstream loads. The STM32s, sensing circuitry, and communication interfaces were then tested independently before installing the boards into the rover.

### Competition Timeline

The primary requirement for competition was a safe and reliable power system. I prioritized completing, validating, and integrating the power-distribution hardware before expanding the firmware feature set.

This meant the rover entered competition with fully functional power distribution and protection, while CAN FD telemetry integration remained incomplete. Keeping the monitoring architecture separate from the core power path meant this unfinished feature did not affect the reliability of the rover's electrical system.

---

## Competition Deployment

The completed power distribution system was installed in the rover and used during our **August 2026 competition**.

All three custom PDBs operated successfully throughout the event, providing the required **24 V, 18 V, 12 V, and 5 V rails** to the rover's drivetrain, robotic arm, computers, networking equipment, cameras, sensors, and other electronics.

**No downstream devices experienced issues receiving power from the PDB system during competition.**

This competition deployment provided the final system-level validation of the boards beyond bench testing: hardware I designed, assembled, and brought up was used as the rover's central power-distribution system in the field.

The remaining development work is primarily firmware-focused, with the next step being completion of the **CAN FD interface and integration of voltage/current telemetry with the rover's onboard computing system**.
