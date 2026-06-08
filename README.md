# PSCAD PV Boost Converter Control Model

This repository contains a simple PSCAD model of a photovoltaic (PV) source connected to a DC-DC boost converter. The model allows the user to vary irradiance, temperature, and DC-link voltage reference. A PI-based voltage control loop generates the duty command used to produce the switching signal for the boost converter switch.

### Overview

The model demonstrates the basic operation of a PV-fed boost converter with closed-loop DC voltage control. The PV output depends on the selected irradiance and temperature conditions. The boost converter regulates the DC-side voltage by adjusting the switch duty ratio through a PI controller.

The main controllable inputs are:

- Solar irradiance
- PV cell/module temperature
- DC voltage reference, `Vdc_ref`

The main observed outputs are:

- PV voltage
- PV current
- DC-link voltage
- Boost converter duty ratio
- Switching signal
- Output voltage response

### System Description

The PV source is connected to a boost converter consisting of an inductor, power semiconductor switch, diode, output capacitor, and load/DC-link side. The boost converter increases the PV-side voltage to the required DC-link voltage level.

For an ideal boost converter, the approximate voltage conversion relationship is:

$$
V_{dc} = \frac{V_{pv}}{1-D}
$$

where:

- $V_{dc}$ is the output DC-link voltage
- $V_{pv}$ is the PV-side voltage
- $D$ is the duty ratio of the boost switch

As the duty ratio increases, the output voltage increases. The PI controller adjusts this duty ratio based on the DC-link voltage tracking error.

### Control Structure

The DC-link voltage is regulated using a PI controller. The voltage error is defined as:

$$
e_v = V_{dc,ref} - V_{dc}
$$

The PI controller generates the duty command:

$$
D = K_p e_v + K_i \int e_v dt
$$

The duty command is limited to a practical range before being compared with a carrier signal to generate the boost switch gating signal.

The control structure is:

```text
Vdc_ref
   |
   v
[Compare with measured Vdc]
   |
   v
Voltage error
   |
   v
PI Controller
   |
   v
Duty ratio command
   |
   v
PWM / switching signal generation
   |
   v
Boost converter switch
