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
├── README.md                   # Documentación del proyecto
└── ciudades_madrid.csv        # Dataset de ciudades de Madrid
```

> **Nota**: Este repositorio actualmente contiene la documentación y dataset base. Las implementaciones de algoritmos y análisis pueden ser desarrolladas como extensiones futuras del proyecto.

## 🚀 Instalación y Uso

### Para trabajar con el dataset

```python
import pandas as pd
import numpy as np
from math import radians, cos, sin, asin, sqrt

# Cargar el dataset
df = pd.read_csv('ciudades_madrid.csv')
print(f"Dataset contiene {len(df)} ciudades")
print(df.head())

# Función para calcular distancia haversine entre dos puntos
def haversine(lon1, lat1, lon2, lat2):
    """
    Calcula la distancia entre dos puntos en la Tierra
    usando la fórmula haversine
    """
    # Convertir grados a radianes
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])
    
    # Fórmula haversine
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a))
    r = 6371  # Radio de la Tierra en kilómetros
    
    return c * r

# Ejemplo: calcular distancia entre Madrid y Alcalá de Henares
madrid = df[df['Ciudad'] == 'Madrid'].iloc[0]
alcala = df[df['Ciudad'] == 'Alcalá de Henares'].iloc[0]

distancia = haversine(madrid['Longitud'], madrid['Latitud'], 
                     alcala['Longitud'], alcala['Latitud'])
print(f"Distancia Madrid-Alcalá: {distancia:.2f} km")
```

### Implementación básica del algoritmo Vecino más Cercano

```python
def nearest_neighbor_tsp(cities_df):
    """
    Implementación básica del algoritmo Vecino más Cercano para TSP
    """
    n = len(cities_df)
    cities = cities_df.values
    
    # Comenzar desde la primera ciudad
    unvisited = set(range(1, n))
    current = 0
    route = [current]
    total_distance = 0
    
    while unvisited:
        nearest = min(unvisited, 
                     key=lambda city: haversine(cities[current][2], cities[current][1],
                                               cities[city][2], cities[city][1]))
        
        distance = haversine(cities[current][2], cities[current][1],
                           cities[nearest][2], cities[nearest][1])
        
        total_distance += distance
        route.append(nearest)
        unvisited.remove(nearest)
        current = nearest
    
    # Regresar al inicio
    final_distance = haversine(cities[current][2], cities[current][1],
                              cities[0][2], cities[0][1])
    total_distance += final_distance
    route.append(0)
    
    return route, total_distance

# Usar con el dataset
route, distance = nearest_neighbor_tsp(df)
print(f"Ruta encontrada: {[df.iloc[i]['Ciudad'] for i in route]}")
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

### Análisis del Dataset Madrid
- **Número de ciudades**: Variable según el dataset
- **Área geográfica**: Comunidad de Madrid
- **Reto computacional**: Perfecto para algoritmos heurísticos
- **Aplicación práctica**: Optimización de rutas de distribución en Madrid

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

### Áreas de Contribución Sugeridas
- Implementación de algoritmos TSP
- Análisis del dataset de Madrid
- Visualizaciones de rutas
- Comparativas de rendimiento
- Nuevos datasets de ciudades
- Documentación y tutoriales
- Tests y validaciones

## 🚀 Ideas para Desarrollo Futuro

- **Implementar algoritmos clásicos**: Fuerza bruta, vecino más cercano, 2-opt
- **Visualización de rutas**: Mapas interactivos con las rutas optimizadas
- **Comparativas de rendimiento**: Benchmarks entre diferentes algoritmos
- **Interfaz web**: Aplicación para visualizar soluciones TSP
- **Extensión de datasets**: Agregar más ciudades españolas o europeas
- **Algoritmos avanzados**: Implementar metaheurísticas como algoritmos genéticos

## 📚 Recursos Adicionales

- [TSP FAQ - Universidad de Waterloo](http://www.math.uwaterloo.ca/tsp/index.html)
- [TSPLIB - Biblioteca de Instancias TSP](http://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/)
- [Concorde TSP Solver](http://www.math.uwaterloo.ca/tsp/concorde.html)

---

**Autor**: [solben](https://github.com/solben)  
**Licencia**: MIT  
**Última Actualización**: Mayo 2025

*"El Problema del Viajante: donde la simplicidad del enunciado contrasta con la complejidad de la solución"*