# ESH Roadmap Advanced

[![Board: NUCLEO-C031C6](https://img.shields.io/badge/Board-NUCLEO--C031C6-green)](https://www.st.com/en/evaluation-tools/nucleo-c031c6.html)
[![IDE: VS Code](https://img.shields.io/badge/IDE-VS%20Code-blue)](https://code.visualstudio.com/)
[![Build: Make](https://img.shields.io/badge/Build-Make-orange)](https://www.gnu.org/software/make/)

An advanced, project-based learning roadmap for embedded systems development on the STM32C031C6T6 (Cortex-M0+). From here on, the focus shifts to design decisions, scalability, and long-term maintainability. You'll stop asking *"How do I make this peripheral work?"* and start asking *"How should this system be structured so it can grow, be reused, and be maintained?"*

## Table of Contents

- [ESH Roadmap Advanced](#esh-roadmap-advanced)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Hardware](#hardware)
  - [Prerequisites](#prerequisites)
  - [Repository Layout](#repository-layout)
  - [Projects](#projects)
  - [Getting Started](#getting-started)
  - [Building \& Flashing](#building--flashing)
    - [Build](#build)
    - [Flash \& Debug](#flash--debug)
  - [Documentation \& Resources](#documentation--resources)
  - [Contributing](#contributing)
  - [License](#license)

## Overview

This repository is the continuation of [ESH Roadmap Basic](https://github.com/Padifu03/ESH_roadmap_basic). While the basic roadmap focused on understanding individual peripherals through register-level access, this advanced roadmap tackles the engineering challenges that come after: system architecture, build infrastructure, reproducible environments, and scalable firmware design.

Key learning objectives:

- Containerized, reproducible build environments with Docker
- Firmware architecture patterns (layered drivers, HAL abstractions, event-driven design)
- Testing strategies for embedded software
- CI/CD pipelines for firmware projects

## Hardware

| | |
|---|---|
| **MCU** | STM32C031C6T6 — ARM Cortex-M0+, 48 MHz, 32 KB Flash, 12 KB SRAM |
| **Board** | NUCLEO-C031C6 |
| **Debugger** | On-board ST-LINK/V2-1 (SWD) |
| **User LED** | LD4 — PA5 |
| **User Button** | B1 — PC13 (active low) |

## Prerequisites

| Tool | Version | Purpose |
|---|---|---|
| Visual Studio Code | Latest | Primary IDE |
| Docker | Latest | Reproducible build environment |
| GNU ARM Embedded Toolchain | Latest | `arm-none-eabi-gcc` cross-compiler |
| Make | Latest | Build automation |
| OpenOCD | Latest | On-chip programming & debugging |

## Repository Layout

```text
ESH_roadmap_advanced/
├── adv-00-docker/              # Dockerized bare-metal blinky (Makefile build)
├── STM32C031C6/                # Datasheets & reference manuals
├── LICENSE
└── README.md
```

Each project follows a consistent internal structure:

```text
adv-XX-<topic>/
├── src/                        # Source files (.c)
├── inc/                        # Header files (.h)
├── startup_stm32c031xx.S       # Vector table & startup code
├── stm32c031x6_flash.ld        # Linker script
├── Makefile                    # Build configuration
└── build/                      # Build output (generated)
```

## Projects

| # | Folder | Topic | Description |
|---|---|---|---|
| 00 | `adv-00-docker` | Docker Build Environment | Bare-metal blinky project built with a Makefile inside a Docker container. Demonstrates reproducible, host-independent firmware builds. |

## Getting Started

1. **Clone the repository**

   ```bash
   git clone https://github.com/Padifu03/ESH_roadmap_advanced.git
   cd ESH_roadmap_advanced
   ```

2. **Open in VS Code**

   ```bash
   code .
   ```

3. **Select a project** — open the desired folder (e.g. `adv-00-docker`).

## Building & Flashing

### Build

From the project directory:

```bash
cd adv-00-docker
make all
```

To clean build artifacts:

```bash
make clean
```

### Flash & Debug

Connect the NUCLEO-C031C6 board via USB, then:

```bash
make flash
```

This uses OpenOCD with the ST-LINK interface to program, verify, and reset the target.

## Documentation & Resources

| Document | File |
|---|---|
| STM32C031C6 Reference Manual | `stm32c031c6_RM.pdf` |
| STM32C031C6 Datasheet | `stm32c031c6_datasheet.pdf` |
| STM32C031C6 User Manual | `stm32c031c6_user_manual.pdf` |
| NUCLEO-64 Board User Manual | `stm32_nucleo64_UM.pdf` |
| Cortex-M0+ Generic User Guide | `Cortex-M0plus_UG.pdf` |
| ESH Roadmap Basic (prerequisite) | [GitHub](https://github.com/Padifu03/ESH_roadmap_basic) |

## Contributing

Contributions, suggestions, and bug reports are welcome. To contribute:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feat/my-feature`).
3. Commit your changes with clear, descriptive messages.
4. Open a Pull Request against `master`.

Please keep the existing project structure and coding style consistent.

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
