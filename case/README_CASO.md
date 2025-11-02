# Documentação do Caso OpenFOAM

## Descrição do Caso

Este caso representa uma simulação CFD 2D básica de escoamento incompressível turbulento utilizando o solver **simpleFoam** do OpenFOAM v13.

## Geometria

O domínio computacional é um retângulo 2D com dimensões:
- Comprimento (X): 1 m
- Altura (Y): 1 m  
- Espessura (Z): 0.1 m (direção vazia para simulação 2D)

## Malha

A malha é gerada usando o utilitário **blockMesh** com:
- 20 células na direção X
- 20 células na direção Y
- 1 célula na direção Z (2D)
- Total: 400 células

**Nota**: Esta é uma malha demonstrativa. Para simulações de produção, recomenda-se realizar estudos de refinamento de malha para garantir independência da solução. Malhas típicas podem ter 100×100 células ou mais, dependendo da complexidade do escoamento.

### Fronteiras

1. **inlet**: Face de entrada (x = 0)
2. **outlet**: Face de saída (x = 1)
3. **walls**: Paredes superior e inferior
4. **frontAndBack**: Faces frontal e traseira (tipo empty para 2D)

## Condições de Contorno

### Velocidade (U)
- **inlet**: Velocidade fixa de 1 m/s na direção X
- **outlet**: Gradiente zero (zeroGradient)
- **walls**: No-slip (noSlip)

### Pressão (p)
- **inlet**: Gradiente zero
- **outlet**: Pressão fixa em 0 Pa
- **walls**: Gradiente zero

### Turbulência (k, epsilon)
- **inlet**: Valores fixos calculados
  - k = 0.375 m²/s² (5% de intensidade turbulenta)
  - epsilon = 14.855 m²/s³
- **outlet**: Gradiente zero
- **walls**: Funções de parede (wall functions)

## Propriedades Físicas

### Fluido
- **Tipo**: Newtoniano (ar)
- **Viscosidade cinemática**: 1.5e-05 m²/s

### Turbulência
- **Modelo**: k-epsilon
- **Tipo de simulação**: RAS (Reynolds-Averaged Simulation)

## Esquemas Numéricos

### Temporal
- **Tipo**: Steady-state (estado estacionário)

### Gradientes
- **Esquema padrão**: Gauss linear

### Divergentes
- **div(phi,U)**: Gauss linearUpwind (segundo ordem, limitado)
- **div(phi,k)**: Gauss upwind
- **div(phi,epsilon)**: Gauss upwind

### Laplacianos
- **Esquema padrão**: Gauss linear corrected

## Solvers

### Pressão (p)
- **Solver**: GAMG (Geometric Algebraic MultiGrid)
- **Tolerância**: 1e-06
- **Tolerância relativa**: 0.1

### Velocidade (U)
- **Solver**: smoothSolver
- **Smoother**: GaussSeidel
- **Tolerância**: 1e-05

### Variáveis de turbulência (k, epsilon)
- **Solver**: smoothSolver
- **Smoother**: GaussSeidel
- **Tolerância**: 1e-05

## Algoritmo SIMPLE

- **Correções não-ortogonais**: 0
- **Consistente**: Sim

### Controle de Resíduos
- p: 1e-5
- U: 1e-5
- k, epsilon: 1e-5

### Fatores de Relaxação
- U: 0.9
- k: 0.7
- epsilon: 0.7

## Parâmetros de Controle

- **Tempo inicial**: 0
- **Tempo final**: 1000 iterações
- **Delta t**: 1
- **Intervalo de gravação**: 100 iterações
- **Formato de saída**: ASCII

## Como Executar

### Método 1: Scripts Automáticos

```bash
# Limpar caso anterior (se existir)
./Allclean

# Executar simulação completa
./Allrun
```

### Método 2: Comandos Manuais

```bash
# Gerar malha
blockMesh

# Executar solver
simpleFoam

# Pós-processamento (opcional)
paraFoam
```

## Resultados Esperados

Após a execução, serão criadas pastas com os resultados:
- `100/`: Resultados na iteração 100
- `200/`: Resultados na iteração 200
- ...
- `1000/`: Resultados finais na iteração 1000

## Modificações Sugeridas

Para adaptar este caso às suas necessidades:

1. **Geometria**: Modifique `system/blockMeshDict`
2. **Condições de contorno**: Edite os arquivos em `0/`
3. **Propriedades do fluido**: Ajuste `constant/transportProperties`
4. **Parâmetros de simulação**: Altere `system/controlDict`
5. **Esquemas numéricos**: Modifique `system/fvSchemes`
6. **Configuração dos solvers**: Ajuste `system/fvSolution`

## Validação

Para verificar a convergência:

```bash
# Extrair dados de log usando foamLog
foamLog log.simpleFoam

# Os arquivos de resíduos serão criados em postProcessing/logs/
# Visualizar resíduos com gnuplot (exemplo para pressão)
gnuplot -p -e "plot 'postProcessing/logs/0/residuals.dat' using 1:2 with lines title 'p residual'"
```

## Referências

- OpenFOAM v13 User Guide
- simpleFoam Solver Documentation
- k-epsilon Turbulence Model
