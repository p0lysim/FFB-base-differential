# Electronics and ODrive setup

[← Back to the README](../README.md)

## Components

| Part | Notes |
|---|---|
| 2 × BLDC 5065 140 KV | 7 pole pairs, `Kt ≈ 8.27 / KV ≈ 0.059 N·m/A` |
| MKS XDrive | Dual-axis controller compatible with ODrive v3.6, firmware 0.5.x |
| 2 × AS5048A | 14-bit absolute magnetic encoders, SPI, one on the rear end of each motor shaft |
| 2 × diametric magnets | Glued on the motor shafts, centred over the AS5048A chip |
| Brake resistor, 2 Ω | On the AUX terminals |
| DC power supply | Within the voltage range of your board |

## Wiring

```
                 ┌──────────────── MKS XDrive / ODrive v3.6 ────────────────┐
 Motor A ──3 ph──┤ M0                                                    DC ├── power supply
 Motor B ──3 ph──┤ M1                                                   AUX ├── brake resistor 2 Ω
                 │                                                          │
 AS5048A (A) ────┤ 3.3V GND SCK MISO MOSI  ·  CSn → GPIO 8 (axis 0)        │
 AS5048A (B) ────┤ (same SPI bus)          ·  CSn → GPIO 7 (axis 1)        ├── USB → PC
                 └──────────────────────────────────────────────────────────┘
```

- Both encoders share the SPI bus (SCK, MISO, MOSI). Each one has its own chip select.
- The encoder of motor A must be wired to the chip select of axis 0, so that each axis reads the encoder of its own motor.
- Keep the encoder wires short and away from the motor phase wires. The SPI signals are sensitive to noise.
- The brake resistor is needed: when you push the stick by hand, the motors act as generators and
  that energy has to go somewhere (the configuration disables regeneration into the power supply).

## ODrive configuration

The file [`firmware/odrive/odrive_config_as5048a.txt`](../firmware/odrive/odrive_config_as5048a.txt)
is **not a script**. It is a list of commands to paste **block by block** into `odrivetool`
(`pip install odrive`, then run `odrivetool`). The blocks are separated by `STOP` markers where the
board reboots or a calibration runs.

On Windows, the board may need the WinUSB driver: use [Zadig](https://zadig.akeo.ie/).

### 1. Board settings

| Parameter | Value | Meaning |
|---|---|---|
| `config.brake_resistance` | 2.0 Ω | Value of the brake resistor |
| `config.dc_bus_undervoltage_trip_level` | 5.0 V | Under-voltage trip |
| `config.dc_bus_overvoltage_trip_level` | 56.0 V | Over-voltage trip. **Lower it to match your supply and board** |
| `config.dc_max_positive_current` | 20 A | Max current drawn from the supply |
| `config.dc_max_negative_current` | −5 A | Max current pushed back to the supply |
| `config.max_regen_current` | 0 | No regeneration: the energy goes into the brake resistor |

### 2. Motor (axis 0, then the same for axis 1)

| Parameter | Value |
|---|---|
| `motor.config.pole_pairs` | 7 |
| `config.calibration_lockin.current` | 4 A |
| `motor.config.resistance_calib_max_voltage` | 12 V |
| `motor.config.current_lim` | 8 A |
| `motor.config.requested_current_range` | 20 A |

### 3. Encoder

| Parameter | Axis 0 | Axis 1 |
|---|---|---|
| `encoder.config.abs_spi_cs_gpio_pin` | 8 | 7 |
| `encoder.config.mode` | `ENCODER_MODE_SPI_ABS_AMS` | `ENCODER_MODE_SPI_ABS_AMS` |
| `encoder.config.cpr` | 16384 | 16384 |
| `encoder.config.bandwidth` | 500 | 1000 |

### 4. Controller

| Parameter | Value |
|---|---|
| `controller.config.control_mode` | `CONTROL_MODE_POSITION_CONTROL` |
| `controller.config.input_mode` | `INPUT_MODE_TRAP_TRAJ` |
| `controller.config.vel_limit` | 50 turns/s |
| `controller.config.pos_gain` | 15 |
| `controller.config.vel_gain` | 0.09 |
| `controller.config.vel_integrator_gain` | 0.5 (0.05 in the testing section) |
| `trap_traj.config.vel_limit` | 30 turns/s |
| `trap_traj.config.accel_limit` / `decel_limit` | 5 / 5 turns/s² |

### 5. Calibration and start-up

1. Reboot, then run `AXIS_STATE_MOTOR_CALIBRATION`. **Wait until the axis is back to idle**, then
   set `motor.config.pre_calibrated = True`.
2. Run `AXIS_STATE_ENCODER_OFFSET_CALIBRATION`. The motor turns a little, so the stick must be free to move.
3. Enable `startup_encoder_offset_calibration` and `startup_closed_loop_control`, save and reboot.

Each axis then calibrates its encoder offset and enters closed-loop position control at power-up.

## Tips

- **Avoid a jump at start-up.** Before you send position commands, set
  `axis.controller.input_pos = axis.encoder.pos_estimate`, so the stick does not move to a stale target.
- **Skip the calibration at every boot.** The AS5048A is absolute, so you can run the encoder offset
  calibration once, then set `encoder.config.pre_calibrated = True` and
  `startup_encoder_offset_calibration = False`. The stick will no longer move by itself at power-up.
- **Torque mode for force feedback.** Set `motor.config.torque_constant = 8.27 / 140`, then use
  `CONTROL_MODE_TORQUE_CONTROL` with `INPUT_MODE_PASSTHROUGH` and command `controller.input_torque`
  in N·m. See [how-it-works.md](how-it-works.md) for the pitch/roll to motor mixing.
- **Watchdog.** For any host-driven control, enable `axis.config.enable_watchdog` with a short
  `watchdog_timeout`, so the motors stop if the PC stops talking to the board.
- `dump_errors(odrv0)` and `odrv0.axisN.clear_errors()` are your friends while tuning.

## Useful tools

- `odrivetool`: the official ODrive console. Use the documentation of the legacy 0.5.x firmware on [docs.odriverobotics.com](https://docs.odriverobotics.com/).
- [odrive-gui](https://github.com/zauberzeug/odrive-gui): a web interface to tune gains and send commands.
