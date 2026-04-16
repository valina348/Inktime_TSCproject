# InkTime Smartwatch

> An open-source, low-cost smartwatch built around the nRF52840 SoC and an e-paper display.

---

## Block Diagram

```
                        ┌─────────────────────────────────────────────────────┐
                        │                   nRF52840 (U1)                     │
                        │              ARM Cortex-M4F @ 64 MHz                │
                        │           BLE 5.0 | USB 2.0 FS | SWD               │
                        └──┬──────┬──────┬──────┬──────┬──────┬──────┬───────┘
                           │ SPI  │ I2C  │ I2C  │ I2C  │ I2C  │GPIO  │ USB
                           │      │      │      │      │      │      │
              ┌────────────┘      │      │      │      │      │      └──────────────┐
              │                   │      │      │      │      │                     │
        ┌─────▼──────┐     ┌──────▼─┐ ┌─▼────┐ │  ┌───▼───┐  │              ┌─────▼──────┐
        │  WSH-12561 │     │BMA421  │ │MAX   │ │  │DRV2605│  │              │KH-TYPE-C   │
        │  E-Paper   │     │  IMU   │ │17048 │ │  │Haptic │  │              │  USB-C     │
        │  Display   │     │ 6-axis │ │Fuel  │ │  │Driver │  │              │ Connector  │
        └────────────┘     └────────┘ │Gauge │ │  └───┬───┘  │              └─────┬──────┘
                                      └──────┘ │      │      │                    │
                                               │  ┌───▼───┐  │              ┌─────▼──────┐
                                               │  │FIT0774│  │              │USBLC6-2SC6Y│
                                        I2C    │  │ ERM   │  │              │ESD Protect │
                                        │      │  │ Motor │  │              └─────┬──────┘
                                        │      │  └───────┘  │                    │
                                  ┌─────▼──────────────────┐ │              ┌─────▼──────┐
                                  │    BQ25180YBGR         │ │              │   VBUS     │
                                  │    LiPo Charger        │◄┘              │  detect    │
                                  └─────┬──────────────────┘                └────────────┘
                                        │
                              ┌─────────▼──────────┐
                              │  AKY0106 LiPo Bat  │
                              │  400 mAh / 3.7V    │
                              └─────────┬──────────┘
                                        │ VBAT
                              ┌─────────▼──────────┐
                              │   RT6160AWSC        │
                              │   Boost Converter   │
                              │  (EPD HV supply)    │
                              └─────────────────────┘

External:
  32.768 kHz Crystal ──► nRF52840 (LFXO — RTC)
  2450AT18B100E Ant  ──► nRF52840 (RF pin, via matching network)
  TC2030-IDC         ──► nRF52840 (SWD debug)
  3x Tactile Buttons ──► nRF52840 (GPIO, active low)
```

---

## Bill of Materials (BOM)

| Ref | Component | Description | Package | Qty | JLCPCB | Datasheet |
|-----|-----------|-------------|---------|-----|--------|-----------|
| U1 | nRF52840-QIAA | MCU, BLE 5.0, ARM Cortex-M4F, 1MB Flash, 256KB RAM | QFN-73 | 1 | [C190794](https://jlcpcb.com/partdetail/NordicSemiconductor-nRF52840_QIAA/C190794) | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.7.pdf) |
| U2 | BQ25180YBGR | LiPo charger, I2C, 1A, integrated power path | DSBGA-9 | 1 | [C2678722](https://jlcpcb.com/partdetail/TexasInstruments-BQ25180YBGR/C2678722) | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) |
| U3 | MAX17048G+T10 | Fuel gauge, 1% SOC accuracy, I2C | SOT-23-6 | 1 | [C82344](https://jlcpcb.com/partdetail/MaximIntegrated-MAX17048G_T10/C82344) | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf) |
| U4 | RT6160AWSC | DC/DC boost converter for EPD supply | SOT-23-6 | 1 | [Search RT6160AWSC](https://jlcpcb.com/parts/componentSearch?searchTxt=RT6160AWSC) | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf) |
| U5 | DRV2605YZFR | Haptic driver, I2C, LRA/ERM support | DSBGA-16 | 1 | [C2062186](https://jlcpcb.com/partdetail/TexasInstruments-DRV2605YZFR/C2062186) | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) |
| IC1 | BMA421 | 6-axis IMU, accelerometer + gyroscope, I2C | LGA-12 | 1 | [Search BMA421](https://jlcpcb.com/parts/componentSearch?searchTxt=BMA421) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma421-ds000.pdf) |
| IC2 | USBLC6-2SC6Y | USB ESD protection | SOT-363 | 1 | [C2827654](https://jlcpcb.com/partdetail/STMicroelectronics-USBLC6_2SC6Y/C2827654) | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) |
| J1 | KH-TYPE-C-16P | USB-C connector, 16-pin | SMD | 1 | [Search KH-TYPE-C-16P](https://jlcpcb.com/parts/componentSearch?searchTxt=KH-TYPE-C-16P) | - |
| J2 | TC2030-IDC | Tag-Connect SWD debug connector, 10-pin | THT | 1 | - | [Datasheet](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-IDC_1.pdf) |
| ANT1 | 2450AT18B100E | 2.4 GHz chip antenna, 50 Ω | SMD | 1 | [Search 2450AT18B100E](https://jlcpcb.com/parts/componentSearch?searchTxt=2450AT18B100E) | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E/2450AT18B100E.pdf) |
| L1 | FTC252012SR47MBCA | Power inductor 4.7 µH | 1008 | 1 | [Search FTC252012SR47M](https://jlcpcb.com/parts/componentSearch?searchTxt=FTC252012SR47M) | - |
| L2 | - | Inductor 10 µH (DC/DC) | 0402 | 1 | - | - |
| L3 | - | Inductor 15 nH (RF matching) | 0402 | 1 | - | - |
| L7 | - | Inductor 3.9 nH (antenna matching) | 0402 | 1 | - | - |
| M1 | DFRobot FIT0774 | ERM haptic motor (shaker) | - | 1 | - | [Datasheet](https://wiki.dfrobot.com/Vibration_Motor_SKU__FIT0774) |
| BAT1 | AKY0106 | LiPo battery 400 mAh, 3.7V | - | 1 | - | - |
| DSP1 | WSH-12561 | E-paper display, SPI interface | - | 1 | - | - |
| X1 | - | Crystal 32.768 kHz | SMD | 1 | - | - |
| SW1–SW3 | - | Tactile push button | SMD | 3 | - | - |
| R1–Rn | - | Resistors, various values | 0201 | - | - | - |
| C1–Cn | - | Capacitors ≤100nF → 0201, >100nF → 0402 | 0201/0402 | - | - | - |

---

## Hardware Description

### MCU — nRF52840 (U1)

The nRF52840 is the central processing unit of InkTime. It integrates:

- ARM Cortex-M4F running at 64 MHz with hardware FPU
- Bluetooth Low Energy 5.0 (used for time synchronization and smartphone notifications)
- USB 2.0 Full Speed (charging detection and firmware updates over DFU)
- 1 MB Flash / 256 KB RAM
- Multiple hardware peripherals: SPI, I2C, UART, ADC, PWM, RTC

All peripherals connect directly to the nRF52840:
- **SPI** — e-paper display (WSH-12561)
- **I2C** — IMU (BMA421), fuel gauge (MAX17048), LiPo charger (BQ25180), haptic driver (DRV2605)
- **GPIO** — buttons, EPD control lines, interrupt lines, HAPTIC_EN

A 32.768 kHz crystal (X1) is connected to XL1/XL2 pins to drive the internal low-frequency oscillator (LFXO), used for the real-time clock (RTC) and sleep timers with µA-level current consumption.

---

### Power Path

```
USB-C (VBUS 5V)
    └──► BQ25180YBGR (LiPo charger, up to 1A)
              ├──► AKY0106 LiPo Battery (400 mAh, 3.7V)
              └──► 3.3V rail (VREG) ──► nRF52840 + all peripherals

VBAT (3.7V from battery)
    └──► RT6160AWSC (boost converter)
              └──► EPD high-voltage supply (PREVGH, PREVGL, GDR, VCOM)
```

**BQ25180YBGR** manages LiPo charging with integrated power path — the system can run from USB while simultaneously charging the battery. It communicates with the MCU over I2C and raises the `PMIC_INT` line on fault or status events (charge complete, overtemperature, etc.).

**RT6160AWSC** is a boost converter that generates the higher voltages required by the e-paper display driver circuit. The EPD requires both positive and negative rail voltages (PREVGH ≈ +20V, PREVGL ≈ −20V) for bistable switching of the electrophoretic ink particles.

**MAX17048G+T10** is a ModelGauge fuel gauge that estimates battery state-of-charge (SOC) with 1% accuracy using a single-cell LiPo model, communicating over I2C. It raises an `ALERT` interrupt when the battery falls below a programmable low-threshold.

---

### E-Paper Display — WSH-12561

The e-paper display uses SPI communication plus 3 control GPIO lines:

| Signal | Direction | Description |
|--------|-----------|-------------|
| SCK | MCU → EPD | SPI clock |
| MOSI | MCU → EPD | SPI data (write only) |
| EPD_CS | MCU → EPD | Chip select (active low) |
| EPD_DC | MCU → EPD | Data / Command select |
| EPD_RST | MCU → EPD | Hardware reset (active low) |
| EPD_BUSY | EPD → MCU | Busy flag — MCU must wait before next command |

The EPD drive circuit (powered by RT6160AWSC boost converter) generates PREVGH, PREVGL, and GDR voltages needed to drive the bistable ink display. A MOSFET (Q3 — DMG2305UX) gates the EPD power supply so it can be fully cut when the display is not refreshing, saving power.

E-paper is chosen for InkTime because it consumes power **only during refresh** (not while displaying a static image), making it ideal for a low-power wearable.

---

### IMU — BMA421

6-axis inertial measurement unit connected via I2C at 400 kHz. Provides:
- 3-axis accelerometer — step counting, gesture detection, tilt/orientation
- 3-axis gyroscope — motion tracking, wrist-raise detection
- Two interrupt lines (`IMU_INT1`, `IMU_INT2`) for wake-on-motion and data-ready events

The IMU is configured for I2C mode by pulling `CSB` high (to 3V3). The I2C address is set via the `SDO` pin (GND = 0x18, 3V3 = 0x19).

---

### Haptic Feedback — DRV2605 + FIT0774

The DRV2605YZFR haptic driver controls the FIT0774 ERM (eccentric rotating mass) vibration motor. It connects to the MCU over I2C and is enabled via the `HAPTIC_EN` GPIO. The driver supports 124 built-in haptic waveform effects (notification patterns, alerts, confirmations) without requiring MCU intervention during playback.

---

### USB — KH-TYPE-C-16P + USBLC6-2SC6Y

USB-C connector with USBLC6-2SC6Y ESD protection on D+ and D− lines. CC1 and CC2 pins each have a 5.1 kΩ pull-down resistor to GND, which is mandatory for USB-C devices to be recognized as UFP (Upstream Facing Port / device) by USB-C chargers and hosts.

The nRF52840 detects VBUS presence via a GPIO to hand off charging control to the BQ25180. USB D+/D− connect directly to the nRF52840's integrated USB transceiver for DFU (Device Firmware Update) support.

---

### Antenna — 2450AT18B100E

2.4 GHz chip antenna placed at the edge of the PCB. Design rules followed:
- No copper pours (ground plane or power planes) under the antenna keepout area
- No signal traces routed under the antenna
- PCB cutout under the antenna to reduce parasitic capacitance to ground
- Pi-network impedance matching (inductor + two capacitors) between the nRF52840 RF pin and the antenna feed, targeting 50 Ω impedance match at 2.4 GHz

---

### SWD Debug — TC2030-IDC

Tag-Connect TC2030-IDC footprint for in-circuit programming and debugging. No physical connector is soldered — a spring-loaded probe clips onto the 6 pads. Test pads are exposed and labeled in silkscreen:

| Test Pad | Signal |
|----------|--------|
| TP_SWDIO | SWDIO |
| TP_SWDCLK | SWDCLK |
| TP_RESET | RESET |
| TP_3.3V | 3V3 |
| TP_GND | GND |
| TP_SWO | SWO (trace output) |

---

## nRF52840 Pin Assignment

| Pin | Net | Connected To | Interface | Notes |
|-----|-----|--------------|-----------|-------|
| P0.00 | XL1 | 32.768 kHz crystal | LFXO | Low-frequency clock for RTC |
| P0.01 | XL2 | 32.768 kHz crystal | LFXO | Low-frequency clock for RTC |
| P0.09 | RF/ANT | Antenna matching network → ANT1 | RF | Dedicated RF pin — PCB cutout required |
| P0.13 | BTN1 | Tactile button SW_UP | GPIO | Active low, 10K pull-up, 1µF debounce |
| P0.14 | BTN2 | Tactile button SW_DN | GPIO | Active low, 10K pull-up, 1µF debounce |
| P0.26 | BTN3 | Tactile button SW_ENT | GPIO | Active low, 10K pull-up, 1µF debounce |
| P0.xx | SDA | BMA421, MAX17048, BQ25180, DRV2605 | I2C | Shared bus, 4.7 kΩ pull-up to 3V3 |
| P0.xx | SCL | BMA421, MAX17048, BQ25180, DRV2605 | I2C | Shared bus, 4.7 kΩ pull-up to 3V3 |
| P1.xx | SCK | E-paper display (WSH-12561) | SPI | |
| P1.xx | MOSI | E-paper display (WSH-12561) | SPI | |
| P1.xx | EPD_CS | E-paper chip select | GPIO | Active low |
| P1.xx | EPD_DC | E-paper data/command | GPIO | High = data, Low = command |
| P1.xx | EPD_RST | E-paper reset | GPIO | Active low |
| P1.xx | EPD_BUSY | E-paper busy flag | GPIO input | MCU polls or uses interrupt |
| P1.xx | HAPTIC_EN | DRV2605 enable | GPIO | Active high |
| P1.xx | PMIC_INT | BQ25180 interrupt | GPIO input | Active low, fault/status events |
| P1.xx | IMU_INT1 | BMA421 interrupt 1 | GPIO input | Wake-on-motion |
| P1.xx | IMU_INT2 | BMA421 interrupt 2 | GPIO input | Data ready |
| P1.xx | ALERT | MAX17048 alert | GPIO input | Low battery threshold |
| SWDIO | SWDIO | TC2030 pin 2 / TP_SWDIO | SWD | Programming & debug |
| SWDCLK | SWDCLK | TC2030 pin 4 / TP_SWDCLK | SWD | Programming & debug |
| D+ | D+ | USB-C D+ (via USBLC6 ESD) | USB | Full-speed USB transceiver |
| D− | D− | USB-C D− (via USBLC6 ESD) | USB | Full-speed USB transceiver |
| VBUS | VBUS | USB-C VBUS detect | GPIO input | 5V presence detection |

> **Note:** Pins marked `P0.xx` / `P1.xx` are assigned in firmware and will be finalized in the GNG revision. The I2C bus is shared between BMA421, MAX17048, BQ25180, and DRV2605 — each has a unique I2C address.

---

## Power Consumption Estimates

| State | Active Components | Estimated Current |
|-------|-------------------|-------------------|
| Deep sleep | nRF52840 RTC only | ~3 µA |
| BLE advertising | MCU + BLE radio | ~3 mA avg |
| E-paper refresh | MCU + EPD + boost converter | ~30 mA peak (1–2 s) |
| IMU active | MCU + IMU | ~1.5 mA |
| Full operation (BLE + EPD + IMU) | All | ~35 mA peak |

**Battery life estimate** (400 mAh, typical usage: BLE advertising + sleep + 1 EPD refresh/min):

```
Average current ≈ 3 mA (BLE) × 10% duty + 3 µA × 90% + 30 mA × (2s/60s)
               ≈ 0.3 mA + 0.003 mA + 1 mA
               ≈ ~1.3 mA average

Battery life ≈ 400 mAh / 1.3 mA ≈ ~13 days
```

---

## Design Log

### PCB Design Decisions

- **4-layer stackup** used to allow a dedicated ground plane on an inner layer, improving RF performance and reducing EMI near the BLE antenna.
- **Antenna placement** at the top edge of the PCB with a copper-free keepout zone and PCB cutout beneath it, per the 2450AT18B100E datasheet recommendations.
- **Decoupling capacitors** (100 nF, 0201) placed as close as possible to each VDD/VDDH/DEC pin of the nRF52840, per Nordic's hardware design guide.
- **Via stitching** applied between top and bottom ground planes, especially around the RF section, to minimize ground impedance at RF frequencies.
- **All components placed on TOP layer** only, as required by the project specification.
- **Power traces** routed at 0.3 mm width (VCC, VBUS, VBAT, 3V3); signal traces at minimum 0.15 mm.
- **No right-angle bends** on any trace — all routing uses 45° angles.
- **USB-C CC resistors** (5.1 kΩ to GND on CC1 and CC2) ensure correct USB-C device recognition by any standard charger.
- **Battery connected directly to test pads** (TP_VBAT, TP_BAT_GND) on the PCB instead of using the JST connector, to save vertical space inside the enclosure.

### Known Accepted Errors

- DRC "Dimension" errors caused by the placement of the three tactile buttons and the USB-C connector at the PCB edge — these are by design to align with the physical openings in the enclosure and are accepted per project specification.
- ERC warning "Only INPUT pins on NET ID" — accepted per project specification (known Fusion360 library artifact).

---

## License

This project is licensed under the **Apache License 2.0**.
