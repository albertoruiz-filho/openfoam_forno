# OpenFOAM Forno

Repositório com cartas, parâmetros de simulação e malhas do projeto no formato OpenFOAM v13 para compartilhamento e desenvolvimento colaborativo.

## Descrição

Este repositório contém casos de simulação CFD (Computational Fluid Dynamics) utilizando OpenFOAM versão 13. O projeto é estruturado para facilitar o compartilhamento e desenvolvimento colaborativo de simulações.

## Estrutura do Projeto

```
case/
├── 0/              # Condições iniciais e de contorno
│   ├── U           # Campo de velocidade
│   ├── p           # Campo de pressão
│   ├── k           # Energia cinética turbulenta
│   ├── epsilon     # Taxa de dissipação turbulenta
│   └── nut         # Viscosidade turbulenta
├── constant/       # Propriedades físicas e malha
│   ├── transportProperties       # Propriedades de transporte
│   └── turbulenceProperties      # Propriedades de turbulência
└── system/         # Parâmetros de controle e esquemas numéricos
    ├── controlDict     # Controle da simulação
    ├── fvSchemes       # Esquemas de discretização
    ├── fvSolution      # Parâmetros dos solvers
    └── blockMeshDict   # Definição da malha
```

## Pré-requisitos

- OpenFOAM v13 instalado
- Sistema operacional: Linux (Ubuntu 22.04 ou superior recomendado)

## Como Usar

### 1. Clonar o Repositório

```bash
git clone https://github.com/albertoruiz-filho/openfoam_forno.git
cd openfoam_forno
```

### 2. Gerar a Malha

```bash
cd case
blockMesh
```

### 3. Executar a Simulação

```bash
simpleFoam
```

### 4. Visualizar Resultados

Para visualizar os resultados no ParaView:

```bash
paraFoam
```

Ou crie um arquivo vazio `.foam` para abrir diretamente:

```bash
touch case.foam
paraview case.foam
```

## Configuração da Simulação

### Solver
- **Aplicação**: simpleFoam (solver steady-state para escoamentos incompressíveis turbulentos)

### Modelo de Turbulência
- **Tipo**: RAS (Reynolds-Averaged Simulation)
- **Modelo**: k-epsilon

### Propriedades do Fluido
- **Viscosidade cinemática**: 1.5e-05 m²/s (aproximadamente ar a 20°C)

### Condições de Contorno
- **Inlet**: Velocidade fixa (1 m/s na direção x)
- **Outlet**: Pressão fixa (0 Pa)
- **Walls**: Condição no-slip
- **FrontAndBack**: Empty (caso 2D)

### Parâmetros de Controle
- **Tempo inicial**: 0
- **Tempo final**: 1000 iterações
- **Intervalo de escrita**: 100 iterações

## Contribuindo

Contribuições são bem-vindas! Para contribuir:

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## Licença

Este projeto está sob a licença especificada no arquivo LICENSE.

## Contato

Para questões ou sugestões, por favor abra uma issue no GitHub.

## Referências

- [Documentação Oficial do OpenFOAM](https://www.openfoam.com/)
- [OpenFOAM User Guide](https://www.openfoam.com/documentation/user-guide)
- [OpenFOAM Wiki](https://openfoamwiki.net/)
