# Unidad 1. Fundamentos de programación estructurada

La **programación estructurada** es una forma de escribir programas usando solo tres tipos de bloques: **secuencia** (una instrucción tras otra), **decisión** (`if`, `switch`) y **repetición** (`while`, `for`). Con esas tres piezas se puede resolver cualquier problema computable, y el código resultante es más fácil de leer, probar y corregir.

Antes de escribir código hay que entender dos cosas: **cómo guarda la computadora la información** y **cómo se describe la solución de un problema** (algoritmo).

## 1.1. Representación de la información

Una computadora solo entiende dos estados eléctricos: **hay corriente** o **no hay corriente**. Todo lo que ves en pantalla —números, texto, imágenes, música— está guardado internamente como combinaciones de esos dos estados, representados con `1` y `0`.

### 1.1.1. Qué es un bit

Un **bit** (*binary digit*, dígito binario) es la unidad mínima de información. Solo puede valer `0` o `1`.

| Valor | Interpretaciones posibles |
|---|---|
| `0` | apagado, falso, no, sin corriente |
| `1` | encendido, verdadero, sí, con corriente |

Con un solo bit puedes representar **2** cosas. Al juntar bits, las combinaciones se multiplican:

| Bits | Combinaciones | Fórmula |
|---|---|---|
| 1 | `0`, `1` → 2 | 2¹ |
| 2 | `00`, `01`, `10`, `11` → 4 | 2² |
| 3 | `000` … `111` → 8 | 2³ |
| 8 | 256 | 2⁸ |
| *n* | 2ⁿ | 2ⁿ |

> Regla clave: con **n bits** se pueden representar **2ⁿ valores distintos**.

### 1.1.2. Qué es un byte

Un **byte** es un grupo de **8 bits**. Es la unidad básica con la que se mide la memoria y el almacenamiento.

```
1 byte = 8 bits → 01000001
```

Con un byte se pueden representar 2⁸ = **256** valores (del 0 al 255). Eso alcanza para guardar un carácter de texto como `'A'`.

#### Múltiplos del byte

| Unidad | Símbolo | Equivale a | Ejemplo de uso |
|---|---|---|---|
| Kilobyte | KB | 1 024 bytes | Un documento de texto pequeño |
| Megabyte | MB | 1 024 KB | Una foto, una canción |
| Gigabyte | GB | 1 024 MB | Una película, la RAM de una PC |
| Terabyte | TB | 1 024 GB | Un disco duro |

> En informática los múltiplos van de **1 024 en 1 024** (2¹⁰), no de 1 000 en 1 000. Por eso un disco "de 1 TB" muestra menos espacio en Windows.

En C# puedes ver cuántos bytes ocupa cada tipo:

```csharp
Console.WriteLine(sizeof(byte));    // → 1
Console.WriteLine(sizeof(int));     // → 4
Console.WriteLine(sizeof(double));  // → 8
Console.WriteLine(sizeof(char));    // → 2  (C# usa Unicode de 16 bits)
```

### 1.1.3. Sistema binario (idea conceptual)

Usamos a diario el sistema **decimal** (base 10): tiene diez dígitos (0-9) y cada posición vale 10 veces más que la de su derecha.

```
  4    7    3
  │    │    └─ 3 × 10⁰ =   3
  │    └────── 7 × 10¹ =  70
  └─────────── 4 × 10² = 400
                        ─────
                         473
```

El sistema **binario** (base 2) funciona igual, pero solo tiene dos dígitos (0 y 1) y cada posición vale **el doble** que la de su derecha.

| Posición | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| Potencia | 2⁷ | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
| Valor | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

#### Binario → decimal

Se suman los valores de las posiciones donde hay un `1`:

```
Binario:   1   1   0   1
Valor:     8   4   2   1
           8 + 4 + 0 + 1 = 13
```

Por lo tanto `1101₂ = 13₁₀`.

| Decimal | Binario | | Decimal | Binario |
|---|---|---|---|---|
| 0 | `0` | | 8 | `1000` |
| 1 | `1` | | 9 | `1001` |
| 2 | `10` | | 10 | `1010` |
| 3 | `11` | | 11 | `1011` |
| 4 | `100` | | 12 | `1100` |
| 5 | `101` | | 13 | `1101` |
| 6 | `110` | | 14 | `1110` |
| 7 | `111` | | 15 | `1111` |

### 1.1.4. Conversión básica de números decimales a binarios

#### Método de divisiones sucesivas

1. Divide el número entre 2.
2. Anota el **residuo** (0 o 1).
3. Repite con el cociente hasta que llegue a 0.
4. Lee los residuos **de abajo hacia arriba**.

Ejemplo: convertir **45** a binario.

| División | Cociente | Residuo |
|---|---|---|
| 45 ÷ 2 | 22 | **1** ↑ |
| 22 ÷ 2 | 11 | **0** ↑ |
| 11 ÷ 2 | 5 | **1** ↑ |
| 5 ÷ 2 | 2 | **1** ↑ |
| 2 ÷ 2 | 1 | **0** ↑ |
| 1 ÷ 2 | 0 | **1** ↑ |

Leyendo de abajo hacia arriba: `45₁₀ = 101101₂`.

Comprobación: `32 + 0 + 8 + 4 + 0 + 1 = 45` ✔

#### Método de restas (tabla de potencias)

Se busca la potencia de 2 más grande que quepa en el número, se resta y se repite.

Ejemplo: **100**

```
100 - 64 = 36   → 1 en la posición 64
 36 - 32 =  4   → 1 en la posición 32
  4 < 16        → 0
  4 <  8        → 0
  4 -  4 =  0   → 1 en la posición 4
                → 0 en 2 y en 1

 64 32 16  8  4  2  1
  1  1  0  0  1  0  0   → 100₁₀ = 1100100₂
```

#### En C#

```csharp
int numero = 45;

string binario = Convert.ToString(numero, 2);
Console.WriteLine(binario);                    // → 101101

// Con 8 dígitos (un byte completo)
Console.WriteLine(binario.PadLeft(8, '0'));    // → 00101101

// De binario a decimal
int deVuelta = Convert.ToInt32("101101", 2);
Console.WriteLine(deVuelta);                   // → 45
```

El mismo algoritmo de divisiones, programado a mano:

```csharp
int n = 45;
string resultado = "";

while (n > 0)
{
    int residuo = n % 2;          // 0 o 1
    resultado = residuo + resultado; // se agrega a la IZQUIERDA (leer de abajo a arriba)
    n = n / 2;
}

Console.WriteLine(resultado);     // → 101101
```

### 1.1.5. Conversión básica de palabras a código ASCII y binario

Para guardar texto, cada carácter se asocia a un **número** mediante una tabla de códigos. La más conocida es **ASCII** (*American Standard Code for Information Interchange*), que usa 7 bits (128 caracteres) y normalmente se guarda en un byte.

#### Fragmento de la tabla ASCII

| Carácter | Decimal | Binario | | Carácter | Decimal | Binario |
|---|---|---|---|---|---|---|
| (espacio) | 32 | `00100000` | | `a` | 97 | `01100001` |
| `0` | 48 | `00110000` | | `b` | 98 | `01100010` |
| `1` | 49 | `00110001` | | `c` | 99 | `01100011` |
| `A` | 65 | `01000001` | | `h` | 104 | `01101000` |
| `B` | 66 | `01000010` | | `l` | 108 | `01101100` |
| `C` | 67 | `01000011` | | `o` | 111 | `01101111` |
| `H` | 72 | `01001000` | | `z` | 122 | `01111010` |

Observaciones útiles:

- Las mayúsculas van del 65 (`A`) al 90 (`Z`); las minúsculas del 97 (`a`) al 122 (`z`).
- Entre una mayúscula y su minúscula hay siempre **32** de diferencia.
- El carácter `'1'` (49) **no** es el número 1: son cosas distintas.

#### Ejemplo: la palabra "Hola"

| Carácter | ASCII (decimal) | Binario |
|---|---|---|
| `H` | 72 | `01001000` |
| `o` | 111 | `01101111` |
| `l` | 108 | `01101100` |
| `a` | 97 | `01100001` |

"Hola" ocupa **4 bytes** = **32 bits**:

```
01001000 01101111 01101100 01100001
```

#### En C#

Un `char` se puede convertir a su código numérico con un *cast* a `int`:

```csharp
char letra = 'A';
int codigo = (int)letra;
Console.WriteLine(codigo);              // → 65

char deNuevo = (char)97;
Console.WriteLine(deNuevo);             // → a
```

Programa que convierte una palabra completa:

```csharp
string palabra = "Hola";

foreach (char c in palabra)
{
    int ascii = (int)c;
    string binario = Convert.ToString(ascii, 2).PadLeft(8, '0');
    Console.WriteLine($"{c} → {ascii,3} → {binario}");
}

// → H →  72 → 01001000
// → o → 111 → 01101111
// → l → 108 → 01101100
// → a →  97 → 01100001
```

> ASCII solo cubre el inglés básico. Letras como `ñ` o `á` no están en ASCII de 7 bits; hoy se usa **Unicode** (UTF-8 / UTF-16), que incluye a ASCII como sus primeros 128 caracteres. Por eso en C# un `char` ocupa 2 bytes.

---

## 1.2. Concepto de algoritmo

### 1.2.1. Definición de algoritmo

Un **algoritmo** es una **secuencia finita y ordenada de pasos** bien definidos que resuelve un problema o realiza una tarea.

No es exclusivo de la computación: una receta de cocina, las instrucciones para armar un mueble o el procedimiento para sacar el promedio son algoritmos.

Ejemplo cotidiano — *preparar un café*:

```
1. Inicio
2. Calentar agua
3. Poner una cucharada de café en la taza
4. Verter el agua caliente en la taza
5. Si se desea azúcar, agregar azúcar
6. Mezclar
7. Fin
```

Un **programa** es un algoritmo escrito en un **lenguaje de programación** para que la computadora lo ejecute.

#### Formas de representar un algoritmo

| Forma | Descripción | Ejemplo |
|---|---|---|
| Lenguaje natural | Pasos en español | "Pedir dos números y sumarlos" |
| Pseudocódigo | Mezcla de español y estructura de programa | `Leer a, b` / `suma ← a + b` |
| Diagrama de flujo | Figuras unidas por flechas | Óvalo (inicio/fin), rectángulo (proceso), rombo (decisión), paralelogramo (entrada/salida) |
| Código | En un lenguaje real | `int suma = a + b;` |

Ejemplo en pseudocódigo — *promedio de tres calificaciones*:

```
Inicio
    Leer c1, c2, c3
    promedio ← (c1 + c2 + c3) / 3
    Escribir promedio
Fin
```

Y el mismo algoritmo en C#:

```csharp
Console.Write("Calificación 1: ");
double c1 = double.Parse(Console.ReadLine()!);
Console.Write("Calificación 2: ");
double c2 = double.Parse(Console.ReadLine()!);
Console.Write("Calificación 3: ");
double c3 = double.Parse(Console.ReadLine()!);

double promedio = (c1 + c2 + c3) / 3;
Console.WriteLine($"Promedio: {promedio:F2}");
```

### 1.2.2. Características de un algoritmo

| Característica | Significa que… | Contraejemplo (mal algoritmo) |
|---|---|---|
| **Preciso** | Cada paso es claro y sin ambigüedad | "Agrega un poco de sal" |
| **Definido** | Con los mismos datos, da siempre el mismo resultado | Un paso que depende del humor de quien lo ejecuta |
| **Finito** | Termina en un número limitado de pasos | "Repite mientras 1 sea igual a 1" |
| **Ordenado** | Los pasos tienen una secuencia lógica | Servir el café antes de calentar el agua |
| **Tiene entrada** | Recibe cero o más datos | — |
| **Tiene salida** | Produce al menos un resultado | Un proceso que calcula pero nunca muestra nada |

### 1.2.3. Resolución de problemas computacionales

Resolver un problema con la computadora sigue casi siempre las mismas etapas:

1. **Análisis del problema.** ¿Qué se pide? Identifica:
   - **Entradas**: datos que se conocen o que el usuario proporciona.
   - **Proceso**: cálculos o transformaciones.
   - **Salidas**: resultados que se deben mostrar.
2. **Diseño del algoritmo.** Escribe los pasos en pseudocódigo o diagrama de flujo.
3. **Codificación.** Traduce el algoritmo a un lenguaje de programación.
4. **Prueba y depuración.** Ejecuta con distintos datos y corrige errores.
5. **Documentación y mantenimiento.** Comenta el código y mejóralo con el tiempo.

#### Ejemplo completo: área de un triángulo

**1. Análisis**

| Entradas | Proceso | Salida |
|---|---|---|
| base, altura | área = base × altura ÷ 2 | área |

**2. Diseño (pseudocódigo)**

```
Inicio
    Leer base
    Leer altura
    area ← base * altura / 2
    Escribir "El área es", area
Fin
```

**3. Codificación**

```csharp
Console.Write("Base: ");
double baseT = double.Parse(Console.ReadLine()!);

Console.Write("Altura: ");
double altura = double.Parse(Console.ReadLine()!);

double area = baseT * altura / 2;

Console.WriteLine($"El área es {area}");
```

**4. Prueba**

| base | altura | Resultado esperado | ¿Correcto? |
|---|---|---|---|
| 10 | 5 | 25 | ✔ |
| 3 | 4 | 6 | ✔ |
| 0 | 7 | 0 | ✔ |

> ⚠️ El error más común es empezar a programar sin analizar. Si no tienes claras las entradas y salidas, el código saldrá mal aunque compile.

---

## 1.3. Buenas prácticas de programación

Un programa no solo debe funcionar: también debe poder **leerlo y modificarlo** otra persona (o tú mismo dentro de un mes).

### 1.3.1. Legibilidad del código

#### Nombres descriptivos

```csharp
// ⚠️ Difícil de entender
double x = a * b / 2;

// ✔ Se entiende solo
double area = baseTriangulo * altura / 2;
```

#### Convenciones de nombres en C#

| Elemento | Estilo | Ejemplo |
|---|---|---|
| Variables locales | `camelCase` | `totalVentas`, `edadAlumno` |
| Constantes | `PascalCase` | `TasaIva`, `MaxIntentos` |
| Métodos y clases | `PascalCase` | `CalcularPromedio()`, `Alumno` |

Evita nombres de una letra (salvo contadores de ciclo como `i`, `j`), abreviaturas raras y palabras sin significado (`dato`, `cosa`, `aux2`).

#### Indentación y espacios

```csharp
// ⚠️ Todo pegado
if(edad>=18){Console.WriteLine("Mayor");}else{Console.WriteLine("Menor");}

// ✔ Con sangría y espacios
if (edad >= 18)
{
    Console.WriteLine("Mayor de edad");
}
else
{
    Console.WriteLine("Menor de edad");
}
```

Otras reglas:

- Una instrucción por línea.
- Deja una línea en blanco entre bloques lógicos (entrada, proceso, salida).
- Evita líneas muy largas (más de ~100 caracteres).
- En Visual Studio / VS Code puedes formatear automáticamente con `Ctrl + K, Ctrl + D` o `Shift + Alt + F`.

### 1.3.2. Uso de comentarios

Los comentarios son texto que el compilador **ignora**; sirven para explicar el código a las personas.

```csharp
// Comentario de una línea

/*
   Comentario de
   varias líneas
*/

/// <summary>
/// Comentario de documentación: describe un método o clase.
/// </summary>
```

#### Qué comentar y qué no

```csharp
// ⚠️ Comentario inútil: repite lo que ya dice el código
i = i + 1;   // suma 1 a i

// ✔ Comentario útil: explica el PORQUÉ
// Se suma 1 porque los días del mes empiezan en 1, no en 0
dia = indice + 1;
```

Buenas prácticas:

- Pon un comentario de encabezado con el propósito del programa, el autor y la fecha.
- Explica la **intención** o las decisiones no obvias, no cada línea.
- Si necesitas muchos comentarios para entender algo, probablemente el código debería ser más claro.
- Actualiza los comentarios cuando cambies el código: un comentario incorrecto es peor que ninguno.

```csharp
// ============================================
// Programa: Conversor de temperatura
// Autor:    Ana López
// Fecha:    07/09/2026
// Descripción: convierte grados Celsius a Fahrenheit
// ============================================
```

### 1.3.3. Pruebas básicas

Probar es **ejecutar el programa con datos conocidos** y comparar el resultado obtenido con el esperado.

#### Tipos de errores

| Tipo | Cuándo aparece | Ejemplo |
|---|---|---|
| **De sintaxis** (compilación) | Al compilar: el código no respeta las reglas del lenguaje | Falta un `;` o una llave `}` |
| **De ejecución** | Mientras corre: el programa se detiene | Dividir entre cero, convertir `"hola"` a número |
| **De lógica** | Corre sin fallar, pero el resultado es incorrecto | Usar `+` en lugar de `*` |

> Los errores de lógica son los más peligrosos: el programa "funciona", pero da resultados equivocados. Solo se detectan probando.

#### Tabla de casos de prueba

Antes de dar por terminado un programa, prueba al menos:

- **Casos normales**: datos típicos.
- **Casos límite**: cero, el valor mínimo y máximo permitido, números negativos.
- **Casos inválidos**: texto donde se espera un número, campos vacíos.

Ejemplo para un programa que dice si un alumno aprobó (mínimo 70):

| Entrada | Esperado | Obtenido | ¿Pasa? |
|---|---|---|---|
| 85 | Aprobado | Aprobado | ✔ |
| 70 | Aprobado | Reprobado | ✘ ← se usó `>` en vez de `>=` |
| 69 | Reprobado | Reprobado | ✔ |
| 0 | Reprobado | Reprobado | ✔ |
| "abc" | Mensaje de error | El programa se cierra | ✘ ← falta validar |

#### Prueba de escritorio

Es seguir el algoritmo **a mano**, en papel, anotando el valor de cada variable en cada paso. Se ve a detalle en la Unidad 2 (2.5.1).

---

## 1.4. Estructura básica de un programa

### 1.4.1. Inicio y fin de un programa

Todo programa tiene un **punto de inicio** (donde empieza la ejecución) y un **fin** (donde termina).

#### Forma clásica en C#

```csharp
using System;                    // 1. Importar espacios de nombres

namespace MiPrimerPrograma       // 2. Espacio de nombres (agrupa el código)
{
    class Program                // 3. Clase que contiene el programa
    {
        static void Main(string[] args)   // 4. INICIO: método Main
        {
            Console.WriteLine("Hola mundo");
        }                                 // 5. FIN: al cerrar Main termina el programa
    }
}
```

| Parte | Para qué sirve |
|---|---|
| `using System;` | Permite usar `Console` sin escribir `System.Console` |
| `namespace` | Organiza el código en grupos con nombre |
| `class Program` | En C# todo el código vive dentro de una clase |
| `static void Main` | **Punto de entrada**: lo primero que se ejecuta |
| `{ }` | Delimitan bloques de código |
| `;` | Termina cada instrucción |

#### Forma moderna (*top-level statements*, C# 9+)

Desde .NET 6, `dotnet new console` genera un archivo `Program.cs` con una sola línea:

```csharp
Console.WriteLine("Hola mundo");
```

El compilador crea automáticamente la clase y el `Main`. **El inicio es la primera línea del archivo y el fin es la última.** Es la forma que se usa en este material.

> ⚠️ C# distingue mayúsculas de minúsculas: `console.writeline` no compila; debe ser `Console.WriteLine`.

### 1.4.2. Secuencia de instrucciones

En la programación estructurada, las instrucciones se ejecutan **una tras otra, de arriba hacia abajo**, en el orden en que están escritas. Es la estructura más simple: la **secuencia**.

Casi cualquier programa sigue el esquema **Entrada → Proceso → Salida**:

```csharp
// ENTRADA
Console.Write("¿Cuántas horas trabajaste? ");
int horas = int.Parse(Console.ReadLine()!);

Console.Write("¿Cuánto te pagan por hora? ");
decimal pagoHora = decimal.Parse(Console.ReadLine()!);

// PROCESO
decimal sueldo = horas * pagoHora;

// SALIDA
Console.WriteLine($"Tu sueldo es {sueldo:C}");
```

#### El orden importa

```csharp
// ⚠️ Incorrecto: se usa 'total' antes de calcularlo
int precio = 50;
int cantidad = 3;
Console.WriteLine(total);       // error: 'total' no existe todavía
int total = precio * cantidad;

// ✔ Correcto
int precio2 = 50;
int cantidad2 = 3;
int total2 = precio2 * cantidad2;
Console.WriteLine(total2);      // → 150
```

```csharp
// ⚠️ Error de lógica: el orden cambia el resultado
int a = 5;
int b = 10;
a = b;      // a = 10
b = a;      // b = 10  ← se perdió el 5

// ✔ Intercambio correcto con variable auxiliar
int x = 5;
int y = 10;
int temp = x;   // temp = 5
x = y;          // x = 10
y = temp;       // y = 5
```

---

## Resumen

- Un **bit** vale 0 o 1; un **byte** son 8 bits y representa 256 valores. Con *n* bits hay 2ⁿ combinaciones.
- El **binario** es base 2: cada posición vale el doble que la anterior (1, 2, 4, 8, 16…). Para convertir de decimal a binario se divide entre 2 y se leen los residuos de abajo hacia arriba.
- Cada carácter de texto tiene un código numérico (**ASCII**/Unicode); por eso una palabra se puede representar en binario.
- Un **algoritmo** es una secuencia finita, precisa y ordenada de pasos. Resolver un problema es: analizar → diseñar → codificar → probar → documentar.
- Código legible = nombres claros, sangría, comentarios que explican el porqué y pruebas con casos normales, límite e inválidos.
- Un programa tiene inicio y fin, y en su forma más simple ejecuta instrucciones **en secuencia**: entrada → proceso → salida.

