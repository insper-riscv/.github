# Insper RISC-V

🌐 [Português](https://github.com/insper-riscv/.github/blob/main/profile/README.md) · [English](https://github.com/insper-riscv/.github/blob/main/profile/README.en.md)

**Capstone project** of the Computer Engineering and Computer Science programs at [Insper](https://www.insper.edu.br), led by
professor **Rafael Corsi** ([@rafaelcorsi](https://github.com/rafaelcorsi),
[rafael.corsi@insper.edu.br](mailto:rafael.corsi@insper.edu.br)), built by a new group of students each semester. The project is funded by
[CTI Renato Archer](https://www.gov.br/cti/pt-br) (Centro de Tecnologia da Informação Renato Archer), the client of the Capstone.
The result is a RISC-V (RV32I + M) processor in VHDL with a 5-stage in-order pipeline: the core, its memories and
peripherals, the board platform (Cyclone V), and the tools, tests and certification around them.

Each repository has one responsibility. **Start at [RV32](https://github.com/insper-riscv/RV32)**:
it pins a version of the others and runs everything together.

```bash
git clone --recurse-submodules https://github.com/insper-riscv/RV32.git
cd RV32
make all        # checks, every repository's tests, and the 89-program simulation suite
```

## The repositories

| Repository | Responsibility | Depends on |
| :--- | :--- | :--- |
| [RV32](https://github.com/insper-riscv/RV32) | the parent: pins the versions of the set, KPI reports, integration CI | all, as submodules |
| [Core](https://github.com/insper-riscv/Core) | the processor in VHDL, organized by ISA extension (`common/`, `I/`, `M/`); profiles `rv32i` and `rv32im`; entity tests | nothing |
| [Memory](https://github.com/insper-riscv/Memory) | simulation models and the board's Quartus IPs | Core |
| [Peripherals](https://github.com/insper-riscv/Peripherals) | memory-mapped GPIO and TIMER (UART later) | Core |
| [TopLevel](https://github.com/insper-riscv/TopLevel) | the platforms: Quartus project, PLL, simulation top, runtime, memory map, testbench | Core, Memory, Peripherals, Tools |
| [Tests](https://github.com/insper-riscv/Tests) | the test programs (`asm/`, `c/`), goldens, and the flows that run them in simulation and on the board | TopLevel, Tools |
| [Certification](https://github.com/insper-riscv/Certification) | the official [riscv-arch-test](https://github.com/riscv/riscv-arch-test) (ACT4) suite against the core | Core, Memory, TopLevel, Tools |
| [Tools](https://github.com/insper-riscv/Tools) | `riscv-tools`: compile, simulate, run on the board over JTAG, certify, check | Infra, when running |
| [Infra](https://github.com/insper-riscv/Infra) | the machine: the toolchain image (RISC-V GCC with picolibc, Spike, GHDL, uv) every CI runs in, and the workstation guides | nothing |

```
RV32 (parent, pins the versions)
 ├─ Core ◄─ Memory, Peripherals ◄─ TopLevel ◄─ Tests
 │                                     ▲           ▲
 │                                     └─ Certification
 └─ Tools (inside each, as a submodule) ◄─ Infra (the image it runs in)
```

## The groups

Each group continued the work of the one before; the most recent comes first.

| Period | Group | What it did | Where it is |
| :--- | :--- | :--- | :--- |
| 2026.2 | [Artur Alvares Cruz](https://github.com/arturacruz), [Breno Schneider Salles de Oliveira](https://github.com/brnoschsaloli), [Lucas Hix](https://github.com/Peng1104) | the split of the project into repositories with one responsibility each, Tools, the test and CI infrastructure, the simulation of the board's top and the certification | the repositories on this page |
| 2026.1 | [Henrique Rocha Bomfim](https://github.com/HenriqueRBomfim), Pedro Carvalho Ribeiro Neto, [Luiz Felipe Borelli Durand](https://github.com/LuizDurand), [Luka Siqueira Ferreira de Figueiredo](https://github.com/lukafig) | the 5-stage pipeline with the M extension, the tests and the KPIs | [RV32](https://github.com/insper-riscv/RV32) and [Core](https://github.com/insper-riscv/Core) |
| 2025.2 | [Ilana Chaia Finger](https://github.com/ilacftemp), [Leonardo Merlin Paloschi](https://github.com/leonardopaloschi), [Lucas Lima](https://github.com/lucasouzamil), [Pedro Pereira Cecilio Ventura](https://github.com/pedropcventura) | L2IP (an RV32I), the development and the test infrastructure, where Infra, Tools and Tests came from | `archive/l2ip` in [RV32](https://github.com/insper-riscv/RV32), and [Infra](https://github.com/insper-riscv/Infra), [Tools](https://github.com/insper-riscv/Tools) and [Tests](https://github.com/insper-riscv/Tests) in their current form |
| 2025.1 | [Pedro Paulo Moreno Camargo](https://github.com/PedroPauloMorenoCamargo), [Pedro Balbo Portella](https://github.com/Vacbo) and [Pedro Cliquet do Amaral](https://github.com/pcliquet) | the SoC and the peripherals | `riscv-SoC` and `FOSS-peripherals` (below) |
| 2024.2 | Eduardo Schneider Monteiro de Barros (Computer Science), [Rodrigo Anciães Patelli](https://github.com/RodrigoAnciaes), Victor Luis Gama de Assis and [Arthur Martins de Souza Barreto](https://github.com/Arthur-Barreto) (Computer Engineering) | — | — |
| 2024.1 | [Luciano Felix](https://github.com/FelixLuciano), [Tiago Vitorino Seixas](https://github.com/TiagoSeixas2103), [Giancarlo Vanoni Ruggiero](https://github.com/gianvr) | the first core and the documentation | `core-old` and `docs` (below) |

## Where things are

- **How the pipeline works:** [Core's `docs/ARCHITECTURE.md`](https://github.com/insper-riscv/Core/blob/main/docs/ARCHITECTURE.md).
- **How to add an ISA extension, and the bus and memory interface:** [Core's `docs/contracts/`](https://github.com/insper-riscv/Core/tree/main/docs/contracts).
- **The memory map, the boot, and how a program runs on the board:** [TopLevel's `docs/`](https://github.com/insper-riscv/TopLevel/tree/main/docs).
- **How to write a test and run it:** [Tools' `docs/`](https://github.com/insper-riscv/Tools/tree/main/docs) and the [Tests README](https://github.com/insper-riscv/Tests).
- **Setting up a workstation and the CI runner:** [Infra](https://github.com/insper-riscv/Infra).

## Other repositories in the organization

[Diagram-Generator](https://github.com/insper-riscv/Diagram-Generator) is independent of the
structure above. `core-old`, `FOSS-peripherals`, `riscv-SoC`, `development-infrastructure` and
`docs` predate it and are not part of it; the state of the project before the split is the tag
`pre-refactor` in each repository above, and what was retired is kept as `archive/*` tags in
RV32.

Everything is licensed under the Apache License 2.0.
