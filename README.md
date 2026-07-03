# Estrategias adaptativas de reinicio y clustering para Branch and Bound mediante Q-Learning

Este repositorio contiene la arquitectura de software, los scripts de entrenamiento, el análisis y el registro histórico de pruebas desarrollados en el marco de la Memoria de Título para optar al título de Ingeniero Civil en Informática y Telecomunicaciones en la Universidad Diego Portales (2026).

## Estructura del Repositorio

Al ingresar al repositorio, el proyecto se organiza en dos directorios principales que separan el núcleo experimental del conjunto de datos extendido:

```text
├── Pruebas de estados y entrenamiento/    # Repositorio central de experimentos y fases de aprendizaje
│   ├── Entrenamiento 1/                  # Fase E1: Pruebas de concepto inicial
│   ├── Entrenamiento 2/                  # Fase E2: Ajuste de umbrales cuantitativos individuales
│   ├── Entrenamiento 3/                  # Fase E3: Eliminación de benchmarks complejos
│   ├── Entrenamiento 4/                  # Fase E4: Reestructuración de recompensas y penalización terminal
│   ├── Entrenamiento 5/                  # Fase E5: Implementación final de la política adaptativa Softmax
│   ├── Pruebas de estados/               # Pruebas individuales de estados para encontrar la configuración óptima
│   └── Validación/                       # Resultados de la propuesta y comparación con el estado del arte
└── benchs/
    └── optim/
        └── benchs_victor_fixxed/         # Instancias NCOP personalizadas independientes de la instalación base de Ibex
