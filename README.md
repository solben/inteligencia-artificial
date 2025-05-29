# Inteligencia Artificial - Problema del Viajante (TSP)

Este repositorio contiene implementaciones y análisis del **Problema del Viajante** (Traveling Salesman Problem - TSP), uno de los problemas más famosos y estudiados en ciencias de la computación, matemáticas aplicadas e inteligencia artificial.

## 📋 Tabla de Contenidos

- [¿Qué es el Problema del Viajante?](#qué-es-el-problema-del-viajante)
- [Importancia y Aplicaciones](#importancia-y-aplicaciones)
- [Complejidad Computacional](#complejidad-computacional)
- [Enfoques de Solución](#enfoques-de-solución)
- [Datasets](#datasets)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Instalación y Uso](#instalación-y-uso)
- [Resultados y Análisis](#resultados-y-análisis)
- [Contribuciones](#contribuciones)

## 🎯 ¿Qué es el Problema del Viajante?

El **Problema del Viajante** es un problema de optimización combinatoria que busca encontrar la ruta más corta que:

1. **Visite cada ciudad exactamente una vez**
2. **Regrese al punto de partida**
3. **Minimice la distancia total recorrida**

### Definición Formal

Dado un conjunto de `n` ciudades y las distancias entre cada par de ciudades, encontrar el ciclo hamiltoniano de peso mínimo que visite todas las ciudades exactamente una vez.

Matemáticamente, se puede expresar como:

```
Minimizar: Σ(i=0 to n-1) d(c[i], c[i+1])
Sujeto a: cada ciudad se visita exactamente una vez
Donde: c[i] representa la ciudad en la posición i del recorrido
       d(a,b) es la distancia entre las ciudades a y b
```

## 🌍 Importancia y Aplicaciones

El TSP no es solo un problema académico, tiene aplicaciones prácticas en múltiples dominios:

### Logística y Transporte
- **Optimización de rutas de entrega**: Empresas como UPS, FedEx, Amazon
- **Planificación de rutas de autobuses urbanos**
- **Optimización de rutas aéreas**
- **Gestión de flotas de vehículos**

### Manufactura
- **Perforación de circuitos impresos**: Minimizar el tiempo de movimiento del taladro
- **Corte por láser**: Optimizar trayectorias de corte
- **Soldadura robotizada**: Planificar secuencias eficientes

### Bioinformática
- **Secuenciación de ADN**: Ordenamiento óptimo de fragmentos
- **Análisis filogenético**: Construcción de árboles evolutivos

### Astronomía
- **Planificación de observaciones telescópicas**
- **Optimización de trayectorias espaciales**

## ⚡ Complejidad Computacional

El TSP pertenece a la clase de problemas **NP-Difícil**, lo que significa:

- **No se conoce algoritmo polinomial** para resolverlo exactamente
- El número de posibles rutas crece **factorialmente**: `(n-1)!/2` para n ciudades
- Para ciudades pequeñas es manejable, pero escala rápidamente:
  - 10 ciudades: 181,440 rutas posibles
  - 15 ciudades: 43,589,145,600 rutas
  - 20 ciudades: 60,822,550,204,416,000 rutas

### Crecimiento de la Complejidad

| Ciudades | Rutas Posibles | Tiempo Estimado (1 ruta/μs) |
|----------|----------------|------------------------------|
| 5        | 12             | Instantáneo                  |
| 10       | 181,440        | 0.18 segundos               |
| 15       | 43 millones    | 43 segundos                 |
| 20       | 60 billones    | 19 horas                    |
| 25       | 310 trillones  | 9.8 años                    |

## 🔧 Enfoques de Solución

### 1. Algoritmos Exactos
- **Fuerza Bruta**: Evalúa todas las posibles rutas
- **Programación Dinámica** (Held-Karp): O(n²2ⁿ)
- **Branch and Bound**: Poda el espacio de búsqueda

### 2. Heurísticas Constructivas
- **Vecino más Cercano**: Greedy, rápido pero no óptimo
- **Inserción más Barata**: Construye gradualmente la ruta
- **Árbol de Expansión Mínima**: Aproximación usando MST

### 3. Metaheurísticas
- **Algoritmos Genéticos**: Evolución de poblaciones de soluciones
- **Recocido Simulado**: Inspirado en el enfriamiento de metales
- **Optimización por Colonia de Hormigas**: Basado en comportamiento animal
- **Búsqueda Tabú**: Evita ciclos en la búsqueda local
- **Búsqueda Local**: 2-opt, 3-opt, Lin-Kernighan

### 4. Algoritmos de Aproximación
- **Algoritmo de Christofides**: Garantiza solución ≤ 1.5 × óptimo
- **Heurística del Doble Árbol**: Aproximación 2-opt

## 📊 Datasets

### Ciudades de Madrid (`ciudades_madrid.csv`)
Este dataset contiene información sobre ciudades y municipios de la Comunidad de Madrid:

- **Formato**: CSV con coordenadas geográficas
- **Tamaño**: Múltiples localidades
- **Uso**: Ideal para probar algoritmos con datos reales
- **Distancias**: Calculadas usando distancia euclidiana o haversine

### Estructura del Dataset
```csv
Ciudad,Latitud,Longitud
Madrid,40.4168,-3.7038
Alcalá de Henares,40.4818,-3.3639
...
```

## 📁 Estructura del Proyecto

```
inteligencia-artificial/
├── README.md                    # Este archivo
├── ciudades_madrid.csv         # Dataset de ciudades de Madrid
├── src/                        # Código fuente (próximamente)
│   ├── algorithms/             # Implementaciones de algoritmos
│   │   ├── exact/             # Algoritmos exactos
│   │   ├── heuristics/        # Heurísticas constructivas
│   │   └── metaheuristics/    # Metaheurísticas
│   ├── utils/                 # Utilidades y helpers
│   └── visualization/         # Herramientas de visualización
├── examples/                   # Ejemplos de uso
├── tests/                     # Pruebas unitarias
├── results/                   # Resultados de experimentos
└── docs/                      # Documentación adicional
```

## 🚀 Instalación y Uso

### Prerrequisitos
```bash
python >= 3.8
numpy
pandas
matplotlib
scipy
```

### Instalación
```bash
git clone https://github.com/solben/inteligencia-artificial.git
cd inteligencia-artificial
pip install -r requirements.txt
```

### Uso Básico
```python
from src.algorithms.heuristics import nearest_neighbor
from src.utils.data_loader import load_cities

# Cargar datos
cities = load_cities('ciudades_madrid.csv')

# Resolver usando vecino más cercano
route, distance = nearest_neighbor(cities)

print(f"Ruta encontrada: {route}")
print(f"Distancia total: {distance:.2f} km")
```

## 📈 Resultados y Análisis

### Comparación de Algoritmos

| Algoritmo | Tiempo | Calidad | Escalabilidad | Uso Recomendado |
|-----------|--------|---------|---------------|-----------------|
| Fuerza Bruta | O(n!) | Óptimo | Muy Baja | n ≤ 10 |
| Held-Karp | O(n²2ⁿ) | Óptimo | Baja | n ≤ 20 |
| Vecino Cercano | O(n²) | ~25% error | Excelente | Solución rápida |
| 2-opt | O(n²) | ~5% error | Buena | Mejora local |
| Genético | O(generaciones) | ~3% error | Buena | Solución robusta |

### Benchmarks
*(Los resultados específicos se actualizarán conforme se implementen los algoritmos)*

## 🎓 Contexto Académico

Este proyecto forma parte del estudio de:
- **Optimización Combinatoria**
- **Algoritmos de Aproximación**
- **Inteligencia Artificial**
- **Metaheurísticas**
- **Análisis de Complejidad**

### Referencias Clave
- Applegate, D. et al. "The Traveling Salesman Problem: A Computational Study"
- Gutin, G. & Punnen, A.P. "The Traveling Salesman Problem and Its Variations"
- Lawler, E.L. et al. "The Traveling Salesman Problem"

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -am 'Agrega nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

### Áreas de Contribución
- Implementación de nuevos algoritmos
- Optimización de algoritmos existentes
- Nuevos datasets
- Visualizaciones
- Documentación
- Tests unitarios
- Análisis comparativos

## 📚 Recursos Adicionales

- [TSP FAQ - Universidad de Waterloo](http://www.math.uwaterloo.ca/tsp/index.html)
- [TSPLIB - Biblioteca de Instancias TSP](http://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/)
- [Concorde TSP Solver](http://www.math.uwaterloo.ca/tsp/concorde.html)

---

**Autor**: [solben](https://github.com/solben)  
**Licencia**: MIT  
**Última Actualización**: Mayo 2025

*"El Problema del Viajante: donde la simplicidad del enunciado contrasta con la complejidad de la solución"*