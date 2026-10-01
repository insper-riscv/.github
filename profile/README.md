# Insper RISC-V

🌐 [Português](https://github.com/insper-riscv/.github/blob/main/profile/README.md) · [English](https://github.com/insper-riscv/.github/blob/main/profile/README.en.md)

**Projeto Capstone** dos cursos de Engenharia de Computação e Ciência da Computação do [Insper](https://www.insper.edu.br), liderado pelo
professor **Rafael Corsi** ([@rafaelcorsi](https://github.com/rafaelcorsi),
[rafael.corsi@insper.edu.br](mailto:rafael.corsi@insper.edu.br)), desenvolvido por grupos de alunos a cada semestre. O projeto é financiado pelo
[CTI Renato Archer](https://www.gov.br/cti/pt-br) (Centro de Tecnologia da Informação Renato Archer), que é o cliente do Capstone.
O resultado é um processador RISC-V (RV32I + M) em VHDL, com pipeline de 5 estágios em ordem: o core, suas
memórias e periféricos, a plataforma da placa (Cyclone V), e as ferramentas, os testes e a certificação em volta.

Cada repositório cuida de uma coisa. **Comece pelo [RV32](https://github.com/insper-riscv/RV32)**:
ele fixa uma versão dos outros e roda tudo junto.

```bash
git clone --recurse-submodules https://github.com/insper-riscv/RV32.git
cd RV32
make all        # verificações, os testes de cada repositório e a suíte de simulação de 89 programas
```

## Os repositórios

| Repositório | Responsabilidade | Depende de |
| :--- | :--- | :--- |
| [RV32](https://github.com/insper-riscv/RV32) | o pai: fixa as versões do conjunto, relatórios de KPI, CI de integração | todos, como submódulos |
| [Core](https://github.com/insper-riscv/Core) | o processador em VHDL, organizado por extensão do ISA (`common/`, `I/`, `M/`); perfis `rv32i` e `rv32im`; testes por entidade | nada |
| [Memory](https://github.com/insper-riscv/Memory) | modelos de simulação e as IPs do Quartus da placa | Core |
| [Peripherals](https://github.com/insper-riscv/Peripherals) | GPIO e TIMER mapeados em memória (UART depois) | Core |
| [TopLevel](https://github.com/insper-riscv/TopLevel) | as plataformas: projeto do Quartus, PLL, topo de simulação, runtime, mapa de memória, testbench | Core, Memory, Peripherals, Tools |
| [Tests](https://github.com/insper-riscv/Tests) | os programas de teste (`asm/`, `c/`), os goldens e os fluxos que os rodam em simulação e na placa | TopLevel, Tools |
| [Certification](https://github.com/insper-riscv/Certification) | a suíte oficial [riscv-arch-test](https://github.com/riscv/riscv-arch-test) (ACT4) contra o core | Core, Memory, TopLevel, Tools |
| [Tools](https://github.com/insper-riscv/Tools) | `riscv-tools`: compilar, simular, rodar na placa por JTAG, certificar, verificar | Infra, ao executar |
| [Infra](https://github.com/insper-riscv/Infra) | a máquina: a imagem de toolchain (GCC RISC-V com picolibc, Spike, GHDL, uv) em que todo CI roda, e os guias da workstation | nada |

```
RV32 (o pai, fixa as versões)
 ├─ Core ◄─ Memory, Peripherals ◄─ TopLevel ◄─ Tests
 │                                     ▲           ▲
 │                                     └─ Certification
 └─ Tools (dentro de cada um, como submódulo) ◄─ Infra (a imagem em que roda)
```

## Os grupos

Cada grupo continuou o trabalho do anterior; o mais recente está primeiro.

| Período | Grupo | O que fez | Onde está |
| :--- | :--- | :--- | :--- |
| 2026.2 | [Artur Alvares Cruz](https://github.com/arturacruz), [Breno Schneider Salles de Oliveira](https://github.com/brnoschsaloli), [Lucas Hix](https://github.com/Peng1104) | A divisão do projeto em repositórios com uma responsabilidade cada, o Tools, a infraestrutura de testes e de CI, a simulação do topo da placa e a certificação | os repositórios desta página |
| 2026.1 | [Henrique Rocha Bomfim](https://github.com/HenriqueRBomfim), Pedro Carvalho Ribeiro Neto, [Luiz Felipe Borelli Durand](https://github.com/LuizDurand), [Luka Siqueira Ferreira de Figueiredo](https://github.com/lukafig) | O pipeline de 5 estágios com a extensão M, os testes e os KPIs | [RV32](https://github.com/insper-riscv/RV32) e [Core](https://github.com/insper-riscv/Core) |
| 2025.2 | [Ilana Chaia Finger](https://github.com/ilacftemp), [Leonardo Merlin Paloschi](https://github.com/leonardopaloschi), [Lucas Lima](https://github.com/lucasouzamil), [Pedro Pereira Cecilio Ventura](https://github.com/pedropcventura) | o L2IP (um RV32I), a infraestrutura de desenvolvimento e a de testes, de onde saíram o Infra, o Tools e o Tests | `archive/l2ip` no [RV32](https://github.com/insper-riscv/RV32), e [Infra](https://github.com/insper-riscv/Infra), [Tools](https://github.com/insper-riscv/Tools) e [Tests](https://github.com/insper-riscv/Tests) em sua forma atual |
| 2025.1 | [Pedro Paulo Moreno Camargo](https://github.com/PedroPauloMorenoCamargo) e [Pedro Balbo Portella](https://github.com/Vacbo) | o SoC e os periféricos | `riscv-SoC` e `FOSS-peripherals` (abaixo) |
| 2024.2 | Eduardo Schneider Monteiro de Barros, [Rodrigo Anciães Patelli](https://github.com/RodrigoAnciaes), Victor Luis Gama de Assis e [Arthur Martins de Souza Barreto](https://github.com/Arthur-Barreto) | — | — |
| 2024.1 | [Luciano Felix](https://github.com/FelixLuciano), [Tiago Vitorino Seixas](https://github.com/TiagoSeixas2103), [Giancarlo Vanoni Ruggiero](https://github.com/gianvr) | o primeiro core e a documentação | `core-old` e `docs` (abaixo) |

## Onde está cada coisa

- **Como o pipeline funciona:** [`docs/ARCHITECTURE.md` do Core](https://github.com/insper-riscv/Core/blob/main/docs/ARCHITECTURE.md).
- **Como acrescentar uma extensão do ISA, e a interface de barramento e de memória:** [`docs/contracts/` do Core](https://github.com/insper-riscv/Core/tree/main/docs/contracts).
- **O mapa de memória, o boot e como um programa roda na placa:** [`docs/` do TopLevel](https://github.com/insper-riscv/TopLevel/tree/main/docs).
- **Como escrever um teste e rodá-lo:** [`docs/` do Tools](https://github.com/insper-riscv/Tools/tree/main/docs) e o [README do Tests](https://github.com/insper-riscv/Tests).
- **Montar uma workstation e o runner do CI:** [Infra](https://github.com/insper-riscv/Infra).

## Outros repositórios da organização

O [Diagram-Generator](https://github.com/insper-riscv/Diagram-Generator) é independente da
estrutura acima. `core-old`, `FOSS-peripherals`, `riscv-SoC`, `development-infrastructure` e
`docs` são anteriores a ela e não fazem parte dela; o estado do projeto antes da divisão é a tag
`pre-refactor` em cada repositório acima, e o que foi aposentado fica nas tags `archive/*` do
RV32.

Tudo é licenciado sob a Apache License 2.0.
