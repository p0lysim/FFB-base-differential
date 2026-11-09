<h1 align="center">FFB Base Differential</h1>

<p align="center">
  <b>An open-source, 3D-printed force feedback joystick base built around a bevel-gear differential.</b><br>
  Two BLDC motors · ODrive v3.6 controller · AS5048A absolute encoders · printed in PLA
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-work%20in%20progress-orange" alt="Status: work in progress">
  <img src="https://img.shields.io/badge/hardware-CC%20BY--NC--SA%204.0-lightgrey" alt="Hardware license: CC BY-NC-SA 4.0">
  <img src="https://img.shields.io/badge/code-MIT-blue" alt="Code license: MIT">
  <img src="https://img.shields.io/badge/controller-ODrive%20v3.6%20%2F%20MKS%20XDrive-0a7bbb" alt="Controller: ODrive v3.6 / MKS XDrive">
  <img src="https://img.shields.io/badge/3D%20printed-PLA-2ea44f" alt="3D printed in PLA">
</p>

<p align="center">
  <img src="docs/images/demo.gif" width="340" alt="DIY force feedback joystick base moving the stick on its own with two BLDC motors">
  &nbsp;
  <img src="docs/images/render-hero.jpg" width="340" alt="3D render of the 3D-printed differential force feedback joystick base">
</p>

<p align="center">
  <a href="https://makerworld.com/fr/models/2220275-ffb-base-differential"><img src="https://img.shields.io/badge/MakerWorld-Download-00AE42?style=for-the-badge" alt="Download on MakerWorld"></a>
  <a href="https://www.printables.com/model/1378606-ffb-base-differential"><img src="https://img.shields.io/badge/Printables-Download-FA6831?style=for-the-badge" alt="Download on Printables"></a>
  <a href="https://www.thingiverse.com/thing:7112820"><img src="https://img.shields.io/badge/Thingiverse-Download-248BFB?style=for-the-badge" alt="Download on Thingiverse"></a>
</p>

<p align="center">
  <a href="#-bill-of-materials">Bill of materials</a> ·
  <a href="#-assembly-guide">Assembly guide</a> ·
  <a href="hardware/stl">STL files on GitHub</a>
</p>

---

## Table of contents

- [Highlights](#-highlights)
- [How it works](#%EF%B8%8F-how-it-works)
- [Specifications](#-specifications)
- [Bill of materials](#-bill-of-materials)
- [Assembly guide](#-assembly-guide)
- [Electronics and ODrive setup](#-electronics-and-odrive-setup)
- [License](#-license)

## ✨ Highlights

- **Differential drive.** Both motors stay fixed in the base, and **both motors work on both axes**. Each axis can use the torque of two motors, and the moving parts stay light.
- **Printed gears.** Spiral bevel gears (20T / 60T) and a 2:1 gear train (15T → 15T → 30T), all printed in PLA.
- **Absolute position sensing.** One 14-bit AS5048A magnetic encoder on each motor, read over SPI by the controller.
- **Off-the-shelf electronics.** 2 × 5065 140 KV BLDC motors and one dual-axis MKS XDrive (an ODrive v3.6-compatible board).
- **Standard bearings.** 12 × 6704 and 2 × 6805, plus M3 screws. No machined parts.
- **Remixable.** The full CAD assembly (STEP) is included.

## ⚙️ How it works

<p align="center">
  <img src="docs/images/how-it-works.gif" height="360" alt="Animation of the bevel gear differential: pitch moves both gear trains the same way, roll moves them in opposite directions">
  &nbsp;
  <img src="docs/images/real-mechanism.gif" height="360" alt="Close-up of the printed differential: crown gear, side gears and BLDC motor turning while the stick moves">
</p>

Each motor drives one **side bevel gear** through a 15T pinion, a 15T idler and a 30T ring (orange
train on one side, blue train on the other). Both side gears mesh with the **60T crown gear** that
carries the grip:

- **Pitch:** when both side gears turn **the same way**, the whole differential carrier tilts around the side-panel axis.
- **Roll:** when they turn **in opposite directions**, the crown gear spins and tilts the grip holder sideways.

| | Pitch (carrier axis) | Roll (crown axis) |
|---|---|---|
| Motor turns per stick turn | 2 | 6 |
| Stick angle from motor angles | (θ₁ + θ₂) / 4 | (θ₁ − θ₂) / 12 |
| Stick torque from motor torques | 2 · (τ₁ + τ₂) | 6 · (τ₁ − τ₂) |

The maths, sign conventions and the torque mixing needed for force feedback are in
[docs/how-it-works.md](docs/how-it-works.md).

## 📐 Specifications

| | |
|---|---|
| Axes | 2 (pitch + roll), bevel-gear differential |
| Motors | 2 × BLDC 5065 140 KV |
| Controller | MKS XDrive (ODrive v3.6 compatible) |
| Sensors | 2 × AS5048A, 14-bit, SPI |
| Reduction | 2:1 pitch · 6:1 roll |
| Peak stick torque (theoretical, 8 A) | ≈ 1.9 N·m pitch · ≈ 5.7 N·m roll |
| Size | 113 × 136 mm footprint, 247 mm tall |

## 🧾 Bill of materials

### Printed parts (14 files, 25 pieces)

The quantity is in each file name (`_x2` means print it twice). The same files are on
[MakerWorld](https://makerworld.com/fr/models/2220275-ffb-base-differential),
[Printables](https://www.printables.com/model/1378606-ffb-base-differential) and
[Thingiverse](https://www.thingiverse.com/thing:7112820).

| | File | Qty | Role |
|:-:|---|:-:|---|
| <img src="docs/images/parts/Side_Panel.jpg" width="80"> | [`Side_Panel_x2.stl`](hardware/stl/Side_Panel_x2.stl) | 2 | Case side. Holds 4 × 6704 bearings (carrier axis + idler) |
| <img src="docs/images/parts/Bottom_Panel.jpg" width="80"> | [`Bottom_Panel_x1.stl`](hardware/stl/Bottom_Panel_x1.stl) | 1 | Case body (bottom, front and back) |
| <img src="docs/images/parts/Motor_Support.jpg" width="80"> | [`Motor_Support_x2.stl`](hardware/stl/Motor_Support_x2.stl) | 2 | Motor mount with a pocket for the AS5048A board |
| <img src="docs/images/parts/Motor_Gear.jpg" width="80"> | [`Motor_Gear_x2.stl`](hardware/stl/Motor_Gear_x2.stl) | 2 | 15T pinion on the motor shaft |
| <img src="docs/images/parts/Transmission_Gear.jpg" width="80"> | [`Transmission_Gear_x2.stl`](hardware/stl/Transmission_Gear_x2.stl) | 2 | 15T idler gear |
| <img src="docs/images/parts/Small_Gear.jpg" width="80"> | [`Small_Gear_x2.stl`](hardware/stl/Small_Gear_x2.stl) | 2 | Side gear: 30T ring + 20T spiral bevel |
| <img src="docs/images/parts/Big_Gear.jpg" width="80"> | [`Big_Gear_x1.stl`](hardware/stl/Big_Gear_x1.stl) | 1 | 60T spiral bevel crown gear (roll output) |
| <img src="docs/images/parts/Gear_Hub.jpg" width="80"> | [`Gear_Hub_x1.stl`](hardware/stl/Gear_Hub_x1.stl) | 1 | Differential carrier (pitch). Holds 2 × 6805 |
| <img src="docs/images/parts/Hub_Axis.jpg" width="80"> | [`Hub_Axis_x2.stl`](hardware/stl/Hub_Axis_x2.stl) | 2 | Carrier shafts (pitch axis) |
| <img src="docs/images/parts/Hub_Spacer.jpg" width="80"> | [`Hub_Spacer_x2.stl`](hardware/stl/Hub_Spacer_x2.stl) | 2 | 1 mm spacer between a side gear and a side panel |
| <img src="docs/images/parts/Axis_Cap.jpg" width="80"> | [`Axis_Cap_x4.stl`](hardware/stl/Axis_Cap_x4.stl) | 4 | Outer bearing caps on the side panels |
| <img src="docs/images/parts/Grip_Holder_P2.jpg" width="80"> | [`Grip_Holder_P2_x1.stl`](hardware/stl/Grip_Holder_P2_x1.stl) | 1 | U-shaped yoke attached to the crown gear |
| <img src="docs/images/parts/Grip_Holder_P1.jpg" width="80"> | [`Grip_Holder_P1_x1.stl`](hardware/stl/Grip_Holder_P1_x1.stl) | 1 | Threaded grip adapter |
| <img src="docs/images/parts/AS5048A_Cap.jpg" width="80"> | [`AS5048A_Cap_x2.stl`](hardware/stl/AS5048A_Cap_x2.stl) | 2 | Encoder board cover |

### Mechanical hardware

| Item | Qty | Where | Link |
|---|:-:|---|---|
| Ball bearing **6704** (20 × 27 × 4 mm) | 12 | 4 per side panel, 2 per side gear | [Amazon](https://amzn.to/46OaAmP) |
| Ball bearing **6805** (25 × 37 × 7 mm) | 2 | Gear hub (roll axis) | [Amazon](https://amzn.to/3H3fCBD) |
| M3 self-tapping screws (for plastic), assorted lengths | 1 kit | Case, caps, yoke | [Amazon](https://amzn.to/45HISak) |
| PLA filament | ≈ 1 kg | All printed parts | [Amazon](https://amzn.to/4loAYaD) |

### Electronics

| Item | Qty | Notes | Link |
|---|:-:|---|---|
| BLDC motor **5065 140 KV** | 2 | Outrunner, 8 mm shaft | [Amazon](https://amzn.to/3Hg6zxa) |
| **MKS XDrive** (ODrive v3.6 compatible) | 1 | Dual-axis BLDC controller | [Amazon](https://amzn.to/456QqmO) |
| **AS5048A** magnetic encoder board (SPI) | 2 | Usually sold with a diametrically magnetized magnet | |
| Diametrically magnetized magnet (6 × 2.5 mm) | 2 | Glued on the rear end of each motor shaft | |
| Brake resistor (2 Ω) | 1 | Absorbs the energy when you push the stick | |
| DC power supply | 1 | Matching your board's voltage rating | |
| Grip | 1 | Any grip that fits the threaded adapter | |

<sub>Some links above are affiliate links. They cost you nothing and help support the project.</sub>

## 🔧 Assembly guide

<p align="center">
  <img src="docs/images/exploded-view.jpg" width="720" alt="Exploded view of the differential force feedback joystick base">
</p>

In the pictures below, **orange** marks the printed parts added at that step. Bearings are shown in steel.

### Step 1: Side panels

<img src="docs/images/assembly/step-1-side-panels.jpg" width="560" alt="Step 1: pressing a 6704 bearing into each side of both bores of a side panel">

- Upper bore (carrier axis): press one **6704** bearing from the outside and one from the inside.
- Lower bore (idler gear): same, one **6704** from each side.
- Repeat with the second side panel. That uses 8 bearings in total.

### Step 2: Side gears

<img src="docs/images/assembly/step-2-side-gears.jpg" width="560" alt="Step 2: one 6704 bearing on each side of a side gear">

- Press one **6704** bearing into each face of a `Small_Gear` (2 per gear).
- Repeat with the second side gear. That uses 4 bearings in total.

### Step 3: Differential core

<img src="docs/images/assembly/step-3-differential-core.jpg" width="560" alt="Step 3: gear hub with two 6805 bearings, the 60 tooth crown gear and the grip yoke">

- Press the **2 × 6805** bearings into the `Gear_Hub`.
- Slide the shaft of the `Big_Gear` (60T crown) into one bearing.
- Fit the `Grip_Holder_P2` yoke: one end goes into the other bearing, the other end is screwed to the back of the crown gear.
- Screw the `Grip_Holder_P1` adapter on top of the yoke.

### Step 4: Carrier shafts and side gears

<img src="docs/images/assembly/step-4-hub-axes.jpg" width="560" alt="Step 4: hub axes pushed into the gear hub, side gears and spacers slid on">

- Push the two `Hub_Axis` shafts into the sides of the `Gear_Hub`.
- Slide a side gear on each shaft, with the bevel teeth facing the crown gear, then a `Hub_Spacer`.

### Step 5: Motor block

<img src="docs/images/assembly/step-5-motor-block.jpg" width="560" alt="Step 5: two 5065 BLDC motors between the motor supports, motor gears and encoder covers">

- The two motors sit side by side, head to tail. Screw each motor to the `Motor_Support` on its output side.
- Press a `Motor_Gear` onto each motor shaft, outside the support.
- Glue a diametric magnet on the **rear** end of each motor shaft.
- Place each AS5048A board in the pocket of the opposite support, facing the magnet, and close it with an `AS5048A_Cap`.

### Step 6: Case body

<img src="docs/images/assembly/step-6-case-body.jpg" width="560" alt="Step 6: motor block lowered into the bottom panel">

- Lower the motor block into the `Bottom_Panel` and screw the two motor supports to it.
- Route the motor phase wires and the encoder cables to the outside before closing.

### Step 7: Close the case

<img src="docs/images/assembly/step-7-close-case.jpg" width="560" alt="Step 7: differential core lowered in, idler gears, side panels and axis caps">

- Lower the differential core into the case.
- Put each `Transmission_Gear` (idler) in place so that it meshes with both the motor gear and the side gear ring.
- Close the case with the two side panels: the carrier shafts and the idler hubs go into the side-panel bearings.
- Fit the four `Axis_Cap`s from the outside and secure each one with 3 screws. Then screw the side panels to the case body.
- Check that the stick moves freely on both axes before you power anything.

### Step 8: Wiring, configuration and grip

- Wire the motors, encoders, brake resistor and power supply (see the next section).
- Configure and calibrate the ODrive.
- Screw your grip onto the adapter.

<p align="center">
  <img src="docs/images/photos/real-base-closeup.jpg" width="250" alt="Printed differential FFB base, close-up of the bevel gears">
  <img src="docs/images/photos/real-with-grip.jpg" width="250" alt="Differential FFB joystick base with a grip mounted">
  <img src="docs/images/photos/real-grip-top.jpg" width="250" alt="Top of the grip mounted on the FFB base">
</p>

## 🔌 Electronics and ODrive setup

| From | To (MKS XDrive / ODrive v3.6) |
|---|---|
| Motor A phases | M0 |
| Motor B phases | M1 |
| Encoder A: VDD 3.3 V, GND, CLK, MISO, MOSI | 3.3 V, GND, SCK, MISO, MOSI |
| Encoder A: CSn | GPIO 8 (axis 0) |
| Encoder B: same bus, CSn | GPIO 7 (axis 1) |
| Brake resistor | AUX |
| Power supply | DC input |

The configuration is a list of `odrivetool` commands:
[`firmware/odrive/odrive_config_as5048a.txt`](firmware/odrive/odrive_config_as5048a.txt).
The full walkthrough (each parameter, calibration order, tips for torque mode) is in
[docs/electronics-and-odrive.md](docs/electronics-and-odrive.md).

## 📜 License

- **Hardware** (STL, STEP, images and documentation):
  [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). You can share and remix it
  for non-commercial use, with attribution, under the same license. See [LICENSE](LICENSE).
- **Code and configuration** (`firmware/`): [MIT](firmware/LICENSE).
