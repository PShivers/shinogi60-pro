# Shinogi60 Pro Modification Guide

This guide walks through converting the katana60-pro design to shinogi60-pro with nice!nano and aligned number row.

## Overview of Changes

1. **Remove ATmega32U4 circuitry** (microcontroller, crystal, USB, power regulation)
2. **Add nice!nano Pro Micro footprint** with battery support
3. **Realign number row keys** to be vertically aligned with top alpha row
4. **Update project metadata** to Shinogi60 Pro branding

---

## Part 1: Component Identification

### Components to REMOVE (Schematic)

#### Microcontroller Section

- **U1** - ATmega32U4-M (QFN-44 package)
- **Y1** - 16MHz crystal (for MCU clock)
- **C1, C2** - Crystal load capacitors (typically 22pF)

#### USB Interface Section

- **J1** - USB Type-C connector (HRO TYPE-C-31-M-12)
- **R1, R2** - USB CC pull-down resistors (5.1kΩ for USB-C)
- **U2** - SRV05-4 ESD protection IC
- **F1** - Polyfuse (500mA typical)

#### Power Regulation Section

- **U3** - 5V to 3.3V voltage regulator (if present)
- **C3, C4, C5** - Power filtering capacitors
- **R3** - Any associated resistors for voltage regulation

#### Reset Circuit (if standalone)

- **SW_RESET** - Reset button (may keep for nice!nano reset)
- **R_RESET, C_RESET** - Reset pull-up resistor and debounce cap

### Components to KEEP

✅ **Switch Matrix**

- All switch footprints (SW1-SW70 or similar)
- All diodes (D1-D70, SOD-123 package)
- Matrix traces connecting rows and columns

✅ **Stabilizers**

- STAB footprints for 2u, 6.25u, or 7u keys

✅ **RGB LEDs (Optional)**

- SK6812 MINI-E LEDs if using per-key lighting
- LED data line traces

✅ **Mounting Holes**

- PCB mounting holes

### Components to ADD

#### Nice!nano Controller

- **U_CONTROLLER** - Pro Micro footprint (from marbastlib)
  - Use: `marbastlib-various:ProMicro` or `ProMicroReversible`
  - This is where nice!nano will socket in

#### Battery Support

- **J_BAT** - Battery connector
  - Footprint: `marbastlib-various:CON_JST_ACH_BM02B` (JST 2-pin)
  - Use: `Connector:Conn_01x02_Pin` symbol
  - Connects to nice!nano BAT+/BAT-

#### Power Management

- **SW_POWER** - Power on/off switch
  - Footprint: `marbastlib-various:SW_MSK12C02-HB` (SMD slide switch)
  - Use: `Switch:SW_SPST` symbol
  - Place between battery and nice!nano

---

## Part 2: Schematic Modifications

### Step 1: Open Project in KiCad

```bash
cd /home/pshivers/code/keyboards/shinogi60-pro
kicad shinogi60-pro.kicad_pro
```

### Step 2: Open Schematic Editor

1. In KiCad project manager, double-click `shinogi60-pro.kicad_sch`
2. You'll see the full schematic with ATmega32U4 and switch matrix

### Step 3: Backup First

1. File → Save As → Copy → `shinogi60-pro-backup.kicad_sch`
2. This gives you a fallback if needed

### Step 4: Remove ATmega32U4 Section

1. **Locate the ATmega32U4 symbol** (should be labeled U1)
2. **Select and delete** the following:
   - ATmega32U4 chip (U1)
   - 16MHz crystal (Y1)
   - Crystal capacitors (C1, C2)
   - All USB-related components:
     - USB Type-C connector (J1)
     - CC resistors (R1, R2)
     - ESD protection (U2)
     - Polyfuse (F1)
   - Voltage regulator section (U3 and associated caps)

3. **Delete the wires** connecting these components
4. **Keep the switch matrix intact!** Don't touch switches, diodes, or matrix wiring

### Step 5: Add Pro Micro Footprint

1. Press **A** or Tools → Add Symbol
2. Search for: `Connector:Conn_01x24_Pin` (or use a Pro Micro-specific symbol if available)
3. Place on schematic where ATmega32U4 was
4. Label it **U1** (controller)

**Pro Micro Pinout Reference:**

```
Left Side (Top to Bottom):    Right Side (Top to Bottom):
 1. TX0  (Serial TX)           24. RAW  (Battery input)
 2. RX1  (Serial RX)           23. GND  (Ground)
 3. GND  (Ground)              22. RST  (Reset)
 4. GND  (Ground)              21. VCC  (3.3V output)
 5. D2   (GPIO)                20. F4   (GPIO/ADC)
 6. D3   (GPIO)                19. F5   (GPIO/ADC)
 7. D4   (GPIO)                18. F6   (GPIO/ADC)
 8. D5   (GPIO)                17. F7   (GPIO/ADC)
 9. D6   (GPIO)                16. B1   (GPIO)
10. D7   (GPIO)                15. B3   (GPIO)
11. B4   (GPIO)                14. B2   (GPIO)
12. B5   (GPIO)                13. B6   (GPIO)
```

### Step 6: Connect Switch Matrix to Pro Micro

1. **Identify your matrix rows and columns** in the existing schematic
   - Rows typically: 5 rows for 60% keyboard
   - Columns typically: 14-15 columns for 60% keyboard

2. **Wire matrix rows to Pro Micro pins:**
   - Example mapping (adjust as needed):
     - Row 0 → D3 (Pin 5)
     - Row 1 → D2 (Pin 6)
     - Row 2 → D1 (Pin 7)
     - Row 3 → D0 (Pin 8)
     - Row 4 → D4 (Pin 9)

3. **Wire matrix columns to Pro Micro pins:**
   - Example mapping (adjust as needed):
     - Col 0-7  → D5, D6, D7, B4, B5, B6, B2, B3
     - Col 8-14 → F7, F6, F5, F4, B1, C6, etc.

4. **Important:** Make sure you have enough GPIO pins for your matrix!
   - 60% keyboard typically needs: 5 rows × 14 columns = 19 GPIO pins minimum

### Step 7: Add Battery Connector

1. Press **A** to add symbol
2. Search: `Connector:Conn_01x02_Pin`
3. Label it **J_BAT**
4. Add properties:
   - Value: "Battery JST 2-pin"
   - Footprint: `marbastlib-various:CON_JST_ACH_BM02B`

5. **Wire Battery Connector:**
   - Pin 1 (BAT+) → Power switch → Pro Micro RAW (Pin 24)
   - Pin 2 (BAT-) → GND

### Step 8: Add Power Switch

1. Press **A** to add symbol
2. Search: `Switch:SW_SPST`
3. Label it **SW_POWER**
4. Add properties:
   - Footprint: `marbastlib-various:SW_MSK12C02-HB`

5. **Wire Power Switch:**
   - Switch connects between Battery BAT+ and Pro Micro RAW

### Step 9: Add Power Rails

1. Add **GND** symbols throughout:
   - Connect all component grounds
   - Pro Micro GND pins (3, 4, 23)
   - Battery GND
   - Switch GND connections

2. Add **VCC** label for 3.3V output from Pro Micro:
   - Pro Micro VCC (Pin 21) provides 3.3V
   - Use this for LED power if needed

### Step 10: RGB LED Data Line (Optional)

If using RGB LEDs:

1. Connect LED data line to a Pro Micro GPIO
2. Recommended: Use **D6** or another available pin
3. Update ZMK config to match this pin later

### Step 11: Update Title Block

1. In schematic editor: File → Page Settings
2. Update:
   - Title: "Shinogi60 Pro"
   - Revision: "1.0"
   - Date: Current date
   - Comment: "Wireless 60% Keyboard with nice!nano"

### Step 12: Run Electrical Rules Check (ERC)

1. Inspect → Electrical Rules Checker
2. Run ERC
3. Fix any errors:
   - Unconnected pins
   - Missing power flags
   - Conflicting net names

4. Common issues to resolve:
   - Add `PWR_FLAG` to battery positive and GND
   - Mark unused Pro Micro pins as "No Connect" (X symbol)

### Step 13: Save Schematic

1. File → Save
2. Review one more time before moving to PCB

---

## Part 3: PCB Layout Modifications

### Step 1: Open PCB Editor

1. Return to KiCad project manager
2. Double-click `shinogi60-pro.kicad_pcb`

### Step 2: Update PCB from Schematic

1. Tools → Update PCB from Schematic (or press F8)
2. This will:
   - Remove ATmega32U4 footprint and associated components
   - Add Pro Micro footprint
   - Add battery connector
   - Add power switch

3. Click "Update PCB"
4. New components will appear in a cluster - you'll place them next

### Step 3: Identify Number Row Keys

The number row keys need to be realigned. Current layout typically has:

- Number row offset by ~0.25u to the right
- Creates a staggered look

Goal: Align number keys **vertically** with QWERTY row below

1. **Find number row switches** on PCB:
   - Look for SW1 through SW13 (or labeled 1, 2, 3, etc.)
   - They'll be in a row at the top of the keyboard

2. **Select all number row switches:**
   - Click first switch
   - Hold Shift
   - Click each additional number row switch
   - All should be highlighted

### Step 4: Align Number Row Keys

**Method 1: Manual Movement**

1. With number row selected, press **M** (Move)
2. Move left by approximately **4.7625mm** (0.25u in keyboard units)
3. This removes the stagger

**Method 2: Using Coordinates**

1. Select a number row key (e.g., "1" key)
2. Note its X position: e.g., X = 52.0mm
3. Select the key directly below it (e.g., "Q" key)
4. Note its X position: e.g., X = 47.2375mm
5. Calculate difference: 52.0 - 47.2375 = 4.7625mm offset
6. Select all number row keys
7. Move left by 4.7625mm to align

**Standard 60% Key Spacing:**

- 1u key spacing: 19.05mm (center to center)
- 0.25u offset: 4.7625mm

### Step 5: Place Pro Micro Footprint

1. **Locate the Pro Micro footprint** (it will be unplaced after Update from Schematic)
2. Common placement options:
   - **Top center** of PCB (between switches)
   - **Back of PCB** (component side, upside down)
   - **Left/right edge** (vertical orientation)

3. **Placement tips:**
   - Orient so USB-C port is accessible
   - Check clearance with switch sockets
   - Consider case compatibility
   - nice!nano is 18mm × 33mm

4. **Recommended position:**
   - Center top area (above number row)
   - Coordinates: roughly X=150mm, Y=30mm (adjust based on your layout)

### Step 6: Place Battery Connector

1. Battery connector (JST 2-pin) typically goes:
   - **Near Pro Micro** for short wire runs
   - **Edge of PCB** for easy battery access
   - **Back side** to hide from view

2. Suggested position: Near Pro Micro, on back of PCB

### Step 7: Place Power Switch

1. Power switch placement options:
   - **Top edge** (easy access when in case)
   - **Side edge** (accessible from side)
   - **Near battery** for clean wiring

2. Orient switch so it's accessible when keyboard is assembled

### Step 8: Route New Connections

1. Press **X** to start routing traces
2. Route the following:
   - Battery connector to power switch
   - Power switch to Pro Micro RAW pin
   - Battery GND to Pro Micro GND
   - Matrix rows/columns to Pro Micro GPIO pins

3. **Trace width recommendations:**
   - Power traces (BAT+, RAW): 0.5mm - 1.0mm
   - Signal traces (matrix): 0.25mm - 0.4mm
   - GND: Use ground plane (flood fill)

### Step 9: Add Ground Plane

1. Select PCB layer: F.Cu (front copper)
2. Press **Ctrl+Shift+Z** or Place → Zone
3. Draw zone around entire PCB
4. Right-click → Zone Properties
5. Set Net: GND
6. Fill zone

7. Repeat for B.Cu (back copper) if 2-layer board

### Step 10: Update Title Block

1. File → Page Settings
2. Update:
   - Title: "Shinogi60 Pro"
   - Revision: "1.0"
   - Date: Current date

### Step 11: Run Design Rule Check (DRC)

1. Inspect → Design Rules Checker
2. Run DRC
3. Fix any errors:
   - Clearance violations
   - Unconnected nets
   - Silk screen overlap

### Step 12: Generate 3D View

1. View → 3D Viewer
2. Verify:
   - Number row alignment looks correct
   - Pro Micro placement doesn't interfere with switches
   - All components fit within PCB boundary

### Step 13: Save PCB

1. File → Save
2. Congratulations! Your PCB is now adapted for nice!nano with aligned number row

---

## Part 4: Final Verification Checklist

### Schematic Checklist

- [ ] ATmega32U4 and associated components removed
- [ ] Pro Micro symbol added with correct pinout
- [ ] Switch matrix connected to Pro Micro GPIO pins
- [ ] Battery connector added and wired
- [ ] Power switch added between battery and Pro Micro
- [ ] All grounds connected
- [ ] ERC passes with no critical errors
- [ ] Title block updated

### PCB Checklist

- [ ] Pro Micro footprint placed and accessible
- [ ] Battery connector placed on PCB edge
- [ ] Power switch placed and accessible
- [ ] Number row keys aligned with QWERTY row below
- [ ] All matrix traces routed to Pro Micro
- [ ] Power traces routed (BAT+ → SW → RAW)
- [ ] Ground plane added
- [ ] DRC passes with no critical errors
- [ ] 3D view looks correct
- [ ] Title block updated

### Physical Verification

- [ ] nice!nano will fit in chosen location
- [ ] USB-C port on nice!nano is accessible
- [ ] Power switch is accessible when assembled
- [ ] Battery has space (check case clearance)
- [ ] Switch clearances OK (no overlap with Pro Micro)
- [ ] Mounting holes preserved

---

## Part 5: Next Steps After Hardware Changes

### 1. Generate Production Files

Once satisfied with design:

- Export Gerbers
- Generate BOM
- Create CPL file

### 2. Order PCB Prototype

- Upload to JLCPCB
- Order nice!nano from Nice Keyboards
- Order battery (301230 or similar LiPo)

### 3. Create ZMK Firmware Config

After hardware validation:

- Create ZMK config repository
- Define matrix pin mapping (must match your schematic)
- Configure keymap
- Build and flash firmware

---

## Reference: nice!nano Pin Compatibility

nice!nano is **3.3V logic** but **5V tolerant** on most pins:

- ✅ Switch matrix: OK (switches pull to ground)
- ✅ SK6812 LEDs: OK (work with 3.3V data signal)
- ✅ Diodes: OK (passive components)

---

## Troubleshooting

**Problem:** Can't find Pro Micro symbol

- **Solution:** Use `Connector:Conn_01x24_Pin` or create custom symbol

**Problem:** Not enough GPIO pins for matrix

- **Solution:** Use a matrix layout that fits in 19 GPIO pins (5×14 or 6×13)

**Problem:** Number keys won't move in PCB

- **Solution:** Make sure you selected the switch footprints, not just the pads

**Problem:** DRC shows clearance errors

- **Solution:** Increase trace spacing or adjust component placement

**Problem:** ERC shows power errors

- **Solution:** Add PWR_FLAG symbols to battery+ and GND nets

---

## Additional Resources

- [nice!nano documentation](https://nicekeyboards.com/docs/nice-nano/)
- [ZMK firmware docs](https://zmk.dev/docs)
- [KiCad keyboard design guide](https://github.com/ruiqimao/keyboard-pcb-guide)
- [Keyboard matrix wiring guide](https://wiki.ai03.com/books/pcb-design)

---

## Questions or Issues?

Save your work frequently and don't hesitate to ask for help if you get stuck on any step!
