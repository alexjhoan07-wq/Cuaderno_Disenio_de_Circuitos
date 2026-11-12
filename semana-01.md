[🏠 Índice](../README.md) · [Semana 2 ➡️](semana-02.md)

---

# Semana 1 · Sistemas numéricos, códigos y compuertas lógicas

> **Unidad 1:** Análisis de lógica combinacional con diseño clásico · **Diseño de Circuitos** · Computación, 2.º ciclo · UNL

## Contenido de la semana

1. [Objetivo](#1-objetivo)
2. [Representación binaria](#2-representación-binaria)
3. [Sistemas numéricos](#3-sistemas-numéricos)
4. [Aritmética binaria y complemento a dos](#4-aritmética-binaria-y-complemento-a-dos)
5. [Códigos BCD, Gray y ASCII](#5-códigos-bcd-gray-y-ascii)
6. [Compuertas lógicas](#6-compuertas-lógicas)
7. [Circuitos integrados y datasheet](#7-circuitos-integrados-y-datasheet)
8. [Ejercicios propuestos](#8-ejercicios-propuestos)
9. [Ideas clave](#9-ideas-clave)

---

## 1. Objetivo

Convertir números entre decimal, binario, octal y hexadecimal; representar negativos en complemento a dos; reconocer los códigos BCD, Gray y ASCII; y leer las 7 compuertas lógicas ubicándolas en un circuito integrado 74LS.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 2. Representación binaria

El computador usa solo **0 y 1** porque un transistor es un interruptor controlado por voltaje. Con solo dos estados es fácil distinguir uno del otro, aunque la señal llegue con ruido.

| Valor | Interruptor | Corriente | Voltaje |
|:---:|:---|:---|:---|
| **0** | Abierto | No pasa | Bajo |
| **1** | Cerrado | Pasa | Alto |

### Un «1» es un rango de voltaje (familia 74LS a 5 V)

| Nivel | El chip **entrega** | El chip **lee** |
|:---|:---:|:---:|
| **1 lógico** | $\ge 2.7\text{ V}$ | $\ge 2.0\text{ V}$ |
| **Zona indefinida** | — | $0.8\text{ V}$ a $2.0\text{ V}$ |
| **0 lógico** | $\le 0.5\text{ V}$ | $\le 0.8\text{ V}$ |

> [!WARNING]
> En la **zona indefinida** el chip no garantiza nada (puede leer 0 o 1). Causa típica: una **entrada sin conectar**.

### Margen de ruido

| Margen | Cálculo | Significado |
|:---|:---:|:---|
| **Alto** | $2.7 - 2.0 = 0.7\text{ V}$ | Un 1 puede perder hasta $0.7\text{ V}$ y se sigue leyendo como 1 |
| **Bajo** | $0.8 - 0.5 = 0.3\text{ V}$ | Un 0 puede subir hasta $0.3\text{ V}$ y se sigue leyendo como 0 |

*Falla típica:* sale un 0 de $0.5\text{ V}$, un motor cercano induce $0.4\text{ V}$, llegan $0.9\text{ V}$ (zona indefinida) y hay error.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 3. Sistemas numéricos

**Sistema posicional:** la posición da el valor, y cada posición vale una potencia de la base (en decimal, $555 = 500 + 50 + 5$). Los números romanos **no** lo son: en XV la V vale 5 esté donde esté.

### Binario

Las posiciones valen potencias de 2: $128, 64, 32, 16, 8, 4, 2, 1$.

- **Bit:** un dígito binario (*binary digit*). **Byte:** 8 bits.
- Binario a decimal: $1011_2 = 8 + 0 + 2 + 1 = 11_{10}$.

### Decimal a binario

Dividir entre 2 y leer los restos **de abajo hacia arriba**. Para $25$ los restos son $1, 0, 0, 1, 1$, de modo que $25_{10} = 11001_2$.

### Hexadecimal y octal: abreviaturas del binario

| Sistema | Cómo se obtiene | Detalle |
|:---|:---|:---|
| **Hexadecimal** | Grupos de **4 bits** desde la derecha | Dígitos 0–9 y A=10, B=11, C=12, D=13, E=14, F=15 |
| **Octal** | Grupos de **3 bits** desde la derecha | Si faltan bits a la izquierda, se completa con ceros |

Ejemplo: $157_{10} = 1001\ 1101_2 = 9D_{16}$ y también $= 010\ 011\ 101_2 = 235_8$.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 4. Aritmética binaria y complemento a dos

### Suma binaria

Igual que en decimal, con acarreo:

$$0+0=0 \qquad 0+1=1 \qquad 1+0=1 \qquad 1+1=10 \qquad 1+1+1=11$$

En $1+1=10$ se escribe 0 y se lleva 1. Ejemplo: $1011_2\,(11) + 0110_2\,(6) = 10001_2\,(17)$.

### Números con signo: complemento a dos

El **bit de la izquierda** indica el signo: 0 es positivo y 1 es negativo.

| Bits | Positivos | Negativos | Rango |
|:---:|:---:|:---:|:---:|
| **4** | $0000$ a $0111$ ($0$ a $7$) | $1000$ a $1111$ ($-8$ a $-1$) | $-8$ a $+7$ |
| **8** | | | $-128$ a $+127$ |

**Obtener el negativo:** invertir todos los bits y sumar 1. Ejemplo: $3 = 0011 \rightarrow 1100 \rightarrow 1101 = -3$.

**Ventaja:** no hace falta un circuito restador, basta el mismo sumador más inversores:

$$A - B = A + (-B)$$

El bit que sobra a la izquierda se descarta.

### Desbordamiento (overflow)

El resultado debe **caber** en los bits asignados.

> [!WARNING]
> $7 + 1$ en 4 bits con signo da $0111 + 0001 = 1000 = -8$. Dos positivos dan un negativo porque el resultado no cabe.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 5. Códigos BCD, Gray y ASCII

| Código | Qué hace | Uso | Detalle |
|:---|:---|:---|:---|
| **BCD** | Cada **dígito decimal** en 4 bits ($157 = 0001\ 0101\ 0111$) | Mostrar números en displays | Las combinaciones $1010$ a $1111$ no se usan |
| **Gray** | Entre vecinos cambia **un solo bit** | Encoders rotativos y mapas de Karnaugh | Se construye con el método del espejo. Previene errores en sensores al transitar: de 3 a 4, en binario $011 \rightarrow 100$ cambian 3 bits y en Gray $010 \rightarrow 110$ cambia 1 |
| **ASCII** | Tabla de **letras a números** (7 bits = 128 caracteres) | Texto | «A» $= 65_{10} = 01000001_2 = \text{0x41}$ |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 6. Compuertas lógicas

| Compuerta | Expresión | Regla |
|:---:|:---:|:---|
| **NOT** | $Y = A'$ | Invierte |
| **AND** | $Y = A \cdot B$ | 1 solo si **todas** valen 1 |
| **OR** | $Y = A + B$ | 1 si **al menos una** vale 1 |
| **NAND** | $Y = (A \cdot B)'$ | 0 solo si todas valen 1. Con solo NAND se puede construir cualquier circuito |
| **NOR** | $Y = (A + B)'$ | 1 solo si **ninguna** vale 1. El círculo en la salida significa «negado» |
| **XOR** | $Y = A \oplus B$ | 1 si las entradas son **distintas** (luz de escalera con dos interruptores) |
| **XNOR** | $Y = (A \oplus B)'$ | 1 si las entradas son **iguales** |

**Normas de símbolos:** ANSI/IEEE (formas distintivas clásicas) e IEC (rectángulos con símbolo: `1`, `&`, `≥1`, `=1`).

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 7. Circuitos integrados y datasheet

**74LS08:** 4 compuertas AND de 2 entradas en encapsulado **DIP-14** (dos filas de 7 pines).

- Muesca arriba, pin 1 arriba a la izquierda, numeración en sentido antihorario.
- **Pin 14** = $+5\text{ V}$ (VCC). **Pin 7** = tierra (GND).

**Nomenclatura de `SN74LS08N`:**

| Parte | Significado |
|:---:|:---|
| **SN** | Fabricante (Texas Instruments) |
| **74** | Uso comercial |
| **LS** | Tecnología TTL (HC sería CMOS) |
| **08** | Función (AND) |
| **N** | Encapsulado DIP |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 8. Ejercicios propuestos

Intenta resolverlos primero y luego despliega la solución.

**Ejercicio 1.** Convierte $83_{10}$ a binario, octal y hexadecimal.

<details>
<summary>Ver solución</summary>

$83 = 64 + 16 + 2 + 1 = 1010011_2$.

- Octal: $001\ 010\ 011 = 123_8$.
- Hexadecimal: $0101\ 0011 = 53_{16}$.

</details>

**Ejercicio 2.** Convierte $2F_{16}$ a binario y a decimal.

<details>
<summary>Ver solución</summary>

$2 = 0010$ y $F = 1111$, así que $2F_{16} = 00101111_2 = 101111_2$.

Decimal: $2 \cdot 16 + 15 = 47$.

</details>

**Ejercicio 3.** Representa $-25$ en complemento a dos con 8 bits.

<details>
<summary>Ver solución</summary>

$25 = 00011001$. Invertido: $11100110$. Sumando 1: $11100111$.

Comprobación: $-128 + 64 + 32 + 7 = -25$.

</details>

**Ejercicio 4.** Calcula $6 - 4$ en 4 bits usando complemento a dos.

<details>
<summary>Ver solución</summary>

$4 = 0100 \rightarrow 1011 \rightarrow 1100 = -4$.

$0110 + 1100 = 1\,0010$. Se descarta el bit sobrante: $0010 = 2$.

</details>

**Ejercicio 5.** En 4 bits con signo, ¿hay overflow al calcular $5 + 4$?

<details>
<summary>Ver solución</summary>

$0101 + 0100 = 1001 = -7$. **Sí hay overflow:** el resultado esperado, $9$, no cabe en el rango $-8$ a $+7$.

</details>

**Ejercicio 6.** Escribe $462$ en BCD. ¿Qué carácter ASCII es $\text{0x43}$?

<details>
<summary>Ver solución</summary>

BCD: $4 = 0100$, $6 = 0110$, $2 = 0010$, de modo que $462 = 0100\ 0110\ 0010$.

Como $\text{0x41}$ es «A», $\text{0x43}$ es «**C**».

</details>

**Ejercicio 7.** Un chip 74LS entrega un 1 de $2.7\text{ V}$ y el ruido le resta $0.9\text{ V}$. ¿Qué lee el chip receptor?

<details>
<summary>Ver solución</summary>

Llegan $2.7 - 0.9 = 1.8\text{ V}$, que es menor que $2.0\text{ V}$ y mayor que $0.8\text{ V}$: cae en la **zona indefinida**, no se garantiza la lectura. El ruido superó el margen alto de $0.7\text{ V}$.

</details>

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 9. Ideas clave

> [!TIP]
> - Un 1 o un 0 lógico es un **rango de voltaje**; los márgenes de ruido protegen la lectura.
> - Hexadecimal y octal son abreviaturas del binario (grupos de 4 y de 3 bits).
> - Complemento a dos: **invertir y sumar 1**; permite restar con el mismo sumador.
> - Siempre verificar que el resultado **quepa** en los bits.
> - BCD para displays, Gray para sensores y encoders, ASCII para texto.
> - Las 7 compuertas: NOT, AND, OR, NAND, NOR, XOR, XNOR.

---

[🏠 Índice](../README.md) · [Semana 2 ➡️](semana-02.md)
