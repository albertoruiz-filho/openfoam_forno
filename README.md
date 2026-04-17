# openfoam_forno

Repositório com os arquivos de configuração, malha e condições iniciais de uma simulação CFD de escoamento interno em um **forno industrial**, utilizando o [OpenFOAM v13](https://openfoam.org/).

---

## Descrição do Projeto

O caso simula o escoamento de ar compressível dentro de um forno com geometria composta por **dedos** (*fingers*), **câmara plenum** e corpo principal do forno. O objetivo é analisar o campo de velocidade, pressão e temperatura dentro do equipamento sob condições turbulentas.

A simulação emprega o método LES (*Large Eddy Simulation*) com o modelo de turbulência `kEqn` para resolução das estruturas de grande escala do escoamento turbulento.

---

## Estrutura do Repositório

```
.
├── 0/                        # Condições iniciais e de contorno
│   ├── T                     # Temperatura [K]
│   ├── U                     # Velocidade [m/s]
│   ├── p                     # Pressão [Pa]
│   ├── k                     # Energia cinética turbulenta
│   ├── alphat                # Difusividade térmica turbulenta
│   ├── muTilda               # Viscosidade modificada (Spalart-Allmaras)
│   └── nut                   # Viscosidade turbulenta cinemática
│
├── constant/                 # Propriedades físicas e malha computacional
│   ├── physicalProperties    # Propriedades termodinâmicas do ar (gás perfeito)
│   ├── momentumTransport     # Configuração do modelo de turbulência (LES kEqn)
│   └── polyMesh/             # Malha gerada no HyperMesh 2022.2.0.27
│
├── system/                   # Parâmetros numéricos e de controle
│   ├── controlDict           # Controle de tempo e saída de dados
│   ├── fvSchemes             # Esquemas de discretização
│   ├── fvSolution            # Solvers lineares e algoritmo PIMPLE
│   ├── fvConstraints         # Restrições do campo
│   └── functions             # Pós-processamento (médias de campo)
│
└── forno_of_R0V1.foam        # Arquivo de caso para visualização no ParaView
```

---

## Configuração da Simulação

### Geometria e Condições de Contorno

| Patch          | Tipo    | Descrição                          |
|----------------|---------|------------------------------------|
| `inlet`        | patch   | Entrada de ar                      |
| `outlet`       | patch   | Saída de ar                        |
| `wall_fingers` | wall    | Paredes dos dedos do forno         |
| `wall_plenum`  | wall    | Paredes da câmara plenum           |
| `wall_oven`    | wall    | Paredes externas do forno          |

### Condições Iniciais

| Campo | Valor Inicial | Descrição |
|-------|--------------|-----------|
| `U`   | 10 m/s (entrada turbulenta, x) | Velocidade |
| `T`   | 300 K        | Temperatura |
| `p`   | 1×10⁵ Pa     | Pressão     |

### Modelo de Fluido

- **Fluido:** Ar (gás perfeito)
- **Peso molecular:** 28,9 g/mol
- **Calor específico (Cv):** 712 J/(kg·K)
- **Viscosidade dinâmica:** 1,8×10⁻⁵ Pa·s
- **Número de Prandtl:** 0,7

### Parâmetros de Simulação

| Parâmetro         | Valor       |
|-------------------|-------------|
| Solver            | `fluid` (compressível) |
| Turbulência       | LES – `kEqn` |
| Algoritmo         | PIMPLE      |
| Passo de tempo    | 1×10⁻⁵ s   |
| Tempo final       | 0,3 s       |
| Intervalo de escrita | 100 passos (a cada 1×10⁻³ s) |
| Esquema temporal  | `backward` (2ª ordem) |

### Pós-processamento

Médias temporais de `U` e `p` são calculadas automaticamente via `fieldAverage`, incluindo valores RMS (`prime2Mean`).

---

## Pré-requisitos

- [OpenFOAM v13](https://openfoam.org/download/)
- [ParaView](https://www.paraview.org/) (para visualização, usando o arquivo `.foam`)

---

## Como Executar

```bash
# Executar a simulação
fluid

# Visualizar no ParaView
paraview forno_of_R0V1.foam
```

---

## Licença

Este projeto está licenciado sob os termos da licença disponível no arquivo [LICENSE](LICENSE).
