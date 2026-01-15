# Component Identification Reference

Quick reference for converting katana60-pro to shinogi60-pro with nice!nano.

---

## Components to REMOVE ❌

### From Schematic and PCB

| Component | Type | Package | Purpose | Why Remove |
| ----------- | ------ | --------- | --------- | ------------ |
| **U1** | ATmega32U4-M | QFN-44 | Main microcontroller | Replaced by nice!nano |
| **Y1** | 16MHz Crystal | SMD | MCU clock source | nice!nano has internal oscillator |
| **C1, C2** | 22pF Capacitors | 0805 | Crystal load caps | No crystal needed |
| **J1** | USB Type-C | HRO TYPE-C-31-M-12 | USB data connection | nice!nano has built-in USB |
| **R1, R2** | 5.1kΩ Resistors | 0805 | USB-C CC pull-downs | No USB connector on PCB |
| **U2** | SRV05-4 | SOT-23-6 | USB ESD protection | No USB data lines to protect |
| **F1** | 500mA Polyfuse | 1206 | USB overcurrent protection | Battery has different protection |
| **U3** | 3.3V Regulator | SOT-223 | Voltage regulation | nice!nano has built-in regulation |
| **C3-C5** | 10µF, 1µF Caps | 0805 | Power filtering | nice!nano handles power |
| **R3** | Various | 0805 | Regulator feedback | No regulator needed |

### Additional Components That May Exist

| Component | Remove? | Notes |
| ----------- | --------- | ------- |
| **ISP Header** | ❌ Remove | nice!nano programmed via USB |
| **Reset Button** | ⚠️ Optional | Can keep for nice!nano reset, or remove |
| **Boot Button** | ❌ Remove | Not needed for nice!nano |
| **USB Shield Ground** | ❌ Remove | Part of USB connector |

---

## Components to KEEP ✅

### Switch Matrix Components

| Component | Quantity | Package | Purpose | Notes |
| ----------- | ---------- | --------- | --------- | ------- |
| **SW1-SW70** | ~60-70 | MX Hotswap | Key switches | Keep all! |
| **D1-D70** | ~60-70 | SOD-123 | Anti-ghosting diodes | Keep all! |
| **Matrix Traces** | N/A | PCB traces | Row/column connections | Don't touch! |
| **STAB1-STAB5** | 3-5 | PCB mount | Stabilizers | Keep all! |

### Optional RGB Lighting

| Component | Quantity | Package | Purpose | Keep? |
|-----------|----------|---------|---------|-------|
| **LED1-LED70** | ~60-70 | SK6812 MINI-E | Per-key RGB | ✅ Yes, if using RGB |
| **C_LED** | 1-2 | 0805 | LED power filtering | ✅ Yes, if using RGB |

### Mechanical Components

| Component | Quantity | Purpose | Keep? |
|-----------|----------|---------|-------|
| **Mounting Holes** | 4-6 | PCB to case mounting | ✅ Yes |
| **PCB Outline** | 1 | Board shape | ✅ Yes |

---

## Components to ADD ➕

### Controller Module

| Component | Type | Footprint | Symbol | Purpose |
|-----------|------|-----------|--------|---------|
| **U1** | nice!nano v2 | `marbastlib-various:ProMicro` or `ProMicroReversible` | `Connector:Conn_01x24_Pin` | Wireless controller |

**Pinout Reference:**

```
      nice!nano Pin Layout
      ┌─────────────────┐
   1  │ TX0         RAW │ 24  ← Battery input (3.7V-5V)
   2  │ RX1         GND │ 23
   3  │ GND         RST │ 22
   4  │ GND         VCC │ 21  ← 3.3V output
   5  │ D2           F4 │ 20
   6  │ D3           F5 │ 19
   7  │ D4           F6 │ 18
   8  │ D5           F7 │ 17
   9  │ D6           B1 │ 16
  10  │ D7           B3 │ 15
  11  │ B4           B2 │ 14
  12  │ B5           B6 │ 13
      └─────────────────┘
       (Top view, USB-C up)
```

### Battery & Power

| Ref | Component | Footprint | Symbol | Connections |
|-----|-----------|-----------|--------|-------------|
| **J_BAT** | JST 2-pin connector | `marbastlib-various:CON_JST_ACH_BM02B` | `Connector:Conn_01x02_Pin` | Pin 1: BAT+<br>Pin 2: GND |
| **SW_PWR** | SPST slide switch | `marbastlib-various:SW_MSK12C02-HB` | `Switch:SW_SPST` | Between BAT+ and RAW |
| **Battery** | LiPo 3.7V | Off-board | N/A | 301230 (150-400mAh) |

---

## Component Specifications

### nice!nano v2 Specifications

- **MCU:** nRF52840 (ARM Cortex-M4, 64MHz)
- **RAM:** 256KB
- **Flash:** 1MB
- **Bluetooth:** BLE 5.0
- **Logic Level:** 3.3V (5V tolerant inputs)
- **USB:** USB-C (for charging/programming)
- **Battery Management:** Built-in LiPo charger
- **Dimensions:** 18mm × 33mm
- **Mounting:** 2×12 pin headers (2.54mm pitch)

### Battery Specifications

Recommended LiPo batteries:

- **301230** (3.0×12×30mm) - 150mAh
- **401230** (4.0×12×30mm) - 200mAh
- **501235** (5.0×12×35mm) - 400mAh

Requirements:

- **Voltage:** 3.7V nominal (3.0V-4.2V)
- **Connector:** JST-ACH 1.25mm pitch
- **Protection:** Built-in PCM recommended
- **Capacity:** 150-400mAh (based on size constraints)

### Power Switch Specifications

**MSK12C02-HB** (recommended):

- **Type:** SPST slide switch
- **Rating:** 50V DC, 0.3A
- **Size:** 8.4mm × 3.8mm
- **Mounting:** SMD

---

## GPIO Pin Assignments

### Typical 60% Matrix Layout

**Matrix Size:** 5 rows × 14 columns = 70 keys

#### Row Assignments

| Row | Pin | Net Name | Keys |
|-----|-----|----------|------|
| Row 0 | D3 (Pin 6) | ROW0 | Esc, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, -, =, Backspace |
| Row 1 | D2 (Pin 5) | ROW1 | Tab, Q, W, E, R, T, Y, U, I, O, P, [, ], \ |
| Row 2 | D1 (Pin 7) | ROW2 | Caps, A, S, D, F, G, H, J, K, L, ;, ', Enter |
| Row 3 | D0 (Pin 8) | ROW3 | Shift, Z, X, C, V, B, N, M, ,, ., /, Shift |
| Row 4 | D4 (Pin 9) | ROW4 | Ctrl, Win, Alt, Space, Alt, Win, Menu, Ctrl |

#### Column Assignments

| Col | Pin | Net Name |
|-----|-----|----------|
| Col 0 | F7 (Pin 17) | COL0 |
| Col 1 | F6 (Pin 18) | COL1 |
| Col 2 | F5 (Pin 19) | COL2 |
| Col 3 | F4 (Pin 20) | COL3 |
| Col 4 | B1 (Pin 16) | COL4 |
| Col 5 | B3 (Pin 15) | COL5 |
| Col 6 | B2 (Pin 14) | COL6 |
| Col 7 | B6 (Pin 13) | COL7 |
| Col 8 | B5 (Pin 12) | COL8 |
| Col 9 | B4 (Pin 11) | COL9 |
| Col 10 | D7 (Pin 10) | COL10 |
| Col 11 | D6 (Pin 9) | COL11 |
| Col 12 | D5 (Pin 8) | COL12 |
| Col 13 | D4 (Pin 7) | COL13 |

**Note:** Adjust these based on your actual matrix layout in the schematic!

#### Optional: RGB LED Data

| Function | Pin | Notes |
|----------|-----|-------|
| LED Data | TX0 (Pin 1) or D6 | Depends on ZMK support |

---

## Wiring Connections Summary

### Power Path

```
Battery (+) ──→ Power Switch ──→ nice!nano RAW (Pin 24)
Battery (-) ──→ GND ──────────→ nice!nano GND (Pin 23)
```

### Data Path

```
Switch Matrix Rows ──→ nice!nano Row Pins (D0-D4)
Switch Matrix Cols ──→ nice!nano Col Pins (F4-F7, B1-B6, D5-D7)
```

### Optional RGB

```
LED Data Out ──→ nice!nano TX0 or D6
LED VCC ──────→ nice!nano VCC (3.3V, Pin 21)
LED GND ──────→ GND
```

---

## PCB Layout Changes Summary

### Number Row Alignment

**Before:** Staggered (0.25u offset)

```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───────┐
│Esc│ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │ 0 │ - │ = │Backsp │
└───┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─────┤
    │ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │ [ │ ] │  \  │
    └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴─────┘
```

**After:** Aligned (no offset)

```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───────┐
│Esc│ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │ 0 │ - │ = │Backsp │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───────┤
│Tab│ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │ [ │ ] │   \   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───────┘
```

**Movement:** Shift number row LEFT by 4.7625mm (0.25u)

---

## Bill of Materials (BOM) Changes

### Removed Components Cost Savings

| Component | Qty | Est. Cost | Total |
|-----------|-----|-----------|-------|
| ATmega32U4-M | 1 | $4.50 | $4.50 |
| Crystal 16MHz | 1 | $0.15 | $0.15 |
| USB Type-C | 1 | $0.50 | $0.50 |
| SRV05-4 | 1 | $0.20 | $0.20 |
| Voltage Reg | 1 | $0.30 | $0.30 |
| Passives | ~10 | $0.05 | $0.50 |
| **Subtotal** | | | **~$6.15** |

### Added Components Cost

| Component | Qty | Est. Cost | Total |
|-----------|-----|-----------|-------|
| nice!nano v2 | 1 | $25.00 | $25.00 |
| Battery 301230 | 1 | $3.00 | $3.00 |
| JST Connector | 1 | $0.10 | $0.10 |
| Power Switch | 1 | $0.20 | $0.20 |
| Pin Headers (24p) | 2 | $0.50 | $1.00 |
| **Subtotal** | | | **~$29.30** |

**Net Cost Difference:** +$23.15 per keyboard (but you gain wireless!)

---

## Testing Checklist

After assembly, test in this order:

### 1. Power Tests

- [ ] Battery voltage measures 3.7V
- [ ] Power switch turns on/off properly
- [ ] nice!nano powers up (red LED visible)
- [ ] USB-C charges battery

### 2. Connectivity Tests

- [ ] nice!nano shows up as Bluetooth device
- [ ] Can pair with computer
- [ ] USB connection works for programming

### 3. Matrix Tests

- [ ] Flash test firmware
- [ ] Test each key press registers
- [ ] Verify no ghosting or chattering
- [ ] Check matrix continuity if keys don't work

### 4. LED Tests (if applicable)

- [ ] RGB LEDs light up
- [ ] Color changes work
- [ ] No flickering

---

## Common Issues & Solutions

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| nice!nano won't power on | Switch wiring reversed | Check battery polarity |
| Keys don't register | Matrix pins wrong | Verify pin assignments in ZMK |
| Battery won't charge | Battery polarity wrong | Check JST connector wiring |
| Bluetooth won't pair | Firmware not flashed | Flash ZMK firmware |
| Some keys don't work | Cold solder joint | Reflow diode/switch connections |
| LEDs don't work | Wrong data pin | Check LED data line in ZMK config |

---

## Reference Documents

- [nice!nano Pinout Diagram](https://nicekeyboards.com/docs/nice-nano/pinout-schematic)
- [ZMK GPIO Configuration](https://zmk.dev/docs/config/kscan)
- [Keyboard Matrix Guide](https://pcbheaven.com/wikipages/How_Key_Matrices_Works/)

---

This reference should be used alongside the main MODIFICATION_GUIDE.md for complete conversion instructions.
