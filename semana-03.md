[🏠 Índice](README.md) · [⬅️ Semana 2](semana-02.md) · [Semana 4 ➡️](semana-04.md)

---

# Semana 3 · Mapas de Karnaugh, indiferentes y NAND/NOR

> **Unidad 1:** Análisis de lógica combinacional con diseño clásico · **Diseño de Circuitos** · Computación, 2.º ciclo A · UNL

Esta semana tuvo dos sesiones:

| Sesión | Tema | Secciones |
|:---:|:---|:---:|
| **Sesión 1** | Mapas de Karnaugh: expresiones mínimas en SOP y POS | [2](#2-teorema-de-adyacencia-y-código-gray) a [7](#7-pos-mínima-con-el-mapa) |
| **Sesión 2** | Indiferentes y NAND/NOR: minimizar e implementar con una sola familia de compuertas | [8](#8-condiciones-indiferentes) a [11](#11-costo-e-integración) |

## Contenido de la semana

1. [Objetivos de la semana](#1-objetivos-de-la-semana)
2. [Teorema de adyacencia y código Gray](#2-teorema-de-adyacencia-y-código-gray)
3. [Mapas de 3 y 4 variables](#3-mapas-de-3-y-4-variables)
4. [Reglas para agrupar](#4-reglas-para-agrupar)
5. [Implicantes primos y esenciales](#5-implicantes-primos-y-esenciales)
6. [Procedimiento para la SOP mínima](#6-procedimiento-para-la-sop-mínima)
7. [POS mínima con el mapa](#7-pos-mínima-con-el-mapa)
8. [Condiciones indiferentes](#8-condiciones-indiferentes)
9. [Universalidad de NAND y NOR](#9-universalidad-de-nand-y-nor)
10. [De SOP a NAND-NAND](#10-de-sop-a-nand-nand)
11. [Costo e integración](#11-costo-e-integración)
12. [Ideas clave de la semana](#12-ideas-clave-de-la-semana)

---

## 1. Objetivos de la semana

- **Sesión 1:** obtener la expresión mínima en **SOP** y en **POS** de funciones de 3 y 4 variables mediante mapas de Karnaugh, justificando cada agrupación.
- **Sesión 2:** minimizar una función de 4 variables con **condiciones indiferentes** e implementarla solo con compuertas **NAND**, cuantificando el ahorro en compuertas y circuitos integrados.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 2. Teorema de adyacencia y código Gray

### La clave del mapa

$$X \cdot Y + X \cdot Y' = X$$

Si dos minitérminos difieren en **una sola variable**, esa variable **desaparece**.

> **Ejemplo:** $m_4\ (AB'C') + m_5\ (AB'C) \rightarrow AB'$ (la $C$ desaparece).

### ¿Por qué el orden 00, 01, 11, 10?

- En binario normal, de $01 \rightarrow 10$ cambian **2 bits**.
- En **código Gray**, cada celda es vecina lógica de la siguiente, y la última de la primera:

$$00 \rightarrow 01 \rightarrow 11 \rightarrow 10 \rightarrow 00$$

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 3. Mapas de 3 y 4 variables

### Mapa de 3 variables

Filas: $A$. Columnas: $BC$ en código Gray.

| $A \backslash BC$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **0** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **1** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |

> [!WARNING]
> $m_3$ va **antes** que $m_2$ (orden Gray).

### Mapa de 4 variables

Filas: $AB$. Columnas: $CD$. Ambas en código Gray.

| $AB \backslash CD$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **00** | 0 | 1 | 3 | 2 |
| **01** | 4 | 5 | 7 | 6 |
| **11** | 12 | 13 | 15 | 14 |
| **10** | 8 | 9 | 11 | 10 |

Los bordes y las esquinas **también son vecinos**. Las 4 esquinas ($m_0, m_2, m_8, m_{10}$) forman el grupo $B'D'$.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 4. Reglas para agrupar

| Regla | Detalle |
|:---:|:---|
| **1. Tamaño $2^k$** | Grupos de 1, 2, 4, 8 o 16 celdas. Un grupo de $2^k$ celdas elimina $k$ variables |
| **2. Rectangular** | Sin formas en L ni diagonales |
| **3. Lo más grande posible** | Grupos grandes dan términos más cortos |
| **4. El mapa se enrolla** | Bordes opuestos y esquinas son vecinos (forma de toroide) |

**Lectura de un grupo:** se conservan solo las variables que **no cambian** dentro del grupo.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 5. Implicantes primos y esenciales

| Concepto | Definición |
|:---|:---|
| **Implicante** | Cualquier grupo válido de 1 (o X) |
| **Implicante primo** | Un grupo que ya **no se puede agrandar** más |
| **Primo esencial** | El **único** primo que cubre algún 1 específico; debe estar en la solución |

$$\text{Solución mínima} = \text{primos esenciales} + \text{los primos mínimos necesarios para cubrir los 1 restantes}$$

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 6. Procedimiento para la SOP mínima

1. **Colocar** los 1, 0 y X en sus celdas.
2. **Agrupar** los grupos más grandes posibles.
3. **Elegir** los primos esenciales.
4. **Cubrir** los 1 restantes con pocos grupos.
5. **Leer** un término por grupo.

### Cómo se lee cada grupo (SOP)

| La variable dentro del grupo... | Se escribe |
|:---|:---:|
| Es constante en **1** | $A$ |
| Es constante en **0** | $A'$ |
| **Cambia** | Desaparece |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 7. POS mínima con el mapa

Para obtener la función en **producto de sumas** se **agrupan los 0**.

### Lectura invertida (POS)

| La variable dentro del grupo... | Se escribe |
|:---|:---:|
| Es constante en **0** | $A$ |
| Es constante en **1** | $A'$ |
| **Cambia** | Desaparece |

**Ejemplo de agrupación de 0:**

- Fila $AB = 11 \rightarrow (A' + B')$
- Columna $CD = 11 \rightarrow (C' + D')$

> [!IMPORTANT]
> **¿SOP o POS?** Construir ambas formas y contar los literales. Elegir la de **menor costo** en literales, compuertas y circuitos integrados.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 8. Condiciones indiferentes

Un dígito **BCD** usa 4 bits, pero solo 10 de las 16 combinaciones posibles (0000 a 1001). Las combinaciones **1010 a 1111 (10 a 15) nunca ocurren**.

¿Qué debe hacer el circuito con ellas? **Lo que más convenga.** Son las **condiciones indiferentes** (*don't cares*), marcadas con **X** en el mapa.

**Notación:**

$$F = \Sigma m(\text{minitérminos}) + d(\text{indiferentes})$$

### Cómo usar las X en el mapa

| Uso | Cuándo |
|:---|:---|
| **Como 1** | Si agranda un grupo; el término queda más corto |
| **Como 0** | Si no ayuda, se ignora; ninguna X está obligada a quedar cubierta |
| **Nunca sola** | Un grupo formado solo por X agrega un término inútil |

### Impacto de las X

| | Expresión | Literales |
|:---|:---|:---:|
| **Sin usar X** | $AB'C' + A'BD + A'BC$ | **9** |
| **Usando X como 1 convenientes** | $A + BD + BC$ | **5** |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 9. Universalidad de NAND y NOR

**Con NAND se construye todo:**

| Función | Cómo se logra con NAND | Expresión |
|:---:|:---|:---:|
| **NOT** | Unir las dos entradas | $(A \cdot A)' = A'$ |
| **AND** | Negar la salida de un NAND | $((A \cdot B)')' = A \cdot B$ |
| **OR** | Negar las entradas antes del NAND | $(A' \cdot B')' = A + B$ |

Si NOT, AND y OR se construyen con NAND, **cualquier función** también se puede. Lo mismo aplica para **NOR** (el caso dual).

### La forma mínima decide la familia

| Forma mínima | Implementación |
|:---:|:---:|
| **SOP** | NAND-NAND |
| **POS** | NOR-NOR |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 10. De SOP a NAND-NAND

Ejemplo con $F = A + BD + BC$:

| Paso | Acción | Resultado |
|:---:|:---|:---|
| **1** | SOP mínima | $F = A + BD + BC$ |
| **2** | Doble negación (no cambia nada) | $F = ((A + BD + BC)')'$ |
| **3** | De Morgan sobre la negación interna | $F = (A' \cdot (BD)' \cdot (BC)')'$ |
| **4** | Implementación | Cada paréntesis es una compuerta NAND |

**Detalles de la implementación:**

- **Literal solo:** entra complementado (una NAND usada como inversor, por ejemplo $A'$).
- **Dos niveles:** el retardo es el mismo que el de la red AND-OR.
- **Una sola familia:** toda la red sale de chips **7400** y **7410**, lo que optimiza inventario y costo.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 11. Costo e integración

- El ahorro principal viene de usar las **X** en el mapa de Karnaugh.
- Convertir a NAND **no agrega chips** (a menudo los reduce, por usar una sola familia).

### Proyecto integrador

| Semana | Etapa |
|:---:|:---|
| **6** | Bloque combinacional clásico |
| **11** | Se agrega memoria (flip-flops) |
| **16** | Descrito en VHDL y configurado en la Basys 3; se contrasta la solución clásica con la moderna |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 12. Ideas clave de la semana

> [!TIP]
> - Dos celdas vecinas en el mapa difieren en **una variable**, y esa variable desaparece (adyacencia).
> - El mapa usa código **Gray** (00, 01, 11, 10) y **se enrolla**: bordes y esquinas son vecinos.
> - Grupos de $2^k$ celdas, rectangulares y **lo más grandes posible**.
> - **SOP:** agrupar los 1. **POS:** agrupar los 0 con lectura invertida.
> - Las **X** (indiferentes) se usan como 1 solo si ayudan; el mayor ahorro sale de ellas.
> - **NAND** y **NOR** son universales: SOP mínima $\rightarrow$ NAND-NAND, POS mínima $\rightarrow$ NOR-NOR.

---

[🏠 Índice](README.md) · [⬅️ Semana 2](semana-02.md) · [Semana 4 ➡️](semana-04.md)
