# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Shinogi60 Pro** is a custom 60% mechanical keyboard PCB design that combines elements from both the katana60-pro and tsuka60-pro projects, with additional refinements and features. The name "shinogi" (鎬) refers to the ridge line on a Japanese sword blade where both sides meet - symbolizing the fusion of design elements from both predecessor projects.

This is a KiCad-based hardware design project optimized for professional PCB manufacturing via JLCPCB.

## Opening and Editing the Project

**Requirements**: KiCad 6.0 or later (KiCad 5.x is not supported)

To work with this project:
```bash
cd shinogi60-pro/
kicad shinogi60-pro.kicad_pro
```

Or open `shinogi60-pro.kicad_pro` directly in KiCad GUI.

### Key Files
- `shinogi60-pro.kicad_pro` - Project configuration file (open this)
- `shinogi60-pro.kicad_sch` - Schematic diagram
- `shinogi60-pro.kicad_pcb` - PCB layout
- `shinogi60-pro.kicad_prl` - Project local settings (auto-generated)
- `fp-info-cache` - Footprint cache (auto-generated, in .gitignore)
- `sym-lib-table` - Symbol library references
- `fp-lib-table` - Footprint library references

## Common Workflows

### Design Review
1. Open schematic editor to review circuit design
2. Open PCB editor to review layout and routing
3. Run Design Rule Checker: Tools → Design Rules Checker

### Export Production Files
Generate Gerber files for manufacturing:
1. File → Plot
2. Configure output settings for JLCPCB
3. Generate drill files
4. Export to `jlcpcb/gerber/`

### Generate BOM and Assembly Files
For automated PCB assembly at JLCPCB:
1. Tools → Generate BOM
2. Export Component Placement List (CPL)
3. Files saved to `jlcpcb/production_files/`

### Design Rule Check
Before ordering PCBs:
```
Tools → Design Rules Checker → Run DRC
```
Resolve all violations before manufacturing.

## Architecture

### Planned Hardware Components

Based on katana60-pro and tsuka60-pro designs:
- **Microcontroller**: ATmega32U4 (QFN-44, USB-capable MCU)
- **USB Interface**: USB Type-C connector (HRO TYPE-C-31-M-12 or similar)
- **Switch Matrix**: ~60-70 switches with anti-ghosting diodes (SOD-123)
- **Hotswap Sockets**: Kailh MX-style sockets (no soldering required)
- **Stabilizers**: PCB-mount stabilizers for larger keys (2u, 6.25u, 7u)
- **Protection**: SRV05-4 ESD protection, polyfuse
- **Power**: 5V regulator with filtering, 16MHz crystal for MCU clock
- **LEDs**: Optional SK6812 MINI-E RGB addressable LEDs (reverse-mount)
- **Programming**: ISP header for firmware flashing

### Directory Structure

```
shinogi60-pro/
├── shinogi60-pro.kicad_pro/sch/pcb  # Core KiCad project files
├── Libraries/                        # Component libraries (git submodules)
│   ├── MX_Alps_Hybrid/              # ai03's switch footprints
│   ├── marbastlib/                  # ebastler's keyboard components (main branch)
│   ├── Type-C.pretty/               # USB-C connector footprints
│   └── locallib.pretty/             # Project-specific custom footprints
├── jlcpcb/                          # Manufacturing files
│   ├── gerber/                      # Gerber files (*.gbr, *.drl, *.pdf)
│   └── production_files/            # BOM and CPL for assembly
├── images/                          # Layout visualizations and renders
├── fp-lib-table                     # Footprint library paths
├── sym-lib-table                    # Symbol library paths
├── .gitignore                       # Git ignore rules
├── .gitmodules                      # Git submodule configuration
├── LICENSE                          # MIT License
└── CLAUDE.md                        # This file
```

### Libraries

**MX_Alps_Hybrid** (ai03-2725)
- Standard keyboard switch footprints
- Cherry MX, Alps, Kailh Choc support
- Hotswap and solder variants
- Note: Being deprecated in favor of MX_V2 library

**marbastlib** (ebastler/MarvFPV)
- Comprehensive keyboard component library
- Switch footprints (MX, Choc, Alps, Hi-Tek)
- Controllers, power management ICs, LEDs
- Includes 3D models for visualization
- Using `main` branch for tested footprints
- See Libraries/marbastlib/readme.md for details

**Type-C.pretty** (ai03-2725)
- USB Type-C connector footprints with 3D models
- Multiple connector types for different assembly methods

**locallib.pretty**
- Project-specific custom footprints
- Add keyboard-specific outline variations here

## Library Management

Libraries are included as git submodules. To initialize after cloning:

```bash
git submodule update --init --recursive
```

To update libraries to latest versions:

```bash
git submodule update --remote
```

To switch marbastlib to untested branch (use with caution):
```bash
# Edit .gitmodules and change:
# [submodule "Libraries/marbastlib"]
#     branch = untested
git submodule update --remote Libraries/marbastlib
```

## Manufacturing

### JLCPCB Production Files

**Gerber Files** (`jlcpcb/gerber/`):
- `*.gbr` - PCB copper layers, soldermask, silkscreen, paste
- `*.drl` - Drill files (through-hole and NPTH)
- `*-drl_map.pdf` - Drill map reference

**Assembly Files** (`jlcpcb/production_files/`):
- `BOM-*.csv` - Bill of Materials with LCSC part numbers
- `CPL-*.csv` - Component Placement List for pick-and-place
- `GERBER-*.zip` - Packaged files ready for upload

### PCB Specifications
- Standard 1.6mm FR4 thickness
- 5V USB 2.0 signaling
- Mix of SMD and through-hole components
- Hand-soldering compatible designs

## Design Philosophy

This project combines the best elements from both predecessor designs:
- **From katana60-pro**: [Document specific design elements as you develop]
- **From tsuka60-pro**: [Document specific design elements as you develop]
- **New innovations**: [Document new features and improvements]

## Development Practices

- Detailed commit messages documenting design changes
- Run Design Rule Checks before prototyping
- Validate component selection against JLCPCB/LCSC inventory
- Peer review recommended before prototype orders
- Use 3D visualization to validate mechanical fit
- Test hotswap socket clearances with actual switches

## License

MIT License (Copyright 2022 Baris Tosun)
