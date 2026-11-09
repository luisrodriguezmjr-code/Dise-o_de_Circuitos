# unidad 1
# 🔌 Resumen Unidad 1: Análisis de Lógica Combinacional con Diseño Clásico

> **Asignatura:** Diseño de Circuitos
>
> **Carrera:** Computación (2.º Ciclo) — Universidad Nacional de Loja
>
> **Período:** Septiembre 2026 – Marzo 2027
>
> **Docente:** Ing. Luis Darío Sinche Cueva, Mg.

## 📑 Tabla de Contenidos

* [Semana 1: Sistemas Numéricos, Códigos y Compuertas Lógicas](#semana-1-sistemas-numéricos-códigos-y-compuertas-lógicas)

* [Semana 2: Álgebra de Boole y Formas Canónicas](#semana-2-álgebra-de-boole-y-formas-canónicas)

* [Semana 3: Mapas de Karnaugh, Condiciones Indiferentes y Universalidad](#semana-3-mapas-de-karnaugh-condiciones-indiferentes-y-universalidad)

* [Semana 4: Circuitos Aritméticos y Elementos de la ALU](#semana-4-circuitos-aritméticos-y-elementos-de-la-alu)

## Semana 1: Sistemas Numéricos, Códigos y Compuertas Lógicas

### 1. Representación Binaria y Niveles de Voltaje

Los sistemas digitales emplean el sistema binario (0 y 1) debido a que los transistores operan como interruptores controlados por voltaje:

* 0 **lógico:** Interruptor abierto (sin paso de corriente, voltaje bajo).

* 1 **lógico:** Interruptor cerrado (paso de corriente, voltaje alto).

En la familia lógica **TTL 74LS**, los estados lógicos se definen en los siguientes rangos:

```
  Voltaje (V)      Salida (Promete)          Entrada (Exige)
     5.0 V ┌─────────────────────────┐  ┌─────────────────────────┐
           │                         │  │                         │
           │       Sale un 1         │  │        Lee un 1         │
     2.7 V ├─────────────────────────┤  │                         │
           │      Solo de paso       │  ├─────────────────────────┤ 2.0 V
           │                         │  │     Zona Indefinida     │
     0.8 V │                         │  ├─────────────────────────┤
           ├─────────────────────────┤  │        Lee un 0         │
     0.5 V │       Sale un 0         │  │                         │
     0.0 V └─────────────────────────┘  └─────────────────────────┘

```

* **Margen de Ruido Alto (**VNH**):** 2.7V - 2.0V = 0.7V

* **Margen de Ruido Bajo (**VNL**):** 0.8V - 0.5V = 0.3V

* **Zona Indefinida:** Entre 0.8V y 2.0V. El chip no garantiza la lectura correcta (causado comúnmente por pines flotantes/no conectados).

### 2. Sistemas Numéricos y Conversiones

En todo sistema posicional, cada posición representa una potencia de la base:

* **Binario (Base 2):** Pesos en potencias de 2 (128, 64, 32, 16, 8, 4, 2, 1). 1Byte = 8bits.

* **Conversión Decimal a Binario:** Divisiones sucesivas por $2$, tomando los residuos de abajo hacia arriba.

* **Abreviaturas del Binario:**

  * **Hexadecimal (Base 16):** Agrupa en bloques de 4bits desde la derecha (10 al 15 representan A, B, C, D, E, F).

  * **Octal (Base 8):** Agrupa en bloques de 3bits desde la derecha.

### 3. Aritmética Binaria y Representación de Signo

* **Suma Binaria Fundamental:**

  * 0 + 0 = 0

  * 0 + 1 = 1

  * 1 + 0 = 1

  * 1 + 1 = 10_2 (Escribo 0, llevo acarreo de 1)

  * 1 + 1 + 1 = 11_2 (Escribo 1, llevo acarreo de 1)

* **Complemento a Dos (Números con signo):**

  * El bit más significativo (MSB) indica el signo: `0` para positivo y `1` para negativo.

  * **Rango para** n **bits:** Desde -2^n-1 hasta +2^n-1 - 1 (para 4 bits: -8a  +7).

  * **Obtención del negativo:** Invertir todos los bits (complemento a 1) y sumar 1.

  * **Ventaja:** Permite restar utilizando el mismo hardware del sumador: A - B = A + (-B).

  * **Desbordamiento (Overflow):** Ocurre cuando el resultado excede los bits disponibles (ej. la suma de dos positivos da un negativo).

### 4. Códigos Digitales

* **BCD (Binary-Coded Decimal):** Representa cada dígito decimal (0-9) mediante 4bits. Las combinaciones de 1010_2 a 1111_2 no se utilizan. Se usa en displays de 7 segmentos.

* **Código Gray:** Código no ponderado en el que entre números consecutivos **solo cambia 1 bit**. Evita errores de lectura transitoria en sensores y encoders rotativos.

* **ASCII:** Estándar de 7bits (128 caracteres) para representar texto (ej. `'A'`= 65_10= 41_{16} = 01000001_2).
 
### 5. Compuertas Lógicas y Circuito Integrado 74LS08

| Compuerta | Símbolo ANSI | Expresión Booleana | Condición de Salida ($Y=1$) | 
| ----- | ----- | ----- | ----- | 
| **NOT** | Triangle + Bubble | Y = A' | Invertida | 
| **AND** | D-Shape | Y = A X B | Todas las entradas son 1 | 
| **OR** | Curved D-Shape | Y = A + B | Al menos una entrada es 1 | 
| **NAND** | D-Shape + Bubble | Y = (A x B)' | Al menos una entrada es 0 | 
| **XOR** | Double Curved D | Y = A ⊕ B | Las entradas son **distintas** | 
| **XNOR** | Double Curved + Bubble | Y = (A ⊕ B)' | Las entradas son **iguales** | 

#### Pinout del Circuito Integrado 74LS08 (Quad 2-Input AND):

```
                     ┌───┬───┐
                  1A │1  └──┘ 14│ VCC (+5V)
                  1B │2       13│ 4B
                  1Y │3       12│ 4A
                  2A │4 74LS08 11│ 4Y
                  2B │5       10│ 3B
                  2Y │6        9│ 3A
            (GND) 0V │7        8│ 3Y
                     └─────────┘

```

* **Nomenclatura:** `SN` (Fabricante - Texas Instruments), `74` (Comercial), `LS` (TTL Low-Power Schottky), `08` (4 compuertas AND de 2 entradas), `N` (Encapsulado DIP-14).

## Semana 2: Álgebra de Boole y Formas Canónicas

### 1. Equivalencias entre Hardware y Álgebra

* Cable de entrad <-> Variable (A, B, C)

* Compuerta AND <-> Producto ( . )

* Compuerta OR <-> Suma (+)

* Compuerta NOT <->  Complemento (')

* **Precedencia:** NOT <-> AND <->  OR

### 2. Postulados de Huntington y Teoremas

#### Postulados Fundamentales:

* **Identidad:** X + 0 = X | X . 1 = X

* **Complemento:** X + X' = 1 | X . X' = 0

* **Conmutativa:** X + Y = Y + X | X . Y = Y . X

* **Distributiva:** X . (Y + Z) = X . Y + X . Z | X + (Y . Z) = (X + Y) . (X + Z)

#### Teoremas Principales:

* **Elemento Nulo:** X + 1 = 1 | X . 0 = 0

* **Idempotencia:** X + X = X | X . X = X

* **Involución:** (X')' = X

* **Absorción:** X + X . Y = X | X . (X + Y) = X

* **Absorción del Complemento:** X + X' . Y = X + Y | X . (X' + Y) = X . Y

* **Adyacencia:** X . Y + X . Y' = X | (X + Y) . (X + Y') = X

* **Consenso:** X . Y + X' . Z + Y . Z = X . Y + X' . Z

#### Leyes de De Morgan:

(A . B)' = A' + B'

(A + B)' = A' . B'

### 3. Formas Canónicas: SOP y POS

* **Minitérmino (**m_i**):** Producto de todas las variables. Vale 1 en una sola combinación (variable en 1 -> sin complementar; en 0 ->complementada).

* **Maxitérmino (**M_i**):** Suma de todas las variables. Vale 0 en una sola combinación (variable en 0 -> sin complementar; en 1 -> complementada). Relación: M_i = (m_i)'.

#### Representaciones:

* **SOP (Sum of Products / Suma de Minitérminos):** Lee las filas donde la salida F = 1.

  F = ∑m(3, 5, 6, 7) = A'BC + AB'C + ABC' + ABC
  

* **POS (Product of Sums / Producto de Maxitérminos):** Lee las filas donde la salida $F = 0$.

  F = π M(0, 1, 2, 4) = (A + B + C)(A + B + C')(A + B' + C)(A' + B + C)

## Semana 3: Mapas de Karnaugh, Condiciones Indiferentes y Universalidad

### 1. Mapas de Karnaugh (K-Maps)

Herramienta gráfica basada en el **Teorema de Adyacencia** (XY + XY' = X). Las celdas se organizan en **Código Gray** ($00, 01, 11, 10$) para garantizar que entre celdas adyacentes solo varíe un bit.

#### Estructura de un Mapa de 4 Variables:

```
  AB \ CD   00   01   11   10
        ┌────┬────┬────┬────┐
     00 │ m0 │ m1 │ m3 │ m2 │
        ├────┼────┼────┼────┤
     01 │ m4 │ m5 │ m7 │ m6 │
        ├────┼────┼────┼────┤
     11 │m12 │m13 │m15 │m14 │
        ├────┼────┼────┼────┤
     10 │ m8 │ m9 │m11 │m10 │
        └────┴────┴────┴────┘

```

#### Reglas de Agrupación:

1. Grupos de tamaño potencia de 2 (1, 2, 4, 8, 16 celdas).

2. Agrupar la mayor cantidad de celdas posibles (elimina más variables).

3. Formas estrictamente rectangulares (sin diagonales ni forma de L).

4. El mapa es **toroidal**: los bordes opuestos y las 4 esquinas son adyacentes.

#### Implicantes:

* **Implicante Primo:** Grupo válido que no puede expandirse más.

* **Implicante Primo Esencial:** Grupo que contiene al menos un 1 no cubierto por ningún otro implicante. Debe incluirse obligatoriamente en la solución final.

### 2. Condiciones Indiferentes ("Don't Care" - X)

Combinaciones de entrada que nunca ocurren en la práctica (ej. números $10$ al $15$ en código BCD).

* Se pueden agrupar como 1 si ayudan a hacer un grupo más grande.

* Se ignoran como 0 si no aportan a la simplificación.

* **Regla clave:** Nunca se forman grupos compuestos únicamente por condiciones indiferentes (X).

### 3. Universalidad de Compuertas NAND y NOR

Cualquier función lógica se puede construir utilizando únicamente compuertas NAND o únicamente compuertas NOR.

#### Construcción de compuertas básicas con NAND:

* **NOT:** A' = (A . A)'

* **AND:** A \cdot B = ((A . B)')'

* **OR:** A + B = (A' B')'. 

#### Conversión a 2 Niveles:

* **SOP** -> **Red NAND-NAND:** Aplica doble negación a la expresión SOP y distribuye De Morgan en la negación interna.

* **POS** -> **Red NOR-NOR:** Implementa la expresión POS directa con compuertas NOR.

## Semana 4: Circuitos Aritméticos y Elementos de la ALU

### 1. Semisumador y Sumador Completo

#### Semisumador (Half Adder - HA):

Suma 2 bits (A y B). Se usa únicamente en la columna 0.

* **Suma:** S = A ⊕ B

* **Acarreo de Salida:** C = A . B

#### Sumador Completo (Full Adder - FA):

Suma 3 bits: operandos A, B y acarreo entrante Cin.

```
                 A ──┐
                 B ──┼───►┌──────────────┐───► Suma (S)
              Cin ───┘    │ Full Adder   │
                          └──────┬───────┘
                                 │
                                 ▼
                          Cout (Acarreo de salida)

```

* **Suma (**S**):** S = A ⊕ B ⊕ Cin *(Vale* 1 *si hay un número IMPAR de unos)*.

* **Acarreo (**$Cout**):** Cout= AB + A Cin + B Cin *(Vale* 1 *si hay AL MENOS DOS unos)*.

### 2. Sumador de 4 Bits con Acarreo Encascadado (Ripple Carry Adder)

Conecta 4 Sumadores Completos en serie, pasando el acarreo Cout de cada etapa al Cin del siguiente.

```
       A3 B3          A2 B2          A1 B1          A0 B0
        │  │           │  │           │  │           │  │
     ┌──▼──▼──┐     ┌──▼──▼──┐     ┌──▼──▼──┐     ┌──▼──▼──┐
Cout │FA (3)  │◄────│FA (2)  │◄────│FA (1)  │◄────│FA (0)  │◄─── Cin=0
     └───┬────┘ C3  └───┬────┘ C2  └───┬────┘ C1  └───┬────┘
         │              │              │              │
         ▼              ▼              ▼              ▼
         S3             S2             S1             S0

```

#### Retardo de Propagación:

Dado que el acarreo avanza como una ola de derecha a izquierda, cada etapa introduce retardo (= 2  compuertas por etapa = 20 ns).

Retardo Total=2 . n . tgate

* **4 bits:** = 80ns

* **16 bits:** = 320 ns

* **32 bits:** = 640ns

### 3. Circuito Sumador / Restador Combinado

Aprovecha el Complemento a Dos introduciendo un bit de control M:

* M = 0 **(Suma):** La entrada B pasa intacta a través de una compuerta XOR (B ⊕ 0 = B) y $C_{in,0} = 0.

* M = 1 **(Resta):** La entrada B se invierte (B ⊕ 1 = B') y Cin,0 = 1 (agrega el +1 necesario para el complemento a dos).

### 4. Desbordamiento (Overflow) y Comparadores

* **Detección de Overflow (**V**):** Ocurre en operaciones con signo cuando el resultado excede la capacidad en bits del circuito. Se calcula como:
  


  V = Cn-1 ⊕ C_n (para 4 bits: V = C_3 ⊕ C_4)

* **Comparador de Magnitud:**

  * **Igualdad (**A=B**):** Aplica XNOR bit a bit y evalúa el producto (AND) de los resultados.

  * **Magnitud (**A > B **o** A < B**):** Evalúa desde el bit más significativo (MSB) hasta encontrar el primer bit que difiere.

* **Unidad Aritmético-Lógica (ALU):** Combina el sumador/restador, comparador y compuertas lógicas (AND/OR/XOR) seleccionando la operación deseada mediante un multiplexor.
