# MiSTer FPGA - Technical Overview

## Introduction

MiSTer is an open-source project that uses Field-Programmable Gate Array (FPGA) technology to recreate vintage computers, game consoles, and arcade systems at the hardware level. Unlike software emulation, MiSTer implements actual hardware circuits, providing cycle-accurate reproduction of original systems.

### MiSTer Pi and Clone Devices

**Note**: "MiSTer Pi" is a branding used by some clone manufacturers and has **no connection to Raspberry Pi**. These are FPGA-based devices that implement the MiSTer project specifications, typically using alternative hardware configurations while maintaining compatibility with MiSTer cores and software.

Common MiSTer clone characteristics:
- **Same FPGA**: Often use Intel Cyclone V or similar FPGAs
- **ARM Processor**: Linux-capable ARM SoC for system management
- **Compatibility**: Run standard MiSTer cores (.rbf files)
- **Cost**: Usually lower than official DE10-Nano
- **Form Factor**: Various designs (handheld, compact cases, etc.)
- **Quality Variance**: Build quality and component selection varies by manufacturer

The "Pi" in "MiSTer Pi" is simply branding/marketing and does not indicate any relationship to the Raspberry Pi platform or hardware.

## Hardware Platform

### Official Platform: DE10-Nano

**Manufacturer**: Terasic Technologies  
**Base Chip**: Intel Cyclone V SoC FPGA  
**Status**: Official reference platform for MiSTer project

### Clone Devices (Including MiSTer Pi)

Clone devices typically replicate the DE10-Nano's core functionality with varying implementations:
- Similar or identical FPGA (Cyclone V variants)
- Comparable ARM processor specifications
- Compatible GPIO pinouts for I/O boards
- May include integrated features (built-in I/O, different connectors)
- Quality and component sourcing varies by manufacturer

#### FPGA Specifications
- **FPGA Model**: Cyclone V SE 5CSEBA6U23I7
- **Logic Elements (LEs)**: 110,000
- **Embedded Memory**: 5,570 Kbits
- **Digital Signal Processing (DSP) Blocks**: 112
- **Fractional PLLs**: 6
- **Hard Memory Controllers**: 2

#### ARM Processor (HPS - Hard Processor System)
- **CPU**: Dual-core ARM Cortex-A9
- **Clock Speed**: 800 MHz (925 MHz capable)
- **Purpose**: Runs Linux OS, handles I/O, core loading, and system management
- **RAM**: 1 GB DDR3 SDRAM (shared between FPGA and HPS)

#### Onboard Peripherals
- **HDMI Output**: HDMI 1.4a transmitter (ADV7513)
- **Video DAC**: Analog Devices ADV7123 (VGA output)
- **Audio Codec**: Realtek ALC5645
- **Ethernet**: 10/100/1000 Mbps Gigabit Ethernet
- **USB**: 2× USB 2.0 host ports (via USB hub)
- **MicroSD**: Card slot for storage
- **GPIO**: 2× 40-pin headers (Arduino compatibility)

### Add-On Boards (I/O Board)

The MiSTer project typically uses expansion boards that connect via GPIO headers:

#### Standard I/O Board Features
- **VGA Output**: 6-bit RGB (18-bit color)
- **Additional Analog Video**: Component, S-Video, Composite
- **Audio Output**: 3.5mm stereo jack, optical S/PDIF
- **User I/O Port**: DB15/DB9 connector for controllers and peripherals
- **Fan Connector**: 5V fan header for cooling
- **Real-Time Clock (RTC)**: Battery-backed timekeeping
- **Additional USB Ports**: Extra USB connectivity

#### Analog I/O Board (Alternative)
- **High-Quality DAC**: Superior audio output
- **Multiple Video Outputs**: Better analog video quality
- **SNAC Support**: Direct controller connection to cores

## Architecture Overview

### FPGA Fabric
The FPGA implements the actual hardware logic of vintage systems:
- CPU cores (Z80, 68000, 6502, etc.)
- Graphics processors (VDP, PPU, custom chips)
- Sound chips (SN76489, YM2612, SID, etc.)
- Memory controllers
- Custom ASICs and glue logic

### HPS (Linux) Responsibilities
- **Core Management**: Loading .rbf (FPGA bitstream) files
- **File System Access**: Reading ROMs, disk images, save states
- **User Interface**: On-screen display (OSD) menu system
- **Input Handling**: USB keyboard, mouse, gamepad processing
- **Network Services**: Updates, file sharing, remote access
- **Video Processing**: Framebuffer composition, scaling

### Communication Between HPS and FPGA
- **Lightweight HPS-to-FPGA Bridge**: Control signals, configuration
- **HPS-to-FPGA Bridge**: Data transfer for ROMs and assets
- **FPGA-to-HPS Bridge**: Status information, save data
- **Shared SDRAM**: Both subsystems access common memory pool

## Video Processing Pipeline

### FPGA Video Generation
1. Core generates native resolution video (e.g., 256×224 for SNES)
2. Pixel data sent to video processing chain
3. Scanline generation, shadow masks, and effects applied
4. Signal converted to target output format

### Output Paths
- **Digital (HDMI)**: Direct digital output, supports various resolutions
- **Analog (VGA)**: 15 kHz to 31 kHz progressive scan
- **Component/S-Video/Composite**: Via I/O board DACs

### Scaling and Filtering
- **Integer Scaling**: Pixel-perfect multiples (2×, 3×, 4×, 5×)
- **Nearest Neighbor**: Sharp pixels, no filtering
- **Bilinear**: Smooth scaling
- **Scanline Filters**: Simulates CRT scanlines at various intensities
- **Shadow Mask Filters**: LCD grid, aperture grille, slot mask simulation

### Resolution Support
- **Native**: 240p, 480i/p, 720p, 1080p, 1200p, 1440p
- **Adaptive**: Adjusts to core requirements
- **VRR Support**: Variable refresh rate via HDMI (G-Sync/FreeSync compatible)

## Audio Processing

### FPGA Audio Generation
- Cores implement original sound chips in hardware
- Sample rates: Typically 48 kHz output
- Bit depth: 16-bit to 24-bit depending on core

### Audio Outputs
- **HDMI Audio**: Embedded in HDMI stream
- **Analog Audio**: Via I/O board (line-level output)
- **S/PDIF Optical**: Digital audio output
- **Bluetooth**: Possible via USB adapters

### Audio Features
- **Low Latency**: Hardware generation = minimal lag
- **Filter Options**: Various audio filters available per core
- **Volume Control**: Software-controlled attenuation

## Storage and File System

### Operating System
- **Linux Distribution**: Custom Arch Linux ARM build
- **Boot Process**: U-Boot bootloader → Linux kernel → MiSTer application
- **File System**: ext4 on microSD card

### Directory Structure
```
/media/fat/
├── MiSTer              # Main executable
├── config/             # Configuration files
├── games/              # ROM files organized by system
│   ├── NES/
│   ├── SNES/
│   ├── Genesis/
│   └── ...
├── saves/              # Save states and SRAM
├── Screenshots/        # Screen captures
├── wallpapers/         # Menu backgrounds
└── _Arcade/            # Arcade ROM sets and MRA files
```

### Core Files
- **.rbf**: FPGA bitstream (core implementation)
- **.mra**: Arcade ROM loading instructions
- **.rom**: System ROMs (BIOS files)
- **ROM files**: Game software (various formats)

## Input System

### Input Handling Flow
1. USB device connects to DE10-Nano USB ports
2. Linux kernel recognizes device
3. MiSTer framework maps inputs
4. Core receives input via standardized interface

### Supported Input Types
- **USB Keyboards**: Full keyboard support
- **USB Mice**: Analog and digital mice
- **USB Gamepads**: Modern controllers (Xbox, PlayStation, etc.)
- **Bluetooth Controllers**: Via USB Bluetooth adapters
- **SNAC (Serial Native Accessory Converter)**: Original controllers via User I/O port

### Input Lag
- **Typical Latency**: <1 frame (~16ms at 60Hz)
- **USB Polling**: 1000 Hz capable
- **SNAC**: Near-zero latency (direct FPGA connection)

## Network Capabilities

### Ethernet Connectivity
- **Update Mechanism**: Automated core and system updates
- **File Transfer**: SSH, FTP, Samba shares
- **Remote Access**: SSH for configuration
- **NTP Time Sync**: Network time synchronization

### Wi-Fi Support
- Via USB Wi-Fi adapters
- Less reliable than Ethernet for updates

## Power Requirements

### DE10-Nano Power
- **Input**: 5V DC, 2A minimum (via barrel jack)
- **Recommended**: 5V 4A for stability with peripherals
- **Connector**: 2.1mm center-positive barrel jack

### Power Considerations
- **USB Peripherals**: Draw from 5V rail
- **Cooling**: Active cooling recommended for sustained use
- **Power Supply Quality**: Clean power important for stability

## Cores and Core Development

### Core Types

#### Computer Cores
- **Examples**: Amiga, Commodore 64, Apple II, MSX, X68000
- **Features**: Full keyboard support, disk image mounting
- **Complexity**: Often require substantial FPGA resources

#### Console Cores
- **Examples**: NES, SNES, Genesis, TurboGrafx-16, PlayStation
- **Features**: Gamepad-centric, save state support
- **Accuracy**: Cycle-accurate reproduction of original hardware

#### Arcade Cores
- **Examples**: Individual arcade boards (CPS1, CPS2, Konami, etc.)
- **Loading**: MRA files define ROM loading
- **Variations**: Each arcade board is separate core

### Core Development Process
1. **HDL Coding**: Verilog or VHDL hardware description
2. **Quartus Compilation**: Intel Quartus Prime synthesis
3. **Testing**: Verification against original hardware
4. **Optimization**: Resource usage, timing closure
5. **Release**: .rbf file distributed to community

### Resource Constraints
- **Logic Elements**: 110K limit on Cyclone V
- **RAM**: ~5.5 Mbit embedded + 1GB DDR3
- **DSP Blocks**: Limited to 112
- **Timing**: Must meet FPGA timing constraints

## Technical Advantages

### Cycle Accuracy
- FPGA implements actual hardware circuits
- Original timing preserved perfectly
- No frame skipping or audio stuttering
- Edge cases handled correctly

### Low Latency
- No OS overhead for emulation
- Direct hardware implementation
- Minimal input-to-display lag
- Suitable for competitive gaming

### Flexibility
- Cores can be updated and improved
- New systems can be added
- Hardware features can be enhanced
- Community-driven development

### Authenticity
- Original video timings
- Native refresh rates (50Hz PAL, 60Hz NTSC, etc.)
- Original audio characteristics
- Proper color reproduction

## Performance Characteristics

### Frame Timing
- **Locked Synchronization**: Matches original hardware refresh rates
- **V-Sync Options**: Run-ahead, fast-forward support
- **Frame Counting**: Precise frame-by-frame operation

### Memory Bandwidth
- **DDR3 Speed**: 800 MHz (data rate 1600 MT/s)
- **Bandwidth**: ~6.4 GB/s theoretical
- **Latency**: Low-latency access from FPGA fabric

### Thermal Management
- **Passive**: Heatsinks on SoC and RAM
- **Active**: 5V fan recommended (40mm typical)
- **Temperature**: Monitor via Linux sensors
- **Throttling**: None if properly cooled

## Limitations

### FPGA Size
- Cannot implement very complex systems (e.g., PS2, GameCube)
- Some cores require resource optimization
- Trade-offs between features and available logic

### RAM Capacity
- 1GB shared between HPS and FPGA
- Limits size of framebuffer and ROM caching
- Some systems with large RAM requirements challenging

### No 3D Acceleration
- FPGA suited for 2D and simple 3D
- Complex 3D systems (N64, Saturn) extremely difficult
- Rasterization and texture mapping resource-intensive

### Development Complexity
- Requires HDL knowledge
- Quartus toolchain learning curve
- Hardware debugging challenges
- Timing closure can be difficult

## Technical Comparisons

### vs. Software Emulation
| Aspect | MiSTer FPGA | Software Emulation |
|--------|-------------|-------------------|
| Accuracy | Cycle-accurate | Varies, often HLE |
| Latency | <1 frame | 1-3+ frames |
| CPU Usage | None (HPS handles I/O only) | 100% of cores |
| Flexibility | Requires new core | Easy updates |
| Cost | ~$200-300 (clones: $100-200) | Free (needs capable PC) |
| Power Draw | ~10W | 50-500W (PC dependent) |

### vs. Original Hardware
| Aspect | MiSTer FPGA | Original Hardware |
|--------|-------------|-------------------|
| Compatibility | 99%+ for most cores | 100% |
| Maintenance | Solid-state, reliable | Aging capacitors, drives |
| Output Options | HDMI, VGA, component | RF, composite, RGB |
| Size | Compact | Multiple systems |
| Cost | Moderate initial | Varies widely |

### vs. Raspberry Pi / RetroPie
| Aspect | MiSTer FPGA (incl. clones) | Raspberry Pi / RetroPie |
|--------|----------------------------|------------------------|
| Technology | FPGA hardware recreation | Software emulation |
| Accuracy | Cycle-accurate | Good but not perfect |
| Latency | <1 frame | 2-4+ frames |
| Hardware | Cyclone V FPGA + ARM | ARM CPU only |
| Complexity | Moderate learning curve | User-friendly |
| Cost | $100-300 | $50-100 |
| Power | ~10W | ~5-15W |
| Game Support | Requires cores | Wide software support |

**Important**: Despite similar naming in clones, MiSTer Pi devices are **not** Raspberry Pi systems. They use completely different hardware (FPGA vs CPU-only) and technology approaches.

## Community and Development

### Open Source Nature
- **Main Repository**: GitHub (MiSTer-devel organization)
- **License**: GPL v3 and various open licenses
- **Contributors**: Global community of developers
- **Documentation**: Wiki, forums, Discord
- **Clone Compatibility**: Clone devices (MiSTer Pi, etc.) benefit from the same open-source cores and updates

### Update Mechanism
- **Update Script**: Automated update via network (works on clones)
- **Frequency**: Cores updated regularly
- **Channels**: Official, experimental, community cores
- **Rollback**: Previous versions accessible
- **Clone Notes**: Most clones use the same update infrastructure; verify compatibility with your specific hardware variant

## Advanced Features

### Save States
- **Full System State**: Complete memory and register capture
- **Slots**: Multiple save slots per game
- **Loading Time**: Instant
- **Storage**: On microSD card

### Cheats
- Support varies by core
- Codes applied via OSD menu
- Game Genie, Action Replay, raw memory patches

### Video Filters and Effects
- **CRT Simulation**: Shadow masks, bloom, curvature
- **Gamma Correction**: Adjustable per-core
- **Aspect Ratio**: Original, stretched, custom
- **Position/Cropping**: Fine-tune video output

### Debugging Features
- **Core-specific**: Some cores include debug modes
- **Signal Tap**: Logic analyzer (development)
- **OSD Overlay**: Real-time information display

## Future Potential

### Hardware Evolution
- Newer FPGA boards (larger FPGAs)
- More RAM capacity
- Faster processors
- Better video output options

### Core Development
- More complex systems
- Improved accuracy
- Additional features
- Better resource optimization

### Software Improvements
- Enhanced user interface
- Better file management
- Cloud integration
- Remote play capabilities

## Technical Resources

### Development Tools
- **Intel Quartus Prime**: FPGA synthesis (Lite edition free)
- **ModelSim**: Simulation and verification
- **Git**: Version control
- **Linux Cross-Compilation**: ARM toolchain

### Documentation
- MiSTer Wiki: Setup and usage guides
- MiSTer Forums: Community support
- GitHub: Source code and technical documentation
- Discord: Real-time developer discussion

### Testing and Validation
- **Test ROMs**: Hardware validation programs
- **Comparison Testing**: Against original hardware
- **Community Testing**: Beta releases
- **Automated Testing**: CI/CD pipelines for builds

## Conclusion

MiSTer FPGA represents a unique approach to vintage computing preservation, leveraging reconfigurable hardware to authentically recreate systems at the gate level. The Cyclone V FPGA (in DE10-Nano or compatible clones) provides sufficient resources for accurate implementations of 8-bit, 16-bit, and early 32-bit systems, while the ARM HPS manages the modern conveniences of file systems, networking, and user interfaces.

The project's open-source nature, active community, and continuous development ensure ongoing improvements and new core releases. Clone devices like MiSTer Pi make the platform more accessible at various price points, though quality and features vary by manufacturer. While limited by FPGA resources compared to modern software emulation, MiSTer excels in accuracy, latency, and authenticity—making it the preferred platform for enthusiasts seeking the most faithful reproduction of vintage gaming and computing experiences.

**For Clone Device Users**: Verify your specific hardware variant's specifications, as some clones may have different FPGA models, RAM configurations, or I/O capabilities. The core MiSTer experience and software compatibility typically remain consistent across properly designed clones.

---

*Last Updated: November 22, 2025*
