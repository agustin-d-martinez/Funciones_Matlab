# Funciones de Control y Estimación en MATLAB

Este repositorio contiene un conjunto de funciones y modelos de Simulink diseñados para asistir en el análisis y diseño de controladores, estimadores de estado, prefiltros y estudio del lugar de las raíces.  
Todas las funciones fueron realizadas pensando en problemas típicos de parciales y prácticos de la materia Control Automático / Teoría de Control.

## Requisitos

- **MATLAB 2023** (tanto scripts como Simulink fueron desarrollados y probados en esta versión).  
  Versiones anteriores o posteriores pueden presentar incompatibilidades.
- Para utilizar correctamente las funciones, **todas deben encontrarse en la misma carpeta** que el script principal.
- Si desea ejecutar los ejemplos incluidos, **mueva los archivos de ejemplo a la misma carpeta** donde se encuentren las funciones.

---

## Estructura del repositorio
```
Ejemplos/
    ├── final2023_02_23.m
    ├── final2024.m
    ├── parcial2022.m
    ├── parcial2023.m
    ├── parcial2024.m
    ├── parcial2025.m
    ├── Prueba.m
	└── ...
func/
    ├── anguloCero.m
    ├── caso1.m
    ├── caso2.m
    ├── control.m
    └── ...
simulink/
    ├── Simulink_Planta_Digital/
        ├── simulink_caso1_digital.slx
        ├── simulink_caso2_digital.slx
        ├── simulink_caso3_digital.slx
        ├── simulink_caso4_digital.slx
        └── simulink_real_estados.slx
    ├── simulink_caso1_adc.slx
    ├── simulink_caso1.slx
    ├── simulink_caso2_adc.slx
    ├── simulink_caso2.slx
    ├── simulink_caso3_adc.slx
    ├── simulink_caso3.slx
    ├── simulink_caso4_adc.slx
    ├── simulink_caso4.slx
    ├── simulink_planta.slx
    ├── simulink_real_Estados_digital.slx
    └── simulink_real_Estados.slx
.gitattributes
README.md
```
---

# Lista de funciones

## Geometría del Lugar de las Raíces
- **anguloCero**  
  Dado un conjunto de polos y ceros y un punto objetivo \( x \), devuelve la posición donde debería añadirse un **cero** para que dicho punto pertenezca al lugar de las raíces.

- **anguloPolo**  
  Análogo a `anguloCero`, pero devuelve dónde debería añadirse un **polo** para que \( x \) pertenezca al lugar de las raíces.

- **rlocusGain**  
  Similar al comando `rlocus`, pero devuelve **el valor de \( K \)** tal que uno de los polos de lazo cerrado sea igual (o lo más cercano posible) al polo deseado `p_des`.

---

## Conversión entre amortiguamiento y overshoot
- **damping2overshoot**  
  Convierte un valor de amortiguamiento \( $ \varepsilon $ \) en overshoot \( MP\% \).

- **overshoot2damping**  
  Convierte el overshoot \( MP\% \) al valor de amortiguamiento \( $ \varepsilon $ \).

---

## Generación de variables para casos de estimadores (Simulink)

Cada una de estas funciones arma automáticamente las matrices y parámetros necesarios para simular estimadores en Simulink:

- **caso1**  
- **caso2**  
- **caso3** *(solo SISO)*  
- **caso4** *(puede fallar según la configuración)*  

Cada caso genera las variables para los modelos:
- `simulink_casoX`
- `simulink_casoX_adc`
- `simulink_casoX_digital`

---

## Diseño de controladores y estimadores

- **control**  
  Función integral que devuelve:
  - Vector de realimentación de estados \( K \)
  - Vector de estimador \( $K_e$ \)
  - Prefiltros \( $K_0$ \) y \( $K_{00}$ \)

  Parámetros adicionales opcionales:
  - Polos del estimador  
  - Valores en infinito `Rinf` y `Yinf`  
  - Tiempo de muestreo `Ts` (solo para planta discreta)

  **Nota:** Esta función encapsula la lógica de:  
  `stateFeedback`, `stateEstimator`, `prefilterFeedback` y `prefilterEstimaro`.

---

### Funciones específicas internas

- **stateFeedback**  
  Devuelve la matriz de realimentación de estados \( K \) y la matriz equivalente \( $A_n$ \).

- **stateEstimator**  
  Devuelve la matriz del estimador \( $K_e$ \).

- **prefilterFeedback**  
  Devuelve el prefiltro \( $K_0$ \) para realimentación de estados (con parámetros opcionales `Rinf`, `Yinf`).

- **prefilterEstimaro**  
  Devuelve el prefiltro \( $K_{00}$ \) para el estimador (con `Rinf`, `Yinf`).

---

## Utilidades adicionales

- **pidValues**  
  Dado un controlador, devuelve sus constantes \( $K_P, K_D, K_I$ \).

- **tf2ssControlable**  
  Similar a `tf2ss`, pero devuelve la representación **canónica controlable** \( A, B, C, D \).

---

# Ejemplos

En la carpeta de ejemplos se incluyen resoluciones de parciales que muestran cómo utilizar estas funciones y comprobar su funcionamiento.  
Se recomienda revisar esos archivos antes de utilizar el paquete completo.

---

# Notas finales

- Consulte los **prototipos de cada función** para ver parámetros y valores devueltos.  
- Los modelos de Simulink requieren que las variables sean generadas previamente mediante las funciones `casoX`.
