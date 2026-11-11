[🏠 Índice](../README.md) · [Semana 2 ➡️](semana-02.md)

---

# Semana 1 · Sistemas numéricos, códigos y compuertas lógicas

> **Unidad 1:** Análisis de lógica combinacional con diseño clásico · **Diseño de Circuitos** · Computación, 2.º ciclo · UNL

## Contenido de la semana

1. [Objetivos de la semana](#1-objetivos-de-la-semana)
2. [Representación binaria](#2-representación-binaria)
3. [Sistemas numéricos](#3-sistemas-numéricos)
4. [Aritmética binaria y complemento a dos](#4-aritmética-binaria-y-complemento-a-dos)
5. [Códigos BCD, Gray y ASCII](#5-códigos-bcd-gray-y-ascii)
6. [Compuertas lógicas](#6-compuertas-lógicas)
7. [Circuitos integrados y datasheet](#7-circuitos-integrados-y-datasheet)
8. [Tareas y próxima sesión](#8-tareas-y-próxima-sesión)
9. [Ideas clave de la semana](#9-ideas-clave-de-la-semana)

---

## 1. Objetivos de la semana

Al terminar la semana soy capaz de:

- **Convertir** números entre decimal, binario, octal y hexadecimal.
- **Representar** números negativos en complemento a dos.
- **Reconocer** los códigos BCD, Gray y ASCII y para qué sirve cada uno.
- **Leer** las 7 compuertas lógicas y ubicarlas en un circuito integrado 74LS.

> [!NOTE]
> Aporta al **R1:** diseña e implementa circuitos combinacionales básicos utilizando técnicas de diseño clásico.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 2. Representación binaria

### ¿Por qué el computador usa solo 0 y 1?

| Valor | Equivale a | Eléctricamente |
|:---:|:---|:---|
| **0** | Interruptor abierto | No pasa corriente, voltaje bajo |
| **1** | Interruptor cerrado | Pasa corriente, voltaje alto |

Un **transistor** es un interruptor controlado por voltaje. Con solo dos estados es fácil distinguir uno del otro, aunque la señal llegue con ruido.

### Un «1» no es un voltaje exacto: es un rango (familia 74LS a 5 V)

| Nivel | Lo que **sale** del chip | Lo que el chip **lee** |
|:---|:---:|:---:|
| **1 lógico** | $\ge 2.7\text{ V}$ | $\ge 2.0\text{ V}$ |
| **Zona indefinida** | — | entre $0.8\text{ V}$ y $2.0\text{ V}$ |
| **0 lógico** | $\le 0.5\text{ V}$ | $\le 0.8\text{ V}$ |

> [!WARNING]
> En la **zona indefinida** el chip no garantiza nada (puede leer 0 o 1). Causa típica: una **entrada sin conectar**.

### Margen de ruido: el colchón de seguridad

- **Margen alto:** $2.7\text{ V} - 2.0\text{ V} = 0.7\text{ V}$. Un 1 puede perder hasta $0.7\text{ V}$ en el camino y se sigue leyendo como 1.
- **Margen bajo:** $0.8\text{ V} - 0.5\text{ V} = 0.3\text{ V}$. Un 0 puede subir hasta $0.3\text{ V}$ por ruido y se sigue leyendo como 0.

**Ejemplo de falla:** sale un 0 de $0.5\text{ V}$ y un motor cercano induce $0.4\text{ V}$ de ruido. Llegan $0.9\text{ V}$, que cae en la zona indefinida y provoca un error.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 3. Sistemas numéricos

### Sistema posicional

La **posición** da el valor: cada posición vale una potencia de la base. En decimal, $555 = 500 + 50 + 5$.

Contraejemplo: los números romanos no son posicionales (en XV la V vale 5 esté donde esté).

### Binario

Las posiciones valen potencias de 2:

| Posición | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Valor** | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

- **Bit:** un dígito binario (*binary digit*). **Byte:** 8 bits.
- Ejemplo: $1011_2 = 8 + 0 + 2 + 1 = 11_{10}$

### Decimal a binario: dividir entre 2 y leer los restos de abajo hacia arriba

Ejemplo con $25_{10}$:

| División | Cociente | Resto |
|:---:|:---:|:---:|
| $25 \div 2$ | 12 | **1** |
| $12 \div 2$ | 6 | **0** |
| $6 \div 2$ | 3 | **0** |
| $3 \div 2$ | 1 | **1** |
| $1 \div 2$ | 0 | **1** |

Leyendo los restos de abajo hacia arriba: $25_{10} = 11001_2$

### Hexadecimal y octal: abreviaturas del binario

| Sistema | Cómo se obtiene | Ejemplo con $157_{10} = 10011101_2$ |
|:---|:---|:---|
| **Hexadecimal** | Grupos de **4 bits** desde la derecha | $1001\ 1101_2 = 9D_{16}$ |
| **Octal** | Grupos de **3 bits** desde la derecha | $010\ 011\ 101_2 = 235_8$ |

- Hexadecimal usa los dígitos 0–9 y las letras A=10, B=11, C=12, D=13, E=14, F=15.
- En octal, si faltan bits en el grupo de la izquierda, se completa con **ceros a la izquierda**.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 4. Aritmética binaria y complemento a dos

### Suma binaria

Igual que en decimal, con acarreo:

| Operación | Resultado |
|:---:|:---|
| $0+0$ | $0$ |
| $0+1$ | $1$ |
| $1+0$ | $1$ |
| $1+1$ | $10$ (escribo 0, llevo 1) |
| $1+1+1$ | $11$ (escribo 1, llevo 1) |

Ejemplo: $1011_2\ (11) + 0110_2\ (6) = 10001_2\ (17)$

### Números con signo: complemento a dos

¿Cómo representar negativos sin un símbolo $-$? Con el **bit de la izquierda** como indicador de signo.

| En 4 bits | Representa | Bit izquierdo |
|:---:|:---:|:---:|
| $0000$ a $0111$ | $0$ a $+7$ (positivos) | 0 |
| $1000$ a $1111$ | $-8$ a $-1$ (negativos) | 1 |

| Cantidad de bits | Rango representable |
|:---:|:---:|
| 4 bits | $-8$ a $+7$ |
| 8 bits | $-128$ a $+127$ |

**Cómo obtener el negativo:** invertir todos los bits y sumar 1.

> **Ejemplo:** $3 = 0011 \rightarrow$ invertido $1100 \rightarrow +1 \rightarrow 1101 = -3$

**La gran ventaja:** para restar no se necesita un circuito restador, basta el mismo sumador más inversores:

$$A - B = A + (-B)$$

El bit que sobra a la izquierda se descarta.

### Desbordamiento (overflow)

El resultado debe **caber** en los bits asignados.

> [!WARNING]
> $7 + 1$ en 4 bits con signo: $0111 + 0001 = 1000 \rightarrow -8$ (**error**). Dos positivos dan un negativo porque el resultado no cabe.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 5. Códigos BCD, Gray y ASCII

| Código | Qué hace | Ejemplo | Dónde se usa |
|:---|:---|:---|:---|
| **BCD** (*Binary Coded Decimal*) | Cada **dígito decimal** en 4 bits | $157 = 0001\ 0101\ 0111$ | Mostrar números en displays |
| **Gray** | Entre vecinos cambia **un solo bit** | $3 \rightarrow 4$: en binario $011 \rightarrow 100$ cambian 3 bits; en Gray $010 \rightarrow 110$ cambia 1 | Encoders rotativos y mapas de Karnaugh |
| **ASCII** | Tabla de **letras a números** (7 bits = 128 caracteres) | «A» $= 65_{10} = 01000001_2 = \text{0x41}$ | Almacenar y transmitir texto |

**Detalles que no se me deben olvidar:**

- En **BCD** las combinaciones $1010$ a $1111$ **no se usan**.
- El código **Gray** previene errores en sensores durante la transición entre valores. Se construye con el **método del espejo**.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 6. Compuertas lógicas

| Compuerta | Expresión | Regla | Idea para recordar |
|:---:|:---:|:---|:---|
| **NOT** | $Y = A'$ | Invierte la entrada | Cambia 0 por 1 y 1 por 0 |
| **AND** | $Y = A \cdot B$ | 1 solo si **todas** valen 1 | "Todas" |
| **OR** | $Y = A + B$ | 1 si **al menos una** vale 1 | "Alguna" |
| **NAND** | $Y = (A \cdot B)'$ | 0 solo si todas valen 1 | Con solo NAND se construye cualquier circuito |
| **NOR** | $Y = (A + B)'$ | 1 solo si **ninguna** vale 1 | El círculo en la salida significa "negado" |
| **XOR** | $Y = A \oplus B$ | 1 si las entradas son **distintas** | Luz de escalera con dos interruptores |
| **XNOR** | $Y = (A \oplus B)'$ | 1 si las entradas son **iguales** | Detector de igualdad |

### Tabla de verdad resumida

| $A$ | $B$ | NOT $A$ | AND | OR | NAND | NOR | XOR | XNOR |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 | 1 |

### Normas de símbolos

- **ANSI / IEEE:** formas distintivas clásicas de cada compuerta.
- **IEC:** rectángulos con un símbolo dentro: `1` (NOT), `&` (AND), `≥1` (OR), `=1` (XOR).

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 7. Circuitos integrados y datasheet

### El 74LS08

Contiene **4 compuertas AND de 2 entradas** en un encapsulado **DIP-14** (dos filas de 7 pines).

| Elemento | Detalle |
|:---|:---|
| **Orientación** | Muesca arriba, pin 1 arriba a la izquierda, numeración en sentido antihorario |
| **Pin 14** | $+5\text{ V}$ (VCC) |
| **Pin 7** | Tierra (GND) |

### Nomenclatura: cómo leer `SN74LS08N`

| Parte | Significado |
|:---:|:---|
| **SN** | Fabricante (Texas Instruments) |
| **74** | Uso comercial |
| **LS** | Tecnología TTL (HC sería CMOS) |
| **08** | Función (compuertas AND) |
| **N** | Encapsulado DIP |

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 8. Tareas y próxima sesión

- [ ] Lectura: apartado de **sistemas numéricos** del texto base.
- [ ] **Cuestionario 01**.
- [ ] Instalar **Logisim-evolution**.
- [ ] **Práctica APE 01**.

[⬆️ Volver al contenido](#contenido-de-la-semana)

---

## 9. Ideas clave de la semana

> [!TIP]
> - El computador usa 0 y 1 porque dos estados son fáciles de distinguir aunque haya ruido.
> - Un 1 o un 0 lógico es un **rango de voltaje**, no un valor exacto.
> - Hexadecimal y octal son **abreviaturas del binario** (grupos de 4 y de 3 bits).
> - Complemento a dos: **invertir y sumar 1**. Permite restar usando solo un sumador.
> - Siempre verificar que el resultado **quepa** en los bits (overflow).
> - BCD para displays, Gray para sensores, ASCII para texto.
> - Las 7 compuertas: NOT, AND, OR, NAND, NOR, XOR, XNOR.

---

[🏠 Índice](../README.md) · [Semana 2 ➡️](semana-02.md)
