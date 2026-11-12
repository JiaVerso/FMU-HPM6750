# FMU-HPM6750 — RISC-V Flight Controller (HPM6750IVM2)

[![KiCad](https://img.shields.io/badge/KiCad-9.0-1a73e8?logo=kicad&logoColor=white)](https://www.kicad.org/)
[![MCU](https://img.shields.io/badge/MCU-HPM6750IVM2%20%7C%20289--BGA%20%7C%20816%20MHz-e63946)](https://www.hpmicro.com/)
[![PX4](https://img.shields.io/badge/Firmware-PX4%20fmu--v6xhpm-2e7d32)](https://px4.io/)
[![Arch](https://img.shields.io/badge/Core-Dual%20RISC--V-orange)]()
[![Layers](https://img.shields.io/badge/PCB-8%20layers%20%7C%201.6mm-6a1b9a)]()
[![License](https://img.shields.io/badge/License-CERN--OHL--S%201.2-lightgrey)]()

A Pixhawk-compatible flight controller built around the **HPMicro HPM6750IVM2**
(dual-core RISC-V, 816 MHz, 289-ball BGA) and running the official PX4 port
`px4_fmu-v6xhpm`.

## Highlights

| Item | Value |
| --- | --- |
| MCU | HPM6750IVM2 — dual RISC-V, 816 MHz, 2 MB SRAM |
| IMU | ICM-42688-P (SPI3) + BMI088 (SPI2, redundant) |
| Baro / Mag | BMP390 / BMM350 (I2C3) |
| Storage | 16 MB QSPI NOR (W25Q128), 32 KB FRAM, 2 × 8 KB EEPROM, microSD |
| Power | 5 V input, LTC4417 ORing, MP2315 buck 3.3 V/3 A, LP5907 sensor LDO |
| IO | 8 × PWM, 5 × UART, 3 × I2C, 3 × SPI, 2 × CAN, USB-C (16P) |
| PCB | 8 layers, 1.6 mm, ENIG, impedance-controlled |

## Repository layout

```
FMU-HPM6750/
├─ FMU-H6750.kicad_pro        # KiCad project
├─ FMU-H6750.kicad_sch        # root sheet (7 sub-sheets)
├─ 02_mcu.kicad_sch           # MCU + clocks + reset/boot + decoupling
├─ 03_power.kicad_sch         # 5V input, ORing, buck, LDO, sensing
├─ 04_storage.kicad_sch       # QSPI flash, microSD, FRAM, EEPROM
├─ 05_sensors.kicad_sch       # IMU / baro / mag + heater
├─ 06_comms.kicad_sch         # USB-C 16P, CAN, telemetry, RC
├─ 07_actuator.kicad_sch      # PWM outputs, buzzer, safety switch, LEDs
├─ 08_connectors.kicad_sch    # connector pin-out reference
├─ 02b_decoupling.kicad_sch   # VDD/VSS decoupling (standalone)
├─ 02c_power_unit.kicad_sch   # power unit + decoupling (standalone)
├─ FMU-H6750.kicad_pcb        # PCB layout (BGA-289 placed, unrouted)
├─ FMU_H6750.kicad_sym        # project symbol library
└─ sym-lib-table              # library table
```

## Pin assignment

The MCU symbol uses **HPM pad names** as pin numbers (e.g. `PZ01`, `PE27`) so they
map 1:1 onto the PX4 `GPIO_PORTx|GPIO_PINn` macros. 289 balls are all present in
the schematic; the 113 pins used by the firmware carry global labels, the
remaining ones are marked no-connect.

## Getting started

```bash
git clone https://github.com/JiaVerso/FMU-HPM6750.git
cd FMU-HPM6750
# open FMU-H6750.kicad_pro with KiCad 8.0 or newer
```

Firmware (PX4):

```bash
git clone https://github.com/hpmicro/PX4-Autopilot -b feature/px4_v1.16.0_hpm_fmuv6x
cd PX4-Autopilot
make px4_fmu-v6xhpm_default
```

## Status

- [x] Schematic complete, ERC-style self check passed (0 errors, 0 floating pins)
- [x] All 289 BGA balls accounted for
- [ ] PCB routing (BGA fan-out in progress)
- [ ] Hardware bring-up

## License

Hardware design files released under **CERN-OHL-S-1.2**.
PX4 firmware components remain under their original licenses.
