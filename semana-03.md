[🏠 Índice](README.md) · [⬅️ Semana 2](semana-02.md) · [Semana 4 ➡️](semana-04.md)

---

# Semana 3 · Mapas de Karnaugh, indiferentes y NAND/NOR

> **Unidad 1:** Análisis de lógica combinacional con diseño clásico · **Diseño de Circuitos** · Computación, 2.º ciclo A · UNL

La semana tuvo dos sesiones:

| Sesión | Tema | Secciones |
|:---:|:---|:---:|
| **1** | Mapas de Karnaugh: expresiones mínimas en SOP y POS | [2](#2-teorema-de-adyacencia-y-código-gray) a [7](#7-pos-mínima-con-el-mapa) |
| **2** | Indiferentes y NAND/NOR: minimizar e implementar con una sola familia | [8](#8-condiciones-indiferentes) a [10](#10-de-sop-a-nand-nand) |

## Contenido de la semana

1. [Objetivos](#1-objetivos)
2. [Teorema de adyacencia y código Gray](#2-teorema-de-adyacencia-y-código-gray)
3. [Mapas de 3 y 4 variables](#3-mapas-de-3-y-4-variables)
4. [Reglas para agrupar](#4-reglas-para-agrupar)
5. [Implicantes primos y esenciales](#5-implicantes-primos-y-esenciales)
6. [Procedimiento para la SOP mínima](#6-procedimiento-para-la-sop-mínima)
7. [POS mínima con el mapa](#7-pos-mínima-con-el-mapa)
8. [Condiciones indiferentes](#8-condiciones-indiferentes)
9. [Universalidad de NAND y NOR](#9-universalidad-de-nand-y-nor)
10. [De SOP a NAND-NAND](#10-de-sop-a-nand-nand)
11. [Ejercicios propuestos](#11-ejercicios-propuestos)
12. [Ideas clave](#12-ideas-clave)

---

## 1. Objetivos

- **Sesión 1:** obtener la expresión mínima en **SOP** y en **POS** de funciones de 3 y 4 variables con mapas de Karnaugh, justificando cada agrupación.
- **Sesión 2:** minimizar una función de 4 variables con **condiciones indiferentes** e implementarla solo con compuertas **NAND**, cuantificando el ahorro en compuertas y circuitos integrados.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 2. Teorema de adyacencia y código Gray

La clave del mapa es el teorema de adyacencia:

$$X \cdot Y + X \cdot Y' = X$$

Si dos minitérminos difieren en **una sola variable**, esa variable desaparece. Por ejemplo, $m_4\,(AB'C') + m_5\,(AB'C) \rightarrow AB'$ (desaparece $C$).

**¿Por qué el orden 00, 01, 11, 10?** En binario normal, de $01 \rightarrow 10$ cambian 2 bits. En **código Gray** cada celda es vecina lógica de la siguiente, y la última de la primera:

$$00 \rightarrow 01 \rightarrow 11 \rightarrow 10 \rightarrow 00$$

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 3. Mapas de 3 y 4 variables

### 3 variables

Filas: $A$. Columnas: $BC$ en código Gray.

| $A \backslash BC$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **0** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **1** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |

> [!WARNING]
> $m_3$ va **antes** que $m_2$.

### 4 variables

Filas: $AB$. Columnas: $CD$. Ambas en código Gray.

| $AB \backslash CD$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **00** | 0 | 1 | 3 | 2 |
| **01** | 4 | 5 | 7 | 6 |
| **11** | 12 | 13 | 15 | 14 |
| **10** | 8 | 9 | 11 | 10 |

Los bordes y las esquinas **también son vecinos**: las 4 esquinas ($m_0, m_2, m_8, m_{10}$) forman el grupo $B'D'$.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 4. Reglas para agrupar

| Regla | Detalle |
|:---:|:---|
| **1. Tamaño $2^k$** | 1, 2, 4, 8 o 16 celdas. Un grupo de $2^k$ celdas elimina $k$ variables |
| **2. Rectangular** | Sin formas en L ni diagonales |
| **3. Lo más grande posible** | Los grupos grandes dan términos más cortos |
| **4. El mapa se enrolla** | Bordes opuestos y esquinas son vecinos (forma de toroide) |

**Lectura de un grupo:** se conservan solo las variables que **no cambian** dentro del grupo.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 5. Implicantes primos y esenciales

| Concepto | Definición |
|:---|:---|
| **Implicante** | Cualquier grupo válido de 1 (o X) |
| **Implicante primo** | Un grupo que ya no se puede agrandar |
| **Primo esencial** | El **único** primo que cubre algún 1 específico; debe estar en la solución |

$$\text{Solución mínima} = \text{primos esenciales} + \text{los primos mínimos necesarios para cubrir el resto de 1}$$

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 6. Procedimiento para la SOP mínima

1. **Colocar** los 1, 0 y X en sus celdas.
2. **Agrupar** los grupos más grandes.
3. **Elegir** los primos esenciales.
4. **Cubrir** los 1 restantes con pocos grupos.
5. **Leer** un término por grupo.

| Variable dentro del grupo | Se escribe |
|:---|:---:|
| Constante en **1** | $A$ |
| Constante en **0** | $A'$ |
| **Cambia** | Desaparece |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 7. POS mínima con el mapa

Para obtener el **producto de sumas** se **agrupan los 0** y la lectura se invierte:

| Variable dentro del grupo | Se escribe |
|:---|:---:|
| Constante en **0** | $A$ |
| Constante en **1** | $A'$ |
| **Cambia** | Desaparece |

Ejemplo de grupos de 0: la fila $AB = 11$ da $(A' + B')$ y la columna $CD = 11$ da $(C' + D')$.

> [!IMPORTANT]
> **¿SOP o POS?** Construir ambas formas, contar los literales y elegir la de **menor costo** en literales, compuertas y circuitos integrados.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 8. Condiciones indiferentes

Un dígito **BCD** usa 4 bits, pero solo 10 de las 16 combinaciones (0000 a 1001). Las combinaciones **1010 a 1111 (10 a 15) nunca ocurren**, y el circuito puede hacer con ellas lo que más convenga: son las **condiciones indiferentes** (*don't cares*), marcadas con **X** en el mapa.

$$F = \Sigma m(\text{minitérminos}) + d(\text{indiferentes})$$

| Uso de la X | Cuándo |
|:---|:---|
| **Como 1** | Si agranda un grupo; el término queda más corto |
| **Como 0** | Si no ayuda, se ignora; ninguna X está obligada a quedar cubierta |
| **Nunca sola** | Un grupo formado solo por X agrega un término inútil |

**Impacto:** sin usar X, una función puede resultar en 9 literales ($AB'C' + A'BD + A'BC$); usando las X como 1 convenientes baja a 5 literales ($A + BD + BC$).

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 9. Universalidad de NAND y NOR

**Con NAND se construye todo:**

| Función | Cómo se logra con NAND | Expresión |
|:---:|:---|:---:|
| **NOT** | Unir las dos entradas | $(A \cdot A)' = A'$ |
| **AND** | Negar la salida de un NAND | $((A \cdot B)')' = A \cdot B$ |
| **OR** | Negar las entradas antes del NAND | $(A' \cdot B')' = A + B$ |

Si NOT, AND y OR se construyen con NAND, **cualquier función** también. Lo mismo aplica a **NOR** (el caso dual).

La forma mínima elegida decide la familia que conviene:

| Forma mínima | Implementación |
|:---:|:---:|
| **SOP** | NAND-NAND |
| **POS** | NOR-NOR |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 10. De SOP a NAND-NAND

En cuatro pasos, con la SOP mínima $F = A + BD + BC$:

| Paso | Acción | Resultado |
|:---:|:---|:---|
| **1** | SOP mínima | $F = A + BD + BC$ |
| **2** | Doble negación (no cambia nada) | $F = ((A + BD + BC)')'$ |
| **3** | De Morgan sobre la negación interna | $F = (A' \cdot (BD)' \cdot (BC)')'$ |
| **4** | Implementación | Cada paréntesis es una compuerta NAND |

- **Literal solo:** entra complementado (una NAND usada como inversor, por ejemplo $A'$).
- **Dos niveles:** el retardo es el mismo que el de la red AND-OR.
- **Una familia:** toda la red sale de chips 7400 y 7410, lo que optimiza inventario y costo.
- **Costo:** el ahorro principal viene de usar las **X**; convertir a NAND no agrega chips (a menudo los reduce).

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 11. Ejercicios propuestos

**Ejercicio 1.** Minimiza con un mapa de 3 variables $F = \Sigma m(0,1,2,3,6,7)$ en SOP.

<details>
<summary>Ver solución</summary>

| $A \backslash BC$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **0** | 1 | 1 | 1 | 1 |
| **1** | 0 | 0 | 1 | 1 |

- Grupo de 4: la fila $A = 0$ ($m_0, m_1, m_3, m_2$) da $A'$.
- Grupo de 4: las columnas $11$ y $10$ ($m_3, m_2, m_7, m_6$) dan $B$.

$$F = A' + B$$

</details>

**Ejercicio 2.** Con el mismo mapa del ejercicio 1, agrupa los **0** y obtén la POS mínima. ¿Coincide con la SOP?

<details>
<summary>Ver solución</summary>

Los 0 están en $m_4$ y $m_5$ (fila $A = 1$, columnas $00$ y $01$). En el grupo $A$ es constante en 1 (se escribe $A'$), $B$ es constante en 0 (se escribe $B$) y $C$ cambia (desaparece):

$$F = (A' + B)$$

Es la misma función que $A' + B$ del ejercicio 1: ambas formas son equivalentes.

</details>

**Ejercicio 3.** Minimiza en SOP $F = \Sigma m(0,1,2,5,8,9,10)$ con un mapa de 4 variables.

<details>
<summary>Ver solución</summary>

| $AB \backslash CD$ | 00 | 01 | 11 | 10 |
|:---:|:---:|:---:|:---:|:---:|
| **00** | 1 | 1 | 0 | 1 |
| **01** | 0 | 1 | 0 | 0 |
| **11** | 0 | 0 | 0 | 0 |
| **10** | 1 | 1 | 0 | 1 |

- Esquinas $m_0, m_2, m_8, m_{10}$: $B'D'$ (esencial por $m_2$).
- $m_0, m_1, m_8, m_9$ (el mapa se enrolla entre las filas $00$ y $10$): $B'C'$ (esencial por $m_9$).
- $m_1, m_5$: $A'C'D$ (esencial por $m_5$).

$$F = B'D' + B'C' + A'C'D$$

</details>

**Ejercicio 4.** Una señal $F$ debe valer 1 cuando un dígito BCD es **impar** ($1,3,5,7,9$). Minimízala en SOP sin usar X y luego usando las X de las combinaciones 10 a 15. ¿Cuántos literales se ahorran?

<details>
<summary>Ver solución</summary>

$F = \Sigma m(1,3,5,7,9) + d(10,11,12,13,14,15)$.

- **Sin X:** $m_1, m_3, m_5, m_7$ forman $A'D$ y $m_1, m_9$ forman $B'C'D$, de modo que $F = A'D + B'C'D$ (**5 literales**).
- **Con X:** todas las celdas con $D = 1$ son 1 o X ($m_{11}, m_{13}, m_{15}$ son X), así que un solo grupo de 8 da $F = D$ (**1 literal**).

Ahorro: 4 literales.

</details>

**Ejercicio 5.** Implementa $F = A + B'C$ usando solo compuertas NAND de 2 entradas. ¿Cuántas compuertas y cuántos circuitos 7400 necesitas?

<details>
<summary>Ver solución</summary>

$F = ((A + B'C)')' = (A' \cdot (B'C)')'$

1. $N_1 = \text{NAND}(B, B) = B'$
2. $N_2 = \text{NAND}(N_1, C) = (B'C)'$
3. $N_3 = \text{NAND}(A, A) = A'$
4. $N_4 = \text{NAND}(N_3, N_2) = F$

Son **4 compuertas NAND**, que caben en **un solo 7400** (trae 4).

</details>

**Ejercicio 6.** Implementa la POS $F = (A + B)(A' + C)$ solo con compuertas NOR de 2 entradas.

<details>
<summary>Ver solución</summary>

Por De Morgan, $F = ((A + B)' + (A' + C)')'$.

1. $N_1 = \text{NOR}(A, B) = (A + B)'$
2. $N_2 = \text{NOR}(A, A) = A'$
3. $N_3 = \text{NOR}(N_2, C) = (A' + C)'$
4. $N_4 = \text{NOR}(N_1, N_3) = F$

Son 4 compuertas NOR (un integrado 7402).

</details>

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 12. Ideas clave

> [!TIP]
> - Dos celdas vecinas del mapa difieren en **una variable**, y esa variable desaparece.
> - El mapa usa código **Gray** y **se enrolla**: bordes y esquinas son vecinos.
> - Grupos de $2^k$ celdas, rectangulares y **lo más grandes posible**.
> - **SOP:** agrupar los 1. **POS:** agrupar los 0 con lectura invertida.
> - Las **X** se usan como 1 solo si ayudan; de ellas sale el mayor ahorro.
> - **NAND** y **NOR** son universales: SOP mínima $\rightarrow$ NAND-NAND, POS mínima $\rightarrow$ NOR-NOR.

---

[🏠 Índice](README.md) · [⬅️ Semana 2](semana-02.md) · [Semana 4 ➡️](semana-04.md)
