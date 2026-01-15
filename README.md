# Shinogi60 Pro

A custom 60% mechanical keyboard PCB that fuses design elements from katana60-pro and tsuka60-pro projects.

## Name Origin

**Shinogi** (鎬) refers to the ridge line on a Japanese sword blade where both sides meet - symbolizing the fusion of design elements from both predecessor projects.

## Project Status

🚧 **In Development** - This project is currently in the initial design phase.

## Features

- **Layout**: 60% keyboard layout
- **Microcontroller**: nice!nano v2 (nRF52840-based, Bluetooth LE)
- **Connectivity**: Wireless (Bluetooth) + USB-C charging
- **Firmware**: ZMK (Zephyr-based Modern Keyboard firmware)
- **Switches**: MX-compatible hotswap sockets (no soldering required)
- **Lighting**: Optional SK6812 MINI-E RGB LEDs (per-key, reverse-mount)
- **Stabilizers**: PCB-mount stabilizers
- **Battery**: LiPo battery support for wireless operation
- **Power Management**: On/off switch, battery charging circuit
- **Manufacturing**: Optimized for JLCPCB production

## Requirements

- **KiCad 6.0 or later** - [Download here](https://www.kicad.org/download/)
- Git (for cloning with submodules)

## Getting Started

### Clone the Repository

```bash
git clone --recursive https://github.com/yourusername/shinogi60-pro.git
cd shinogi60-pro
```

If you already cloned without `--recursive`, initialize submodules:

```bash
git submodule update --init --recursive
```

### Open in KiCad

```bash
kicad shinogi60-pro.kicad_pro
```

Or open `shinogi60-pro.kicad_pro` directly in KiCad GUI.

## Project Structure

```
shinogi60-pro/
├── shinogi60-pro.kicad_pro    # KiCad project file (open this)
├── shinogi60-pro.kicad_sch    # Schematic
├── shinogi60-pro.kicad_pcb    # PCB layout
├── Libraries/                 # Component libraries (submodules)
│   ├── MX_Alps_Hybrid/       # Switch footprints
│   ├── marbastlib/           # Keyboard components
│   └── Type-C.pretty/        # USB-C connectors
├── jlcpcb/                   # Manufacturing files
│   ├── gerber/               # Gerber files for PCB fab
│   └── production_files/     # BOM & placement files
├── images/                   # Renders and layouts
├── CLAUDE.md                 # Developer documentation
└── README.md                 # This file
```

## Libraries

This project uses the following open-source libraries:

- **[MX_Alps_Hybrid](https://github.com/ai03-2725/MX_Alps_Hybrid)** by ai03 - Switch footprints
- **[marbastlib](https://github.com/ebastler/marbastlib)** by ebastler/MarvFPV - Keyboard components
- **[Type-C.pretty](https://github.com/ai03-2725/Type-C.pretty)** by ai03 - USB-C connectors

## Manufacturing

This PCB is designed for manufacturing at [JLCPCB](https://jlcpcb.com/) with their SMT assembly service.

### Export Production Files

1. **Gerber Files**: File → Plot → Generate Gerber and drill files
2. **BOM**: Tools → Generate BOM
3. **CPL**: File → Fabrication Outputs → Component Placement (.pos)

Production-ready files will be saved in the `jlcpcb/` directory.

### PCB Specifications

- **Size**: TBD (60% layout)
- **Layers**: 2-layer PCB
- **Thickness**: 1.6mm
- **Finish**: HASL / ENIG (recommended for better durability)
- **Color**: Your choice

## Design Philosophy

Shinogi60 Pro combines:

- Refined circuit design from predecessor projects
- Wireless-first design with Bluetooth LE (nice!nano)
- Battery-powered for true cable-free operation
- Optional per-key RGB lighting
- Hotswap sockets for easy switch replacement
- USB-C for charging and wired fallback
- Pro Micro footprint for controller compatibility

## Development

### Before Manufacturing

1. Run Design Rule Check: `Tools → Design Rules Checker`
2. Review 3D view for mechanical clearances
3. Verify all footprints against datasheets
4. Double-check BOM part numbers (LCSC availability)
5. Peer review recommended

### Contributing

This is a personal project, but suggestions and improvements are welcome! Please open an issue to discuss changes.

## Firmware

This keyboard will run **[ZMK Firmware](https://zmk.dev/)** - a modern, wireless-first keyboard firmware built on the Zephyr RTOS.

### ZMK Features

- Native Bluetooth LE support
- Low power consumption for extended battery life
- Advanced features (layers, combos, macros, mod-tap)
- GitHub Actions-based build system
- Active development and community

_Firmware configuration will be added after hardware validation._

### nice!nano

The [nice!nano](https://nicekeyboards.com/nice-nano/) is a Pro Micro-compatible wireless controller:

- Based on nRF52840 (ARM Cortex-M4, 64MHz)
- Built-in Bluetooth 5.0 LE
- USB-C connector for charging/data
- Battery management circuitry
- ZMK firmware pre-installed

## License

MIT License - See [LICENSE](LICENSE) file for details.

Copyright (c) 2022 Baris Tosun

## Acknowledgments

- **ai03** - For MX_Alps_Hybrid and Type-C libraries
- **ebastler & MarvFPV** - For marbastlib
- The keyboard community for open-source resources and inspiration

## Related Projects

- [katana60-pro](../katana60-pro/) - Predecessor project
- [tsuka60-pro](../tsuka60-pro.kicad/) - Predecessor project

---

**Shinogi60 Pro** - Where design elements meet at the ridge line.
