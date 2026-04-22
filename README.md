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

###  Demultiplexor de 8 Salidas (DEMUX 1:8)

- Entrada de datos:D (una sola)
- Líneas de selección: S0, S1, S2
- Salidas: Y0, Y1, Y2, Y3, Y4, Y5, Y6, Y7

## 3. Sumador Medio, Sumador Completo y Circuitos Secuenciales

Sumador Medio (Half Adder)
Es un circuito que suma 2 bits y da dos resultados: la suma y el acarreo. Su limitación es que no puede recibir acarreo de entrada, por eso no se puede encadenar solo.

Sumador Completo (Full Adder)
A diferencia del medio, este sí recibe un acarreo de entrada además de los 2 bits, lo que permite encadenar varios para sumar números más grandes.

Circuitos Secuenciales
Son circuitos que tienen memoria, es decir, su salida depende no solo de lo que entra ahora sino también de lo que pasó antes. Usan flip-flops para guardar ese estado. Ejemplos típicos son los contadores y los registros.

## 4. Mapa de Karnaugh
El Mapa de Karnaugh es una herramienta gráfica que permite simplificar funciones booleanas de forma visual y sin hacer álgebra compleja.
sirve para:
- Minimizar la cantidad de compuertas lógicas necesarias en un circuito.
- Reducir el costo, tamaño y consumo de energía del hardware.
- Identificar implicantes, implicantes primos y simplificar con errores mínimos.

###  ¿Cómo funciona?

1. Se organiza como una tabla donde las celdas adyacentes difieren en solo 1 bit (usando Código Gray).
2. Se colocan los 1s de la función en las celdas correspondientes.
3. Se agrupan los 1s en grupos de 1, 2, 4, 8... (potencias de 2).
4. Cada grupo elimina una variable de la expresión final.
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


---

## 2.a Análisis de la ecuación booleana

### Ecuación:

```
X = [ AB̄((C + BD) + ĀB̄) ] C
```

---

### Descomposición por partes

Para entender y construir el circuito, se divide la ecuación en partes más pequeñas:

| Parte | Expresión | Descripción |
|-------|-----------|-------------|
| P1 | `BD` | AND entre B y D |
| P2 | `C + BD` | OR entre C y el resultado P1 |
| P3 | `AB̄ · (C + BD)` | AND entre A, B̄ y P2 |
| P4 | `ĀB̄` | AND entre A negado y B negado |
| P5 | `P3 + P4` | OR entre P3 y P4 → contenido del corchete |
| X | `P5 · C` | AND final con C |

---

### Tabla de Verdad

> Variables: A, B, C, D → 2⁴ = 16 combinaciones

| A | B | C | D | BD | C+BD | AB̄ | AB̄(C+BD) | ĀB̄ | [P5] | X |
|---|---|---|---|----|------|----|-----------|-----|------|-------|
| 0 | 0 | 0 | 0 |  0 |  0   |  0 |     0     |  1  |  1   | 0 |
| 0 | 0 | 0 | 1 |  0 |  0   |  0 |     0     |  1  |  1   | 0 |
| 0 | 0 | 1 | 0 |  0 |  1   |  0 |     0     |  1  |  1   | 1 |
| 0 | 0 | 1 | 1 |  0 |  1   |  0 |     0     |  1  |  1   | 1 |
| 0 | 1 | 0 | 0 |  0 |  0   |  0 |     0     |  0  |  0   | 0 |
| 0 | 1 | 0 | 1 |  1 |  1   |  0 |     0     |  0  |  0   | 0 |
| 0 | 1 | 1 | 0 |  0 |  1   |  0 |     0     |  0  |  0   | 0 |
| 0 | 1 | 1 | 1 |  1 |  1   |  0 |     0     |  0  |  0   | 0 |
| 1 | 0 | 0 | 0 |  0 |  0   |  1 |     0     |  0  |  0   | 0 |
| 1 | 0 | 0 | 1 |  0 |  0   |  1 |     0     |  0  |  0   | 0 |
| 1 | 0 | 1 | 0 |  0 |  1   |  1 |     1     |  0  |  1   | 1 |
| 1 | 0 | 1 | 1 |  0 |  1   |  1 |     1     |  0  |  1   | 1 |
| 1 | 1 | 0 | 0 |  0 |  0   |  0 |     0     |  0  |  0   | 0 |
| 1 | 1 | 0 | 1 |  1 |  1   |  0 |     0     |  0  |  0   | 0 |
| 1 | 1 | 1 | 0 |  0 |  1   |  0 |     0     |  0  |  0   | 0 |
| 1 | 1 | 1 | 1 |  1 |  1   |  0 |     0     |  0  |  0   | 0 |

> X = 1 únicamente cuando: `A=0,B=0,C=1,D=0` | `A=0,B=0,C=1,D=1` | `A=1,B=0,C=1,D=0` | `A=1,B=0,C=1,D=1`
>
> Patrón: X solo es 1 cuando C=1 y B=0

---

### Circuito Lógico

El circuito se construye con las siguientes compuertas en orden:

```
Entradas: A, B, C, D

1. NOT(B) → B̄
2. NOT(A) → Ā
3. NOT(B) → B̄  (para ĀB̄)

4. AND(B, D)              → BD
5. OR(C, BD)              → C + BD
6. AND(A, B̄, C+BD)       → AB̄(C+BD)
7. AND(Ā, B̄)             → ĀB̄
8. OR(AB̄(C+BD), ĀB̄)     → [ corchete ]
9. AND(corchete, C)       → X  
```

Diagrama del circuito:

```
A ────────────────────────────────────[AND]──────[OR]──[AND]── X
B ──[NOT]────────────────────────────[AND]        │       │
                                                  │       │
C ──────────────────────[OR]─────────[AND]        │     C─┘
B ──────────[AND]────────┘                        │
D ──────────┘                                     │
                                                  │
A ──[NOT]──[AND]──────────────────────────────────┘
B ──[NOT]──┘
```

> 📎 Ver imagen del circuito: `circuito_2a.svg`
