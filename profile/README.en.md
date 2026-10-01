# Insper RISC-V

🌐 [Português](https://github.com/insper-riscv/.github/blob/main/profile/README.md) · [English](https://github.com/insper-riscv/.github/blob/main/profile/README.en.md)

A RISC-V (RV32I + M) processor in VHDL with a 5-stage in-order pipeline, built at
[Insper](https://www.insper.edu.br) as a capstone project: the core, its memories and
peripherals, the board platform (Cyclone V), and the tools, tests and certification
around them.

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
