# parcial-sistemas-digitales
# 📘 Parcial – Electrónica Digital
### Parte Conceptual

> Desarrollado paso a paso con explicaciones claras de cada concepto.

---

## 📋 Tabla de Contenidos

1. [Latch vs Flip-Flop](#1-latch-vs-flip-flop)
2. [Multiplexor vs Demultiplexor](#2-multiplexor-vs-demultiplexor)
3. [Sumador Medio, Sumador Completo y Circuitos Secuenciales](#3-sumador-medio-sumador-completo-y-circuitos-secuenciales)
4. [Mapa de Karnaugh](#4-mapa-de-karnaugh)

---

## 1. Latch vs Flip-Flop

### 🔍 Diferencia Principal

| Característica | **Latch** | **Flip-Flop** |
|---|---|---|
| Activación | Por **nivel** (mientras la señal esté activa) | Por **flanco** (subida ↑ o bajada ↓ del reloj) |
| Sincronismo | **Asíncrono** – no depende del reloj | **Síncrono** – controlado por reloj |
| Estabilidad | Menos estable, puede cambiar en cualquier momento | Más estable y predecible |
| Uso común | Circuitos simples o de baja velocidad | Registros, contadores, memorias |

> 💡 **Resumen fácil:** Un latch cambia su salida **mientras** la señal de habilitación está activa. Un flip-flop solo cambia en el **momento exacto** del flanco del reloj.

---

### 🔸 Tipos de Latch

#### SR Latch (Set-Reset)
- **Entradas:** S (Set) y R (Reset)
- **Funcionamiento:**

| S | R | Q (salida) | Descripción |
|---|---|---|---|
| 0 | 0 | Q (sin cambio) | Mantiene estado |
| 1 | 0 | 1 | Set – pone Q en 1 |
| 0 | 1 | 0 | Reset – pone Q en 0 |
| 1 | 1 | ❌ | **Estado prohibido** |

- **Característica:** Es el más básico. El estado S=1, R=1 es inválido porque genera una condición indeterminada.

---

#### D Latch (Data)
- **Entradas:** D (dato) y E (Enable/habilitación)
- **Funcionamiento:**

| E | D | Q |
|---|---|---|
| 1 | 0 | 0 |
| 1 | 1 | 1 |
| 0 | X | Q (sin cambio) |

- **Característica:** Elimina el estado prohibido del SR. Cuando E=1, la salida "sigue" a D. Cuando E=0, "congela" el último valor.

---

#### JK Latch
- **Entradas:** J (similar a Set), K (similar a Reset) y Clock
- **Funcionamiento:**

| J | K | Q siguiente |
|---|---|---|
| 0 | 0 | Q (sin cambio) |
| 1 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 1 | **Toggle** (complemento de Q) |

- **Característica:** Resuelve el estado prohibido del SR reemplazándolo por **toggle** (conmutación).

---

### 🔸 Tipos de Flip-Flop

> Todos los flip-flops se activan en el **flanco del reloj** (subida o bajada), no por nivel.

#### FF tipo D (Data)
- El más utilizado en la práctica.
- En cada flanco de reloj: **Q toma el valor de D**.
- Usado en: registros, memorias RAM, pipelines.

```
     D ────┐
           │  [FF-D] ──── Q
    CLK ───┘
```

---

#### FF tipo SR
- Igual al latch SR pero **activado solo en el flanco** del reloj.
- Sigue teniendo el estado prohibido S=R=1.

---

#### FF tipo JK
- Versión mejorada del SR.
- J=K=1 produce **toggle** en el flanco del reloj.
- El más **versátil** de todos.

| J | K | Q siguiente |
|---|---|---|
| 0 | 0 | Q |
| 1 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 1 | Q' (toggle) |

---

#### FF tipo T (Toggle)
- Tiene una sola entrada T.
- Si T=1 → **conmuta** en cada flanco del reloj.
- Si T=0 → **mantiene** su estado.
- Muy usado en **contadores binarios**.

| T | Q siguiente |
|---|---|
| 0 | Q (sin cambio) |
| 1 | Q' (toggle) |

---

## 2. Multiplexor vs Demultiplexor

### 🔍 Diferencia Principal

| | **Multiplexor (MUX)** | **Demultiplexor (DEMUX)** |
|---|---|---|
| Función | **Muchas entradas → 1 salida** | **1 entrada → Muchas salidas** |
| Analogía | Como un selector de canales en TV | Como un distribuidor postal |
| Señales de control | Seleccionan **cuál entrada** pasa a la salida | Seleccionan **a cuál salida** se envía la entrada |
| Dirección del dato | Convergente (muchos a uno) | Divergente (uno a muchos) |

> 💡 **Resumen fácil:** El MUX elige **de dónde** viene el dato. El DEMUX elige **a dónde va** el dato.

---

### 🔸 Multiplexor de 8 Entradas (MUX 8:1)

- **Entradas de datos:** I0, I1, I2, I3, I4, I5, I6, I7
- **Líneas de selección:** S0, S1, S2 (3 bits → 2³ = 8 combinaciones)
- **Salida:** Y (una sola)

**Tabla de selección:**

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

**Diagrama:**
```
I0 ──┐
I1 ──┤
I2 ──┤
I3 ──┤──── [MUX 8:1] ──── Y (salida)
I4 ──┤
I5 ──┤
I6 ──┤
I7 ──┘
       ↑
   S2 S1 S0
  (selección)
```

**Expresión booleana:**
```
Y = (S2'·S1'·S0'·I0) + (S2'·S1'·S0·I1) + (S2'·S1·S0'·I2) + (S2'·S1·S0·I3)
  + (S2·S1'·S0'·I4) + (S2·S1'·S0·I5) + (S2·S1·S0'·I6) + (S2·S1·S0·I7)
```

---

### 🔸 Demultiplexor de 8 Salidas (DEMUX 1:8)

- **Entrada de datos:** D (una sola)
- **Líneas de selección:** S0, S1, S2
- **Salidas:** Y0, Y1, Y2, Y3, Y4, Y5, Y6, Y7

**Tabla de selección:**

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

**Diagrama:**
```
              ┌──── Y0
              ├──── Y1
              ├──── Y2
D (entrada) ──┤ [DEMUX 1:8] ──── Y3
              ├──── Y4
              ├──── Y5
              ├──── Y6
              └──── Y7
         ↑
     S2 S1 S0
```

---

## 3. Sumador Medio, Sumador Completo y Circuitos Secuenciales

### 🔸 Sumador Medio (Half Adder)

- **¿Qué hace?** Suma **2 bits** (A y B).
- **Salidas:** Suma (S) y Acarreo de salida (Cout)
- **Limitación:** ❌ No acepta acarreo de entrada (Cin). No se puede encadenar directamente.

**Tabla de verdad:**

| A | B | Suma (S) | Acarreo (C) |
|---|---|----------|-------------|
| 0 | 0 |    0     |      0      |
| 0 | 1 |    1     |      0      |
| 1 | 0 |    1     |      0      |
| 1 | 1 |    0     |      1      |

**Ecuaciones lógicas:**
```
S = A XOR B
C = A AND B
```

**Diagrama:**
```
A ──┬──[XOR]──── S (Suma)
    │
B ──┴──[AND]──── C (Acarreo)
```

---

### 🔸 Sumador Completo (Full Adder)

- **¿Qué hace?** Suma **3 bits**: A, B y **Cin** (acarreo entrante).
- **Salidas:** Suma (S) y Cout (acarreo de salida)
- **Ventaja:** ✅ Puede encadenarse para sumar números de N bits.

**Tabla de verdad:**

| A | B | Cin | Suma (S) | Cout |
|---|---|-----|----------|------|
| 0 | 0 |  0  |    0     |  0   |
| 0 | 0 |  1  |    1     |  0   |
| 0 | 1 |  0  |    1     |  0   |
| 0 | 1 |  1  |    0     |  1   |
| 1 | 0 |  0  |    1     |  0   |
| 1 | 0 |  1  |    0     |  1   |
| 1 | 1 |  0  |    0     |  1   |
| 1 | 1 |  1  |    1     |  1   |

**Ecuaciones lógicas:**
```
S    = A XOR B XOR Cin
Cout = (A AND B) OR (Cin AND (A XOR B))
```

**Diagrama (usando 2 Half Adders):**
```
A, B ──[Half Adder 1]──┬── S1 ──[Half Adder 2]──── S (Suma final)
                       │
                  Cin ─┘
                              └── C2 ──[OR]──── Cout
                  C1 ────────────────────┘
```

> 💡 Un sumador de 4 bits se construye encadenando 4 Full Adders donde el Cout de uno se conecta al Cin del siguiente.

---

### 🔸 Circuitos Secuenciales

**¿Qué son?**
Son circuitos digitales cuya salida **no solo depende de las entradas actuales**, sino también del **estado anterior** (tienen memoria).

**Diferencia con circuitos combinacionales:**

| | **Combinacional** | **Secuencial** |
|---|---|---|
| Memoria | ❌ No tiene | ✅ Tiene (usa flip-flops) |
| Salida depende de | Solo entradas actuales | Entradas actuales + estado anterior |
| Ejemplo | Sumador, MUX | Contador, registro, máquina de estados |

**Componentes clave:**
- **Flip-flops:** guardan el estado actual del circuito.
- **Lógica combinacional:** calcula el próximo estado y las salidas.
- **Reloj (CLK):** sincroniza las transiciones de estado.

**Tipos de circuitos secuenciales:**
1. **Contadores** – cuentan pulsos de reloj en binario.
2. **Registros de desplazamiento** – mueven datos bit a bit.
3. **Máquinas de estado finito (FSM)** – sistemas con estados definidos (semáforo, cajero, etc.).

**Diagrama general:**
```
Entradas ──┬──[Lógica Combinacional]──── Salidas
           │          │
           │     [Flip-Flops] ──── Estado actual
           └──────────┘
                CLK ──┘
```

---

## 4. Mapa de Karnaugh

### 🔍 ¿Qué es?

El **Mapa de Karnaugh (K-Map)** es una herramienta gráfica que permite **simplificar funciones booleanas** (lógica digital) de forma visual y sin hacer álgebra compleja.

Fue propuesto por **Maurice Karnaugh** en 1953 como una extensión visual de las tablas de verdad.

---

### 🎯 ¿Para qué sirve?

- **Minimizar** la cantidad de compuertas lógicas necesarias en un circuito.
- Reducir el **costo, tamaño y consumo de energía** del hardware.
- Obtener la **expresión booleana más simple** posible (Suma de Productos o Producto de Sumas).
- Identificar implicantes, implicantes primos y simplificar con errores mínimos.

---

### 🔸 ¿Cómo funciona?

1. Se organiza como una tabla donde las **celdas adyacentes difieren en solo 1 bit** (usando Código Gray).
2. Se colocan los **1s** de la función en las celdas correspondientes.
3. Se agrupan los 1s en grupos de **1, 2, 4, 8...** (potencias de 2).
4. Cada grupo **elimina una variable** de la expresión final.
5. Cuanto más grande el grupo → expresión más simplificada.

> ⚠️ **Regla clave:** Los grupos deben ser rectangulares, de tamaño potencia de 2, y pueden "envolver" los bordes del mapa.

---

### 🔸 Ejemplo con 2 variables (A, B)

**Función:** F(A,B) = {1, 2, 3} → minterms 1, 2 y 3

```
         B=0    B=1
   A=0 [  0  |  1  ]   ← minterm 1
   A=1 [  1  |  1  ]   ← minterms 2 y 3
```

- El grupo de los tres 1s se simplifica a: **F = A + B**
- Sin K-Map: F = A'B + AB' + AB (más complejo)
- Con K-Map: F = A + B ✅ (mucho más simple)

---

### 🔸 Ejemplo con 3 variables (A, B, C)

```
         BC=00  BC=01  BC=11  BC=10
   A=0 [  0  |   1  |   1  |   0  ]
   A=1 [  0  |   1  |   1  |   0  ]
```

- El grupo de 4 (columnas BC=01 y BC=11) → elimina A y C → queda: **F = B**

---

### 📌 Resumen de reglas de agrupación

| Tamaño del grupo | Variables eliminadas |
|---|---|
| 1 celda | 0 variables eliminadas |
| 2 celdas | 1 variable eliminada |
| 4 celdas | 2 variables eliminadas |
| 8 celdas | 3 variables eliminadas |

> 💡 **Meta:** Hacer los grupos lo más grandes posible para eliminar la mayor cantidad de variables.

---

## 👨‍💻 Autor

Parcial de Electrónica Digital  
Repositorio creado como parte de la evaluación conceptual del curso.
