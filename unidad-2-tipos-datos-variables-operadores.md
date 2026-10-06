# Unidad 2. Tipos de datos, variables y operadores

## 2.1. Tipos de datos

Un **tipo de dato** indica qué clase de valor puede guardar una variable, cuánta memoria ocupa y qué operaciones se pueden hacer con ella. C# es un lenguaje **fuertemente tipado**: cada variable tiene un tipo fijo que el compilador verifica.

```csharp
int edad = 20;          // número entero
double precio = 99.50;  // número con decimales
string nombre = "Ana";  // texto
bool activo = true;     // verdadero o falso
char inicial = 'A';     // un solo carácter
```

### Enteros

| Tipo | Tamaño | Rango aproximado | Uso típico |
|---|---|---|---|
| `byte` | 1 byte | 0 a 255 | Valores pequeños sin signo |
| `short` | 2 bytes | −32 768 a 32 767 | Rara vez se usa |
| `int` | 4 bytes | ±2 100 millones | **Opción por defecto** para enteros |
| `long` | 8 bytes | ±9.2 × 10¹⁸ | Valores muy grandes |

> Relación con la Unidad 1: un `byte` ocupa 8 bits, por eso su rango es 0 a 255 (2⁸ = 256 valores). Un `int` usa 32 bits.

### Reales (con decimales)

| Tipo | Tamaño | Precisión | Sufijo | Uso típico |
|---|---|---|---|---|
| `float` | 4 bytes | ~7 dígitos | `f` | Gráficos, juegos |
| `double` | 8 bytes | ~15-16 dígitos | `d` (opcional) | **Opción por defecto** para decimales |
| `decimal` | 16 bytes | 28-29 dígitos | `m` | **Dinero** y cálculos financieros |

```csharp
float temperatura = 36.6f;   // sin la f no compila
double distancia = 1234.5678;
decimal saldo = 1500.75m;    // sin la m no compila
```

> ⚠️ `double` no representa exactamente todos los decimales: `0.1 + 0.2` da `0.30000000000000004`. Para dinero usa `decimal`.

### Otros tipos

| Tipo | Valores | Ejemplo |
|---|---|---|
| `bool` | `true` / `false` | `bool aprobado = true;` |
| `char` | Un solo carácter, entre comillas **simples** | `char letra = 'A';` |
| `string` | Texto (cero o más caracteres), entre comillas **dobles** | `string saludo = "Hola";` |

> ⚠️ `'A'` es un `char` y `"A"` es un `string`. No son intercambiables.

### ¿Qué tipo elijo?

| Dato a guardar | Tipo recomendado |
|---|---|
| Edad, cantidad de productos, número de alumnos | `int` |
| Estatura, peso, promedio, temperatura | `double` |
| Precio, salario, saldo | `decimal` |
| ¿Aprobó?, ¿está activo?, ¿es mayor de edad? | `bool` |
| Sexo (`'M'`/`'F'`), opción de menú, inicial | `char` |
| Nombre, dirección, correo | `string` |
| Número de teléfono, CURP, código postal | `string` (no se hacen cuentas con ellos) |

### Conversión entre tipos

#### Implícita (automática)

Ocurre cuando no hay riesgo de perder información:

```csharp
int entero = 100;
double real = entero;     // int → double, automático
```

#### Explícita (*cast*)

Cuando puede perderse información hay que pedirla con `(tipo)`:

```csharp
double pi = 3.99;
int truncado = (int)pi;   // → 3 (trunca, NO redondea)
```

#### Texto → número

```csharp
int a = int.Parse("42");          // → 42
double b = double.Parse("3.5");   // → 3.5
int c = Convert.ToInt32("123");   // → 123

int.Parse("hola");                // ⚠️ lanza FormatException
```

La forma segura es `TryParse` (se ve en 2.4.1).

---

## 2.2. Variables y constantes

### Variables

Una **variable** es un espacio en memoria, con un **nombre** y un **tipo**, cuyo valor **puede cambiar** durante la ejecución.

```
   tipo   nombre   valor inicial
    │       │        │
   int   contador =  0 ;
```

```csharp
int contador;          // declaración
contador = 0;          // asignación
contador = contador + 1;

int total = 0;         // declaración + inicialización en una línea
```

#### Reglas para nombrar variables

| Regla | Válido | Inválido |
|---|---|---|
| Empieza con letra o `_` | `edad`, `_total` | `1numero` |
| Sin espacios | `nombreCompleto` | `nombre completo` |
| Sin símbolos especiales (salvo `_`) | `precio_final` | `precio-final`, `costo$` |
| No puede ser palabra reservada | `clase` | `class`, `int`, `if` |
| Distingue mayúsculas | `edad` ≠ `Edad` | — |

Convención en C#: variables en `camelCase` → `totalVentas`, `promedioFinal`.

#### Inferencia con `var`

```csharp
var cantidad = 10;      // el compilador deduce int
var mensaje = "Hola";   // deduce string
```

`var` no hace la variable "dinámica": el tipo se fija al compilar y no puede cambiar.

```csharp
var x = 10;
x = "hola";   // ⚠️ error: x es int
```

#### Ámbito (*scope*)

Una variable solo existe dentro del bloque `{ }` donde se declaró:

```csharp
if (true)
{
    int dentro = 5;
}
Console.WriteLine(dentro);   // ⚠️ error: 'dentro' no existe aquí
```

#### Variables sin inicializar

```csharp
int numero;
Console.WriteLine(numero);   // ⚠️ error: variable local sin asignar
```

C# obliga a asignar un valor antes de usar una variable local.

### Constantes

Una **constante** es un valor con nombre que **no puede cambiar** después de declararse. Se usa `const`.

```csharp
const double Pi = 3.14159;
const int DiasSemana = 7;
const decimal TasaIva = 0.16m;

// Pi = 3.14;  ⚠️ error de compilación
```

Reglas de `const`:

- Debe inicializarse en la misma línea donde se declara.
- Solo admite tipos primitivos, `string` o `null`.
- Por convención se nombra en `PascalCase`.

#### ¿Por qué usar constantes?

```csharp
// ⚠️ "Número mágico": ¿qué es 0.16? Si cambia, hay que buscarlo en todo el código
decimal total = subtotal + subtotal * 0.16m;

// ✔ Con constante: se entiende y se cambia en un solo lugar
const decimal TasaIva = 0.16m;
decimal total2 = subtotal + subtotal * TasaIva;
```

| | Variable | Constante |
|---|---|---|
| ¿Su valor cambia? | Sí | No |
| ¿Se debe inicializar al declarar? | No (pero sí antes de usarla) | Sí, obligatorio |
| Palabra clave | tipo o `var` | `const` |
| Ejemplo | `int edad = 20;` | `const int MayoriaEdad = 18;` |

---

## 2.3. Operadores

Un **operador** es un símbolo que realiza una operación sobre uno o más valores (operandos) y produce un resultado.

### 2.3.1. Operadores aritméticos

| Operador | Operación | Ejemplo | Resultado |
|---|---|---|---|
| `+` | Suma | `7 + 2` | `9` |
| `-` | Resta | `7 - 2` | `5` |
| `*` | Multiplicación | `7 * 2` | `14` |
| `/` | División | `7 / 2` | `3` ⚠️ |
| `%` | Módulo (residuo) | `7 % 2` | `1` |

#### Programa de prueba: operadores aritméticos en consola

```csharp
int a = 17;
int b = 5;

Console.WriteLine("--------------");
Console.WriteLine("-- Suma (+) --");
Console.WriteLine("--------------");
Console.WriteLine(a + b);       // → 22
Console.WriteLine(a + 0);       // → 17
Console.WriteLine(-a + b);      // → -12

Console.WriteLine("---------------");
Console.WriteLine("-- Resta (-) --");
Console.WriteLine("---------------");
Console.WriteLine(a - b);       // → 12
Console.WriteLine(b - a);       // → -12
Console.WriteLine(a - a);       // → 0

Console.WriteLine("------------------------");
Console.WriteLine("-- Multiplicación (*) --");
Console.WriteLine("------------------------");
Console.WriteLine(a * b);       // → 85
Console.WriteLine(a * 0);       // → 0
Console.WriteLine(a * -1);      // → -17

Console.WriteLine("------------------");
Console.WriteLine("-- División (/) --");
Console.WriteLine("------------------");
Console.WriteLine(a / b);       // → 3    (int / int → se pierden los decimales)
Console.WriteLine(17.0 / 5);    // → 3.4  (double / int → double)
Console.WriteLine((double)a / b); // → 3.4

Console.WriteLine("----------------");
Console.WriteLine("-- Módulo (%) --");
Console.WriteLine("----------------");
Console.WriteLine(a % b);       // → 2   (17 = 5 * 3 + 2)
Console.WriteLine(10 % 2);      // → 0   (10 es par)
Console.WriteLine(7 % 2);       // → 1   (7 es impar)
Console.WriteLine(3 % 5);       // → 3   (si el divisor es mayor, el residuo es el mismo número)
```

> Observa que `a / b` y `a % b` se complementan: `17 / 5 = 3` y `17 % 5 = 2`, porque `5 * 3 + 2 = 17`.

#### División entera vs decimal

```csharp
Console.WriteLine(7 / 2);         // → 3   (int / int = int)
Console.WriteLine(7 / 2.0);       // → 3.5 (si uno es double, el resultado es double)
Console.WriteLine((double)7 / 2); // → 3.5
```

> ⚠️ Error clásico al calcular promedios: `int suma = 15; int n = 4; double prom = suma / n;` da `3`, no `3.75`. Solución: `double prom = (double)suma / n;`

#### Usos comunes de `%`

```csharp
bool esPar = numero % 2 == 0;
int ultimoDigito = 1234 % 10;     // → 4
int minutos = 135 % 60;           // → 15
int horas = 135 / 60;             // → 2
```

`%` es justo lo que se usó en la Unidad 1 para convertir a binario: `n % 2` da el residuo (0 o 1).

#### Incremento, decremento y asignación compuesta

```csharp
int x = 5;
x++;      // x = 6
x--;      // x = 5
x += 3;   // x = x + 3 → 8
x -= 2;   // x = x - 2 → 6
x *= 2;   // x = x * 2 → 12
x /= 4;   // x = x / 4 → 3
x %= 2;   // x = x % 2 → 1
```

```csharp
int a = 5;
int b = a++;   // b = 5, a = 6  (postincremento: usa y luego suma)
int c = ++a;   // c = 7, a = 7  (preincremento: suma y luego usa)
```

#### Clase `Math`

```csharp
Math.Pow(2, 10);      // → 1024
Math.Sqrt(81);        // → 9
Math.Abs(-5);         // → 5
Math.Round(3.456, 2); // → 3.46
Math.Max(4, 9);       // → 9
Math.Min(4, 9);       // → 4
Math.PI;              // → 3.14159265358979
```

#### Jerarquía (precedencia) de operadores aritméticos

1. `()` paréntesis
2. `*`, `/`, `%`
3. `+`, `-`

A igual jerarquía, se evalúa de **izquierda a derecha**.

```csharp
int r1 = 2 + 3 * 4;      // → 14 (primero 3*4)
int r2 = (2 + 3) * 4;    // → 20
int r3 = 20 / 4 * 2;     // → 10 (izquierda a derecha: 5 * 2)
```

> ⚠️ `(a + b + c) / 3` no es lo mismo que `a + b + c / 3`. Usa paréntesis en los promedios.

### 2.3.2. Operadores relacionales

Comparan dos valores y **siempre devuelven un `bool`** (`true` o `false`).

| Operador | Significado | Ejemplo | Resultado |
|---|---|---|---|
| `==` | Igual a | `5 == 5` | `true` |
| `!=` | Distinto de | `5 != 3` | `true` |
| `>` | Mayor que | `5 > 8` | `false` |
| `<` | Menor que | `5 < 8` | `true` |
| `>=` | Mayor o igual | `5 >= 5` | `true` |
| `<=` | Menor o igual | `4 <= 3` | `false` |

#### Programa de prueba: operadores relacionales en consola

```csharp
int a = 10;
int b = 20;

Console.WriteLine("----------------");
Console.WriteLine("-- Igual (==) --");
Console.WriteLine("----------------");
Console.WriteLine(a == b);      // → False
Console.WriteLine(a == 10);     // → True
Console.WriteLine(b == 2 * a);  // → True

Console.WriteLine("--------------------");
Console.WriteLine("-- Distinto (!=) --");
Console.WriteLine("--------------------");
Console.WriteLine(a != b);      // → True
Console.WriteLine(a != 10);     // → False

Console.WriteLine("------------------");
Console.WriteLine("-- Mayor que (>) --");
Console.WriteLine("------------------");
Console.WriteLine(a > b);       // → False
Console.WriteLine(b > a);       // → True
Console.WriteLine(a > 10);      // → False

Console.WriteLine("------------------");
Console.WriteLine("-- Menor que (<) --");
Console.WriteLine("------------------");
Console.WriteLine(a < b);       // → True
Console.WriteLine(b < a);       // → False
Console.WriteLine(a < 10);      // → False

Console.WriteLine("-----------------------");
Console.WriteLine("-- Mayor o igual (>=) --");
Console.WriteLine("-----------------------");
Console.WriteLine(a >= 10);     // → True
Console.WriteLine(a >= b);      // → False

Console.WriteLine("-----------------------");
Console.WriteLine("-- Menor o igual (<=) --");
Console.WriteLine("-----------------------");
Console.WriteLine(a <= 10);     // → True
Console.WriteLine(b <= a);      // → False
```

> Observa la diferencia entre `>` y `>=`: con `a = 10`, `a > 10` es `False` pero `a >= 10` es `True`. Es el error más común en los límites (por ejemplo, ¿un 70 aprueba?).

```csharp
int calificacion = 75;
bool aprobo = calificacion >= 70;   // true
Console.WriteLine(aprobo);          // → True
```

> ⚠️ No confundas `=` (asignación) con `==` (comparación).

Comparar cadenas:

```csharp
string a = "Hola";
string b = "hola";
Console.WriteLine(a == b);                                          // → False
Console.WriteLine(a.Equals(b, StringComparison.OrdinalIgnoreCase)); // → True
```

Comparar caracteres compara su código ASCII/Unicode:

```csharp
Console.WriteLine('a' < 'b');   // → True  (97 < 98)
Console.WriteLine('Z' < 'a');   // → True  (90 < 97)
```

### 2.3.3. Operadores lógicos

Combinan expresiones booleanas.

| Operador | Nombre | Devuelve `true` cuando… |
|---|---|---|
| `&&` | Y (AND) | **ambas** condiciones son verdaderas |
| `\|\|` | O (OR) | **al menos una** es verdadera |
| `!` | NO (NOT) | la condición es falsa |

#### Tablas de verdad

| A | B | `A && B` | `A \|\| B` | `!A` |
|---|---|---|---|---|
| true | true | true | true | false |
| true | false | false | true | false |
| false | true | false | true | true |
| false | false | false | false | true |

#### Programa de prueba: tablas de verdad en consola

Cada bloque imprime las cuatro combinaciones posibles de `false` y `true`, en el mismo orden: `(F,F)`, `(T,F)`, `(F,T)`, `(T,T)`.

```csharp
Console.WriteLine("---------");
Console.WriteLine("-- AND --");
Console.WriteLine("---------");
Console.WriteLine(false & false);   // → False
Console.WriteLine(true & false);    // → False
Console.WriteLine(false & true);    // → False
Console.WriteLine(true & true);     // → True

Console.WriteLine("--------");
Console.WriteLine("-- OR --");
Console.WriteLine("--------");
Console.WriteLine(false | false);   // → False
Console.WriteLine(true | false);    // → True
Console.WriteLine(false | true);    // → True
Console.WriteLine(true | true);     // → True

Console.WriteLine("---------");
Console.WriteLine("-- NOT --");
Console.WriteLine("---------");
Console.WriteLine(!false);          // → True
Console.WriteLine(!true);           // → False

Console.WriteLine("----------------");
Console.WriteLine("-- AND -> Not --");
Console.WriteLine("----------------");
Console.WriteLine(!(false & false)); // → True
Console.WriteLine(!(true & false));  // → True
Console.WriteLine(!(false & true));  // → True
Console.WriteLine(!(true & true));   // → False

Console.WriteLine("---------------");
Console.WriteLine("-- OR -> Not --");
Console.WriteLine("---------------");
Console.WriteLine(!(false | false)); // → True
Console.WriteLine(!(true | false));  // → False
Console.WriteLine(!(false | true));  // → False
Console.WriteLine(!(true | true));   // → False

Console.WriteLine("---------");
Console.WriteLine("-- XOR --");
Console.WriteLine("---------");
Console.WriteLine(false ^ false);   // → False
Console.WriteLine(true ^ false);    // → True
Console.WriteLine(false ^ true);    // → True
Console.WriteLine(true ^ true);     // → False
```

> `&` y `|` dan el mismo resultado que `&&` y `||`, pero **siempre evalúan ambos lados**. `&&` y `||` se detienen en cuanto conocen el resultado (ver *Evaluación en cortocircuito* más abajo). En condiciones de `if` usa `&&` y `||`. `^` (XOR) es `true` cuando **exactamente uno** de los dos es `true`.

```csharp
int edad = 20;
bool tieneCredencial = true;

bool puedeEntrar = edad >= 18 && tieneCredencial;   // true
bool descuento = edad < 12 || edad >= 60;           // false
bool esMenor = !(edad >= 18);                       // false
```

#### Rangos

```csharp
int nota = 85;
bool enRango = nota >= 0 && nota <= 100;   // ✔

// ⚠️ Esto NO compila en C# (sí en matemáticas):
// bool enRango = 0 <= nota <= 100;
```

#### Evaluación en cortocircuito

`&&` y `||` dejan de evaluar en cuanto conocen el resultado:

```csharp
int divisor = 0;

// Como divisor != 0 es false, NO se evalúa 10 / divisor → no hay error
if (divisor != 0 && 10 / divisor > 2)
{
    Console.WriteLine("Mayor que 2");
}
```

#### Precedencia completa (de mayor a menor)

1. `()` paréntesis
2. `!`, `++`, `--` (unarios)
3. `*`, `/`, `%`
4. `+`, `-`
5. `<`, `>`, `<=`, `>=`
6. `==`, `!=`
7. `&&`
8. `||`
9. `=`, `+=`, `-=`… (asignación)

```csharp
bool r = 3 + 2 * 4 > 10 && !false;   // 3 + 8 = 11 > 10 → true && true → true
```

> Ante la duda, usa paréntesis: hacen el código más legible.

---

## 2.4. Entrada y salida de datos

Los programas de consola se comunican con el usuario a través de la clase `Console`.

### 2.4.1. Entrada de datos por consola

`Console.ReadLine()` espera a que el usuario escriba algo y presione **Enter**, y devuelve lo escrito como `string`.

```csharp
Console.Write("¿Cómo te llamas? ");
string? nombre = Console.ReadLine();
Console.WriteLine($"Hola, {nombre}");
```

> El `?` en `string?` indica que el valor puede ser `null` (por ejemplo, si la entrada se cierra). El operador `!` en `Console.ReadLine()!` le dice al compilador "confía en mí, no será null".

#### Leer números

`ReadLine()` **siempre devuelve texto**. Para obtener un número hay que convertirlo:

```csharp
Console.Write("Edad: ");
int edad = int.Parse(Console.ReadLine()!);

Console.Write("Estatura (m): ");
double estatura = double.Parse(Console.ReadLine()!);
```

> ⚠️ Si el usuario escribe `"veinte"` en lugar de `20`, `int.Parse` lanza `FormatException` y el programa se detiene.

#### Lectura segura con `TryParse`

`TryParse` intenta convertir; devuelve `true` si lo logra y deja el número en la variable `out`.

```csharp
Console.Write("Ingresa tu edad: ");
string? entrada = Console.ReadLine();

if (int.TryParse(entrada, out int edad))
{
    Console.WriteLine($"Tienes {edad} años");
}
else
{
    Console.WriteLine("Eso no es un número válido");
}
```

| Método | Si el texto no es número |
|---|---|
| `int.Parse(s)` | Lanza excepción (el programa se cae) |
| `Convert.ToInt32(s)` | Lanza excepción |
| `int.TryParse(s, out n)` | Devuelve `false`, no se cae |

#### Leer un carácter o una tecla

```csharp
Console.Write("¿Continuar? (s/n): ");
char opcion = Console.ReadLine()![0];       // primer carácter de lo escrito

Console.WriteLine("Presiona cualquier tecla...");
Console.ReadKey();                          // espera una tecla sin Enter
```

### 2.4.2. Salida de datos por consola

| Método | Qué hace |
|---|---|
| `Console.Write(x)` | Escribe sin salto de línea |
| `Console.WriteLine(x)` | Escribe y salta a la siguiente línea |
| `Console.WriteLine()` | Solo imprime una línea en blanco |
| `Console.Clear()` | Limpia la pantalla |

```csharp
Console.Write("Hola ");
Console.Write("mundo");
Console.WriteLine();          // salto
Console.WriteLine("Adiós");

// → Hola mundo
// → Adiós
```

#### Formas de combinar texto y valores

```csharp
string nombre = "Luis";
int edad = 21;

Console.WriteLine("Hola " + nombre + ", tienes " + edad + " años");   // concatenación
Console.WriteLine("Hola {0}, tienes {1} años", nombre, edad);          // marcadores
Console.WriteLine($"Hola {nombre}, tienes {edad} años");               // interpolación (recomendada)
```

#### Formato de números

| Formato | Significado | Ejemplo | Resultado |
|---|---|---|---|
| `F2` | 2 decimales fijos | `$"{3.14159:F2}"` | `3.14` |
| `N2` | Separador de miles + 2 decimales | `$"{1234567.8:N2}"` | `1,234,567.80` |
| `C` | Moneda | `$"{99.5:C}"` | `$99.50` |
| `P1` | Porcentaje con 1 decimal | `$"{0.256:P1}"` | `25.6 %` |
| `D3` | Entero con 3 dígitos | `$"{7:D3}"` | `007` |

> El símbolo de moneda y los separadores dependen de la configuración regional del equipo.

#### Alineación en columnas

`{valor,ancho}`: ancho positivo alinea a la derecha, negativo a la izquierda.

```csharp
Console.WriteLine($"{"Producto",-10}{"Precio",10}");
Console.WriteLine($"{"Lápiz",-10}{8.5,10:F2}");
Console.WriteLine($"{"Cuaderno",-10}{45,10:F2}");

// → Producto      Precio
// → Lápiz           8.50
// → Cuaderno       45.00
```

#### Secuencias de escape

| Secuencia | Resultado |
|---|---|
| `\n` | Salto de línea |
| `\t` | Tabulación |
| `\"` | Comillas dobles |
| `\\` | Barra invertida |

```csharp
Console.WriteLine("Nombre:\tAna\nEdad:\t20");
// → Nombre:  Ana
// → Edad:    20
```

---

## 2.5. Estructuras secuenciales

Una **estructura secuencial** es aquella en la que las instrucciones se ejecutan **una tras otra, en orden, sin saltos ni repeticiones**. Es la forma más simple de programa y sigue el esquema:

```
ENTRADA  →  PROCESO  →  SALIDA
```

Ejemplo — *calcular el total de una compra con IVA*:

```csharp
const decimal TasaIva = 0.16m;

// Entrada
Console.Write("Precio unitario: ");
decimal precio = decimal.Parse(Console.ReadLine()!);

Console.Write("Cantidad: ");
int cantidad = int.Parse(Console.ReadLine()!);

// Proceso
decimal subtotal = precio * cantidad;
decimal iva = subtotal * TasaIva;
decimal total = subtotal + iva;

// Salida
Console.WriteLine($"Subtotal: {subtotal:C}");
Console.WriteLine($"IVA:      {iva:C}");
Console.WriteLine($"Total:    {total:C}");
```

### 2.5.1. Ejecución paso a paso

#### Prueba de escritorio

Una **prueba de escritorio** consiste en ejecutar el programa **a mano**, línea por línea, anotando en una tabla cómo cambian las variables. Sirve para encontrar errores de lógica sin computadora.

Programa:

```csharp
int a = 4;          // línea 1
int b = 7;          // línea 2
int c = a + b;      // línea 3
a = c * 2;          // línea 4
b = a - b;          // línea 5
c = c % 3;          // línea 6
Console.WriteLine($"{a} {b} {c}");   // línea 7
```

Tabla de seguimiento:

| Línea | a | b | c | Salida |
|---|---|---|---|---|
| 1 | 4 | — | — | |
| 2 | 4 | 7 | — | |
| 3 | 4 | 7 | 11 | |
| 4 | **22** | 7 | 11 | |
| 5 | 22 | **15** | 11 | |
| 6 | 22 | 15 | **2** | |
| 7 | 22 | 15 | 2 | `22 15 2` |

> Cada fila muestra el estado **después** de ejecutar esa línea. Marca en negritas lo que cambió.

#### Depurar con el *debugger*

En Visual Studio o VS Code puedes ver la ejecución paso a paso de verdad:

| Acción | Visual Studio | VS Code |
|---|---|---|
| Poner/quitar punto de interrupción (*breakpoint*) | `F9` o clic en el margen | `F9` o clic en el margen |
| Iniciar depuración | `F5` | `F5` |
| Ejecutar la siguiente línea | `F10` | `F10` |
| Entrar a un método | `F11` | `F11` |
| Continuar hasta el siguiente *breakpoint* | `F5` | `F5` |

Mientras el programa está detenido, pasa el cursor sobre una variable o mira la ventana **Variables / Locales** para ver su valor actual.

#### Imprimir valores intermedios

Otra técnica sencilla es mostrar valores temporales para verificar el proceso:

```csharp
decimal subtotal = precio * cantidad;
Console.WriteLine($"[DEBUG] subtotal = {subtotal}");   // quitar al terminar
```

---

## Resumen

- Elige `int` para enteros, `double` para decimales, `decimal` para dinero, `bool` para sí/no, `char` para un carácter y `string` para texto.
- Una **variable** puede cambiar de valor; una **constante** (`const`) no. Usa constantes para evitar "números mágicos".
- Cuidado con la **división entera** (`7 / 2 == 3`) y con confundir `=` y `==`.
- Los operadores relacionales devuelven `bool`; los lógicos (`&&`, `||`, `!`) los combinan y se evalúan en cortocircuito.
- `Console.ReadLine()` siempre devuelve texto: conviértelo con `Parse` o, mejor, con `TryParse`.
- Una estructura **secuencial** ejecuta entrada → proceso → salida en orden. La **prueba de escritorio** y el *debugger* permiten seguirla paso a paso.

