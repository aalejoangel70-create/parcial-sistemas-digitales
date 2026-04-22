# parcial-sistemas-digitales
### Parte Conceptual
## 1. Latch vs Flip-Flop

### Diferencia Principal

| Característica | Latch | Flip-Flop |
|---|---|---|
| Activación | Por nivel (mientras la señal esté activa) | Por flanco (subida ↑ o bajada ↓ del reloj) |
| Sincronismo | Asíncrono – no depende del reloj | Síncrono – controlado por reloj |
| Estabilidad | Menos estable, puede cambiar en cualquier momento | Más estable y predecible |
| Uso común | Circuitos simples o de baja velocidad | Registros, contadores, memorias |

Un latch cambia su salida mientras la señal de habilitación está activa, un flip-flop solo cambia en el momento exacto del flanco del reloj.

### Tipos de Latch

#### SR Latch (Set-Reset)
- Entradas: S (Set) y R (Reset)
- Funcionamiento:

| S | R | Q (salida) | Descripción |
|---|---|---|---|
| 0 | 0 | Q (sin cambio) | Mantiene estado |
| 1 | 0 | 1 | Set – pone Q en 1 |
| 0 | 1 | 0 | Reset – pone Q en 0 |
| 1 | 1 |  | Estado prohibido |

- Característica: Es el más básico. El estado S=1, R=1 es inválido porque genera una condición indeterminada.

---

#### D Latch (Data)
- Entradas: D (dato) y E (Enable/habilitación)
- Funcionamiento:

| E | D | Q |
|---|---|---|
| 1 | 0 | 0 |
| 1 | 1 | 1 |
| 0 | X | Q (sin cambio) |

- Característica: Elimina el estado prohibido del SR. Cuando E=1, la salida "sigue" a D. Cuando E=0, "congela" el último valor.

---

#### JK Latch
- Entradas: J (similar a Set), K (similar a Reset) y Clock
- Funcionamiento:

| J | K | Q siguiente |
|---|---|---|
| 0 | 0 | Q (sin cambio) |
| 1 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 1 | Toggle (complemento de Q) |

- Característica: Resuelve el estado prohibido del SR reemplazándolo por toggle (conmutación).

---

### Tipos de Flip-Flop

> Todos los flip-flops se activan en el flanco del reloj (subida o bajada), no por nivel.

#### FF tipo D (Data)
- El más utilizado en la práctica.
- En cada flanco de reloj: Q toma el valor de D.
- Usado en: registros, memorias RAM, pipelines.

```
     D ────┐
           │  [FF-D] ──── Q
    CLK ───┘
```

---

#### FF tipo SR
- Igual al latch SR pero activado solo en el flanco del reloj.
- Sigue teniendo el estado prohibido S=R=1.

---

#### FF tipo JK
- Versión mejorada del SR.
- J=K=1 produce toggle en el flanco del reloj.
- El más versátil de todos.

| J | K | Q siguiente |
|---|---|---|
| 0 | 0 | Q |
| 1 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 1 | Q' (toggle) |

---

#### FF tipo T (Toggle)
- Tiene una sola entrada T.
- Si T=1 → conmuta en cada flanco del reloj.
- Si T=0 → mantiene su estado.
- Muy usado en contadores binarios.

| T | Q siguiente |
|---|---|
| 0 | Q (sin cambio) |
| 1 | Q' (toggle) |

---

## 2. Multiplexor vs Demultiplexor

### Diferencia Principal

| | Multiplexor (MUX) | Demultiplexor (DEMUX) |
|---|---|---|
| Función | Muchas entradas → 1 salida | 1 entrada → Muchas salidas |
| Analogía | Como un selector de canales en TV | Como un distribuidor postal |
| Señales de control | Seleccionan cuál entrada pasa a la salida | Seleccionan a cuál salida se envía la entrada |
| Dirección del dato | Convergente (muchos a uno) | Divergente (uno a muchos) |

> En si el MUX elige de dónde viene el dato, el DEMUX elige a dónde va el dato.

---

###  Multiplexor de 8 Entradas (MUX 8:1)

- Entradas de datos: I0, I1, I2, I3, I4, I5, I6, I7
- Líneas de selección: S0, S1, S2 (3 bits → 2³ = 8 combinaciones)
- Salida: Y (una sola)

Tabla de selección:

| S2 | S1 | S0 | Salida Y |
|----|----|----|----------|
| 0  | 0  | 0  | I0       |
| 0  | 0  | 1  | I1       |
| 0  | 1  | 0  | I2       |
| 0  | 1  | 1  | I3       |
| 1  | 0  | 0  | I4       |
| 1  | 0  | 1  | I5       |
| 1  | 1  | 0  | I6       |
| 1  | 1  | 1  | I7       |

###  Demultiplexor de 8 Salidas (DEMUX 1:8)

- Entrada de datos:D (una sola)
- Líneas de selección: S0, S1, S2
- Salidas: Y0, Y1, Y2, Y3, Y4, Y5, Y6, Y7

Tabla de selección:

| S2 | S1 | S0 | Salida activa |
|----|----|----|---------------|
| 0  | 0  | 0  | Y0 = D        |
| 0  | 0  | 1  | Y1 = D        |
| 0  | 1  | 0  | Y2 = D        |
| 0  | 1  | 1  | Y3 = D        |
| 1  | 0  | 0  | Y4 = D        |
| 1  | 0  | 1  | Y5 = D        |
| 1  | 1  | 0  | Y6 = D        |
| 1  | 1  | 1  | Y7 = D        |

---

3. Sumador Medio, Sumador Completo y Circuitos Secuenciales
Sumador Medio (Half Adder)
Es un circuito que suma 2 bits y da dos resultados: la suma y el acarreo. Su limitación es que no puede recibir acarreo de entrada, por eso no se puede encadenar solo.
Sumador Completo (Full Adder)
A diferencia del medio, este sí recibe un acarreo de entrada además de los 2 bits, lo que permite encadenar varios para sumar números más grandes.
Circuitos Secuenciales
Son circuitos que tienen memoria, es decir, su salida depende no solo de lo que entra ahora sino también de lo que pasó antes. Usan flip-flops para guardar ese estado. Ejemplos típicos son los contadores y los registros.

## 4. Mapa de Karnaugh

###  ¿Qué es?

El Mapa de Karnaugh es una herramienta gráfica que permite simplificar funciones booleanas de forma visual y sin hacer álgebra compleja.
---

###  ¿Para qué sirve?

- Minimizar la cantidad de compuertas lógicas necesarias en un circuito.
- Reducir el costo, tamaño y consumo de energía del hardware.
- Obtener la expresión booleana más simple posible (Suma de Productos o Producto de Sumas).
- Identificar implicantes, implicantes primos y simplificar con errores mínimos.

---

###  ¿Cómo funciona?

1. Se organiza como una tabla donde las celdas adyacentes difieren en solo 1 bit (usando Código Gray).
2. Se colocan los 1s de la función en las celdas correspondientes.
3. Se agrupan los 1s en grupos de 1, 2, 4, 8... (potencias de 2).
4. Cada grupo elimina una variable de la expresión final.
5. Cuanto más grande el grupo → expresión más simplificada.
---

###  Ejemplo con 2 variables (A, B)

Función: F(A,B) = {1, 2, 3} → minterms 1, 2 y 3

```
         B=0    B=1
   A=0 [  0  |  1  ]   ← minterm 1
   A=1 [  1  |  1  ]   ← minterms 2 y 3
```

- El grupo de los tres 1s se simplifica a: F = A + B
- Sin K-Map: F = A'B + AB' + AB (más complejo)
- Con K-Map: F = A + B  (mucho más simple)
