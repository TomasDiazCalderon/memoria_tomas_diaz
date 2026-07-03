# Estrategias adaptativas de reinicio y clustering para Branch and Bound mediante Q-Learning

Este repositorio contiene la arquitectura de software, los scripts de entrenamiento, el análisis y el registro histórico de pruebas desarrollados en el marco de la Memoria de Título para optar al título de Ingeniero Civil en Informática y Telecomunicaciones en la Universidad Diego Portales (2026).

## Descripción general

Este proyecto aborda las limitaciones críticas en la resolución de problemas de optimización numérica con restricciones (NCOP) en el algoritmo Branch and Bound, las cuales corresponden a un crecimiento exponencial del árbol de búsqueda y el estancamiento en regiones infactibles. Este trabajo presenta una estrategia de reinicio dinámica que utiliza algoritmos de clustering para identificar las regiones más prometedoras y reanudar la búsqueda a partir de ellas, siendo gestionadas por un agente inteligente de aprendizaje por refuerzo (RL).



## Estructura del Repositorio

Al ingresar al repositorio, el proyecto se organiza en dos directorios principales que separan el núcleo experimental del conjunto de datos extendido:

```text
├── Pruebas de estados y entrenamiento/   # Repositorio central de experimentos y fases de aprendizaje
│   ├── Entrenamiento 1/                  # Fase E1: Pruebas de concepto inicial
│   ├── Entrenamiento 2/                  # Fase E2: Ajuste de umbrales cuantitativos individuales
│   ├── Entrenamiento 3/                  # Fase E3: Eliminación de benchmarks complejos
│   ├── Entrenamiento 4/                  # Fase E4: Reestructuración de rewards y penalización terminal
│   ├── Entrenamiento 5/                  # Fase E5: Implementación final de la política adaptativa Softmax
│   ├── Pruebas de estados/               # Pruebas individuales y analítica de distribución de visitas
│   └── Validación/                       # Resultados de la propuesta y comparación con el estado del arte
├── benchs/
│   └── optim/
│       └── benchs_victor_fixxed/         # Instancias NCOP personalizadas (Víctor Reyes Rodríguez)
├── src/                                  # Código fuente modificado para la integración en Ibex
│   └── optim/
│       ├── ibex_Optimizer.h
│       └── ibex_Optimizer.cpp
└── train_ql.sh                           # Script bash de automatización del entrenamiento global

```

## Instalación

Para compilar el proyecto, es necesario tener la librería Ibex instalada.

1. **Clonación de repositorio**: 
```text
   git clone https://github.com/TomasDiazCalderon/memoria_tomas_diaz.git
```
2. **Reemplazar archivos**: Copiar los ficheros de ```src/optim``` a la carpeta ```src/optim``` de Ibex. Insertar archivo ```train_ql.sh``` y ```q_table_trained.txt``` de ```Pruebas de estado y entrenamiento/Entrenamiento 5``` en la carpeta ```build/bin``` de Ibex.
3. **Compilación**: Siga las instrucciones de compilación de la biblioteca Ibex, un ejemplo de ejecución abajo.

```text
   ./ibexopt [ibex-lib-path]/benchs/optim/medium/himmel16.bch 
```

## Uso

El proyecto cuenta con dos modos principales de ejecución que modifican el comportamiento de la política Softmax del agente. Esto se controla mediante un flag booleano en el archivo de configuración del solver. Antes de compilar, abre el archivo `src/optim/ibex_Optimizer.h` y modifica la línea correspondiente según el modo deseado:
```cpp
// Ubicación: src/optim/ibex_Optimizer.h
bool exploitation_mode = true; // TRUE para Validación, FALSE para Entrenamiento
```

### 1. Entrenamiento (Exploración)

En este modo, el agente interactúa activamente con el entorno para poblar la tabla Q, utilizando la temperatura $\tau$ dinámica para permitir una mayor exploración en el árbol de búsqueda.

1. Comienza cambiando el flag a ```false``` en ```ibex_Optimizer.h```
```cpp
// Ubicación: src/optim/ibex_Optimizer.h
bool exploitation_mode = false; 
```
2. Recompila la librería Ibex para aplicar el cambio (escribir ```make``` dentro de ```build```.
3. Asegúrate de que el script ```train_ql.sh``` tenga permisos de ejecución y ejecútalo, lo que dará inicio al entrenamiento de los problemas en 5 semillas distintas.
   ```cpp
   ./train_ql.sh
   ```

De esta forma, se irá actualizando dinámicamente la matriz en ```q_table_trained.txt``` y registrará el decaimiento de temperatura en ```tau_state.txt```. Si alguno de estos dos archivos mencionados no existe, se crearán desde cero.

### 2. Validación 

Para evaluar el rendimiento final de la tabla utilizando el conocimiento acumulado:

1. Cambia el flag a ```true``` en ```ibex_Optimizer.h```:
```cpp
bool exploitation_mode = true;
```
2. Recompila la librería ibex.
3. Ejecuta el optimizador sobre cualquier benchmark del conjunto:
   ```text
   ./ibexopt [ibex-lib-path]/benchs/optim/benchs_victor_fixxed/avion2
   ```

En este modo, el agente actuará de manera determinista, seleccionando siempre la acción con un mejor valor en el estado en que se encuentre.
