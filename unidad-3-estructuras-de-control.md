# Unidad 3. Estructuras de control

En la Unidad 2 todos los programas eran **secuenciales**: cada instrucción se ejecutaba exactamente una vez, de arriba hacia abajo. Las **estructuras de control** permiten cambiar ese flujo:

| Estructura | Pregunta que responde | Instrucciones en C# |
|---|---|---|
| **Condicional** (decisión) | ¿Ejecuto esto o no? ¿Cuál de estos caminos tomo? | `if`, `if-else`, `switch` |
| **Repetitiva** (ciclo) | ¿Cuántas veces repito esto? | `while`, `do-while`, `for` |

## 3.1. Estructuras condicionales

Una estructura condicional evalúa una **condición** (una expresión que da `true` o `false`) y, según el resultado, ejecuta un bloque de código u otro.

### 3.1.1. `if`

Ejecuta un bloque **solo si** la condición es verdadera. Si es falsa, lo salta.

```csharp
if (condicion)
{
    // se ejecuta solo si condicion es true
}
```

```csharp
Console.Write("Temperatura: ");
double temp = double.Parse(Console.ReadLine()!);

if (temp > 38)
{
    Console.WriteLine("Tienes fiebre");
}

Console.WriteLine("Fin del programa");   // se ejecuta siempre
```

> ⚠️ Si el bloque tiene una sola línea las llaves son opcionales, pero **úsalas siempre**: evitan errores al agregar líneas después.

```csharp
// ⚠️ Trampa: solo la primera línea pertenece al if
if (temp > 38)
    Console.WriteLine("Tienes fiebre");
    Console.WriteLine("Ve al médico");   // ¡se ejecuta SIEMPRE!
```

### 3.1.2. `if-else`

Elige entre **dos caminos**: uno si la condición es verdadera y otro si es falsa.

```csharp
int calificacion = 65;

if (calificacion >= 70)
{
    Console.WriteLine("Aprobado");
}
else
{
    Console.WriteLine("Reprobado");
}
```

#### `if - else if - else` (múltiples caminos)

Cuando hay más de dos opciones se encadenan condiciones. Se evalúan **en orden** y se ejecuta **solo el primer** bloque cuya condición sea verdadera.

```csharp
int calificacion = 87;

if (calificacion >= 90)
{
    Console.WriteLine("Excelente");
}
else if (calificacion >= 80)
{
    Console.WriteLine("Muy bien");      // ← se ejecuta este
}
else if (calificacion >= 70)
{
    Console.WriteLine("Suficiente");
}
else
{
    Console.WriteLine("Reprobado");
}
```

> ⚠️ El orden importa. Si la primera condición fuera `calificacion >= 70`, un 95 diría "Suficiente".

#### `if` anidados

Un `if` dentro de otro. Útil cuando una decisión depende de otra previa.

```csharp
Console.Write("Edad: ");
int edad = int.Parse(Console.ReadLine()!);

if (edad >= 18)
{
    Console.Write("¿Tienes INE? (s/n): ");
    char ine = Console.ReadLine()![0];

    if (ine == 's')
    {
        Console.WriteLine("Puedes votar");
    }
    else
    {
        Console.WriteLine("Necesitas tramitar tu INE");
    }
}
else
{
    Console.WriteLine("Aún no puedes votar");
}
```

#### Operador ternario `? :`

Forma corta de un `if-else` que **devuelve un valor**:

```csharp
string resultado = calificacion >= 70 ? "Aprobado" : "Reprobado";
```

### 3.1.3. `switch`

Compara **una sola variable** contra varios valores posibles. Es más legible que muchos `else if` cuando se trata de opciones fijas (menús, días, categorías).

```csharp
Console.Write("Número de día (1-7): ");
int dia = int.Parse(Console.ReadLine()!);

switch (dia)
{
    case 1:
        Console.WriteLine("Lunes");
        break;
    case 2:
        Console.WriteLine("Martes");
        break;
    case 3:
        Console.WriteLine("Miércoles");
        break;
    case 4:
        Console.WriteLine("Jueves");
        break;
    case 5:
        Console.WriteLine("Viernes");
        break;
    case 6:
    case 7:                              // varios casos comparten código
        Console.WriteLine("Fin de semana");
        break;
    default:                             // ningún caso coincidió
        Console.WriteLine("Día inválido");
        break;
}
```

| Parte | Función |
|---|---|
| `switch (variable)` | Valor que se va a comparar |
| `case valor:` | Código que se ejecuta si coincide |
| `break;` | Termina el `switch` (**obligatorio** en C# al final de cada caso) |
| `default:` | Se ejecuta si ningún `case` coincide (equivale al `else`) |

> ⚠️ En C#, olvidar el `break` es un **error de compilación** (en otros lenguajes, como C o Java, el código "se cae" al siguiente caso).

`switch` funciona con `int`, `char`, `string`, `bool` y enumeraciones:

```csharp
Console.Write("Operación (+ - * /): ");
char op = Console.ReadLine()![0];
double a = 10, b = 4;

switch (op)
{
    case '+': Console.WriteLine(a + b); break;
    case '-': Console.WriteLine(a - b); break;
    case '*': Console.WriteLine(a * b); break;
    case '/': Console.WriteLine(a / b); break;
    default:  Console.WriteLine("Operación no válida"); break;
}
```

#### Expresión `switch` (C# 8+)

Forma compacta cuando cada caso solo **devuelve un valor**:

```csharp
string nombreDia = dia switch
{
    1 => "Lunes",
    2 => "Martes",
    3 => "Miércoles",
    4 => "Jueves",
    5 => "Viernes",
    6 or 7 => "Fin de semana",
    _ => "Día inválido"          // _ equivale a default
};
```

#### ¿`if` o `switch`?

| Situación | Usa |
|---|---|
| Comparar **una variable** con **valores exactos** (1, 2, 3 / 'a', 'b' / "alta", "baja") | `switch` |
| Condiciones con **rangos** (`>= 90`, `< 18`) | `if - else if` |
| Condiciones que combinan **varias variables** (`edad > 18 && tieneINE`) | `if` |

### 3.1.4. Diseño de algoritmos con decisiones

Antes de programar una decisión conviene identificar **todas las condiciones posibles** y qué hacer en cada una.

#### Pasos

1. Identifica las entradas y las salidas posibles.
2. Escribe una tabla con **cada caso** y su resultado.
3. Verifica que los casos **no se traslapen** y que **no falte ninguno** (incluidos los inválidos).
4. Escribe el pseudocódigo con `Si - Entonces - Sino`.
5. Codifica y prueba **cada camino** al menos una vez.

#### Ejemplo: tarifa de un estacionamiento

> Las primeras 2 horas cuestan $15 cada una. Cada hora adicional cuesta $10. Si se queda más de 12 horas se cobra una tarifa fija de $150. Las horas deben ser mayores a 0.

**Tabla de casos**

| Condición | Cálculo |
|---|---|
| horas ≤ 0 | Error: "Horas inválidas" |
| 1 ≤ horas ≤ 2 | horas × 15 |
| 3 ≤ horas ≤ 12 | 2 × 15 + (horas − 2) × 10 |
| horas > 12 | 150 |

**Pseudocódigo**

```
Inicio
    Leer horas
    Si horas <= 0 Entonces
        Escribir "Horas inválidas"
    Sino Si horas <= 2 Entonces
        total ← horas * 15
    Sino Si horas <= 12 Entonces
        total ← 30 + (horas - 2) * 10
    Sino
        total ← 150
    FinSi
    Escribir total
Fin
```

**Código**

```csharp
const int TarifaInicial = 15;
const int TarifaAdicional = 10;
const int TarifaFija = 150;

Console.Write("Horas de estacionamiento: ");
int horas = int.Parse(Console.ReadLine()!);

if (horas <= 0)
{
    Console.WriteLine("Horas inválidas");
}
else
{
    int total;

    if (horas <= 2)
        total = horas * TarifaInicial;
    else if (horas <= 12)
        total = 2 * TarifaInicial + (horas - 2) * TarifaAdicional;
    else
        total = TarifaFija;

    Console.WriteLine($"Total a pagar: ${total}");
}
```

**Pruebas (una por camino + límites)**

| horas | Esperado |
|---|---|
| 0 | Horas inválidas |
| 1 | $15 |
| 2 | $30 |
| 3 | $40 |
| 12 | $130 |
| 13 | $150 |

---

## 3.2. Estructuras repetitivas

Un **ciclo** (o bucle) repite un bloque de instrucciones mientras se cumpla una condición. Cada repetición se llama **iteración**.

Sin ciclos, imprimir los números del 1 al 100 requeriría 100 líneas `Console.WriteLine`. Con un ciclo bastan tres.

### 3.2.1. `while`

Repite **mientras** la condición sea verdadera. La condición se evalúa **antes** de cada iteración, así que el bloque puede ejecutarse **cero** veces.

```csharp
while (condicion)
{
    // se repite mientras condicion sea true
}
```

```csharp
int i = 1;              // 1. inicialización

while (i <= 5)          // 2. condición
{
    Console.WriteLine(i);
    i++;                // 3. actualización
}
// → 1 2 3 4 5 (uno por línea)
```

> ⚠️ **Ciclo infinito**: si olvidas actualizar la variable de control (`i++`), la condición nunca se vuelve falsa y el programa no termina. Detén la ejecución con `Ctrl + C`.

`while` es ideal cuando **no se sabe cuántas veces** se va a repetir:

```csharp
// Pedir un número hasta que sea positivo
Console.Write("Número positivo: ");
int n = int.Parse(Console.ReadLine()!);

while (n <= 0)
{
    Console.Write("Inválido. Intenta de nuevo: ");
    n = int.Parse(Console.ReadLine()!);
}

Console.WriteLine($"Gracias, ingresaste {n}");
```

```csharp
// Contar los dígitos de un número
int numero = 48213;
int digitos = 0;

while (numero > 0)
{
    numero /= 10;     // quita el último dígito
    digitos++;
}

Console.WriteLine(digitos);   // → 5
```

### 3.2.2. `do-while`

Igual que `while`, pero la condición se evalúa **al final**. Por eso el bloque se ejecuta **al menos una vez**.

```csharp
do
{
    // se ejecuta al menos una vez
} while (condicion);   // ⚠️ lleva punto y coma
```

Es la estructura natural para **menús** y **validación de entradas**:

```csharp
int opcion;

do
{
    Console.WriteLine("\n=== MENÚ ===");
    Console.WriteLine("1. Saludar");
    Console.WriteLine("2. Mostrar la hora");
    Console.WriteLine("3. Salir");
    Console.Write("Elige: ");
    opcion = int.Parse(Console.ReadLine()!);

    switch (opcion)
    {
        case 1: Console.WriteLine("¡Hola!"); break;
        case 2: Console.WriteLine(DateTime.Now.ToShortTimeString()); break;
        case 3: Console.WriteLine("Adiós"); break;
        default: Console.WriteLine("Opción inválida"); break;
    }
} while (opcion != 3);
```

```csharp
// Validar con TryParse
int edad;
bool valido;

do
{
    Console.Write("Edad (0-120): ");
    valido = int.TryParse(Console.ReadLine(), out edad) && edad >= 0 && edad <= 120;

    if (!valido)
        Console.WriteLine("Dato inválido");
} while (!valido);
```

### 3.2.3. `for`

Junta en una sola línea la **inicialización**, la **condición** y la **actualización**. Es ideal cuando **se conoce el número de repeticiones**.

```csharp
for (inicialización; condición; actualización)
{
    // cuerpo
}
```

```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine(i);
}
// → 1 2 3 4 5
```

Orden de ejecución:

```
1. int i = 1        (solo una vez)
2. ¿i <= 5?  → sí → ejecuta el cuerpo → i++ → vuelve al paso 2
             → no → sale del ciclo
```

#### Variantes comunes

```csharp
for (int i = 10; i >= 1; i--)         // cuenta regresiva: 10, 9, ..., 1
for (int i = 0; i <= 100; i += 5)     // de 5 en 5: 0, 5, 10, ..., 100
for (int i = 2; i <= 20; i += 2)      // pares del 2 al 20
for (int i = 1; i <= 1000; i *= 2)    // potencias de 2: 1, 2, 4, ..., 512
```

```csharp
// Tabla de multiplicar
Console.Write("Tabla del: ");
int t = int.Parse(Console.ReadLine()!);

for (int i = 1; i <= 10; i++)
{
    Console.WriteLine($"{t} x {i,2} = {t * i,3}");
}
```

#### Comparación de los tres ciclos

| | `while` | `do-while` | `for` |
|---|---|---|---|
| ¿Cuándo evalúa la condición? | Antes | Después | Antes |
| Mínimo de ejecuciones | 0 | **1** | 0 |
| Uso típico | Repeticiones desconocidas (hasta que el usuario…) | Menús, validar entradas | Número de repeticiones conocido, contar, recorrer |
| Ejemplo | Leer hasta que escriba "fin" | Mostrar menú hasta elegir "Salir" | Imprimir del 1 al 100 |

Los tres son equivalentes: cualquier `for` se puede escribir como `while` y viceversa.

```csharp
// for                                  // while equivalente
for (int i = 0; i < 3; i++)             int i = 0;
{                                       while (i < 3)
    Console.WriteLine(i);               {
}                                           Console.WriteLine(i);
                                            i++;
                                        }
```

---

## 3.3. Control de ciclos

### 3.3.1. Uso de `break`

`break` **termina el ciclo inmediatamente** y continúa con la primera instrucción después de él.

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i == 5)
        break;
    Console.Write(i + " ");
}
// → 1 2 3 4
```

Uso típico: **detener una búsqueda** en cuanto se encuentra lo que se buscaba.

```csharp
// ¿Es primo?
Console.Write("Número: ");
int n = int.Parse(Console.ReadLine()!);
bool esPrimo = n > 1;

for (int d = 2; d * d <= n; d++)
{
    if (n % d == 0)
    {
        esPrimo = false;
        break;          // ya encontramos un divisor; no tiene caso seguir
    }
}

Console.WriteLine(esPrimo ? "Es primo" : "No es primo");
```

Uso típico 2: **ciclo controlado por centinela**.

```csharp
int suma = 0;

while (true)          // ciclo "infinito" a propósito
{
    Console.Write("Número (0 para terminar): ");
    int x = int.Parse(Console.ReadLine()!);

    if (x == 0)
        break;        // la única salida

    suma += x;
}

Console.WriteLine($"Suma: {suma}");
```

> `break` solo sale del ciclo (o `switch`) **más interno** donde se encuentra.

### 3.3.2. Uso de `continue`

`continue` **salta el resto de la iteración actual** y pasa directamente a la siguiente.

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
        continue;          // si es par, se lo salta
    Console.Write(i + " ");
}
// → 1 3 5 7 9
```

Uso típico: **ignorar datos inválidos** sin detener el proceso.

```csharp
int sumaPositivos = 0;

for (int i = 1; i <= 5; i++)
{
    Console.Write($"Dato {i}: ");
    int dato = int.Parse(Console.ReadLine()!);

    if (dato < 0)
    {
        Console.WriteLine("  (negativo, se ignora)");
        continue;
    }

    sumaPositivos += dato;
}

Console.WriteLine($"Suma de positivos: {sumaPositivos}");
```

| | `break` | `continue` |
|---|---|---|
| ¿Qué hace? | Sale del ciclo por completo | Salta a la siguiente iteración |
| ¿El ciclo sigue? | No | Sí |
| Uso típico | Búsqueda encontrada, centinela | Ignorar ciertos valores |

> ⚠️ En un `while`, si el `continue` está **antes** de la actualización (`i++`), se produce un ciclo infinito. En `for` no pasa porque la actualización siempre se ejecuta.

---

## 3.4. Contadores y acumuladores

### 3.4.1. Uso de contadores

Un **contador** es una variable que **aumenta (o disminuye) en una cantidad fija**, normalmente 1, cada vez que ocurre algo. Sirve para responder "**¿cuántos?**".

```
contador = contador + 1      →   contador++
```

Reglas:

1. Se **inicializa** (normalmente en 0) **antes** del ciclo.
2. Se **incrementa** dentro del ciclo, solo cuando ocurre el evento.
3. Se **usa** después del ciclo.

```csharp
// ¿Cuántos aprobados hay entre 5 calificaciones?
int aprobados = 0;                      // 1. inicializar

for (int i = 1; i <= 5; i++)
{
    Console.Write($"Calificación {i}: ");
    int cal = int.Parse(Console.ReadLine()!);

    if (cal >= 70)
        aprobados++;                    // 2. incrementar solo si aprobó
}

Console.WriteLine($"Aprobados: {aprobados}");   // 3. usar
```

### 3.4.2. Uso de acumuladores

Un **acumulador** es una variable que **suma (o multiplica) valores variables** en cada iteración. Responde "**¿cuánto en total?**".

```
acumulador = acumulador + valor   →   acumulador += valor
```

| | Contador | Acumulador |
|---|---|---|
| Incremento | Fijo (`+1`) | Variable (`+valor`) |
| Pregunta que responde | ¿Cuántos? | ¿Cuánto suma? |
| Inicialización para sumar | 0 | 0 |
| Inicialización para multiplicar | — | **1** |

```csharp
// Suma de N números
Console.Write("¿Cuántos números? ");
int n = int.Parse(Console.ReadLine()!);
double suma = 0;

for (int i = 1; i <= n; i++)
{
    Console.Write($"Número {i}: ");
    suma += double.Parse(Console.ReadLine()!);
}

Console.WriteLine($"Suma: {suma}");
```

```csharp
// Factorial: acumulador multiplicativo (inicia en 1)
int numero = 5;
long factorial = 1;

for (int i = 1; i <= numero; i++)
{
    factorial *= i;
}

Console.WriteLine($"{numero}! = {factorial}");   // → 5! = 120
```

> ⚠️ Si un acumulador multiplicativo empieza en 0, el resultado siempre será 0.

#### Contadores y acumuladores juntos: promedio

```csharp
int cantidad = 0;      // contador
double suma = 0;       // acumulador

while (true)
{
    Console.Write("Calificación (-1 para terminar): ");
    double cal = double.Parse(Console.ReadLine()!);

    if (cal == -1)
        break;

    cantidad++;
    suma += cal;
}

if (cantidad > 0)
    Console.WriteLine($"Promedio de {cantidad} calificaciones: {suma / cantidad:F2}");
else
    Console.WriteLine("No se ingresaron calificaciones");
```

#### Máximo y mínimo

Otra variante: variables que **guardan el mayor o menor** valor visto hasta el momento.

```csharp
int mayor = int.MinValue;
int menor = int.MaxValue;

for (int i = 1; i <= 5; i++)
{
    Console.Write($"Número {i}: ");
    int x = int.Parse(Console.ReadLine()!);

    if (x > mayor) mayor = x;
    if (x < menor) menor = x;
}

Console.WriteLine($"Mayor: {mayor}, Menor: {menor}");
```

---

## 3.5. Ciclos anidados

### 3.5.1. Uso de ciclos dentro de ciclos

Un **ciclo anidado** es un ciclo dentro de otro. Por **cada** iteración del ciclo externo, el ciclo interno se ejecuta **completo**.

```csharp
for (int i = 1; i <= 3; i++)            // externo
{
    for (int j = 1; j <= 2; j++)        // interno
    {
        Console.WriteLine($"i={i}, j={j}");
    }
}
// → i=1, j=1
// → i=1, j=2
// → i=2, j=1
// → i=2, j=2
// → i=3, j=1
// → i=3, j=2
```

> Total de ejecuciones del cuerpo interno = (vueltas externas) × (vueltas internas) = 3 × 2 = 6.

Piénsalo como un reloj: el ciclo externo son las horas y el interno los minutos; los minutos dan una vuelta completa por cada hora.

#### Tablas de multiplicar del 1 al 10

```csharp
for (int tabla = 1; tabla <= 10; tabla++)
{
    Console.WriteLine($"--- Tabla del {tabla} ---");
    for (int i = 1; i <= 10; i++)
    {
        Console.WriteLine($"{tabla} x {i} = {tabla * i}");
    }
}
```

#### Figuras con asteriscos

```csharp
// Rectángulo 4 x 6
for (int fila = 1; fila <= 4; fila++)
{
    for (int col = 1; col <= 6; col++)
    {
        Console.Write("* ");
    }
    Console.WriteLine();     // salto al terminar cada fila
}
// → * * * * * *
// → * * * * * *
// → * * * * * *
// → * * * * * *
```

```csharp
// Triángulo: el interno depende del externo
int altura = 5;

for (int fila = 1; fila <= altura; fila++)
{
    for (int col = 1; col <= fila; col++)
    {
        Console.Write("* ");
    }
    Console.WriteLine();
}
// → *
// → * *
// → * * *
// → * * * *
// → * * * * *
```

> ⚠️ Usa variables **distintas** para cada ciclo (`i` y `j`, o `fila` y `col`). Reutilizar la misma rompe el conteo.

> ⚠️ `break` dentro del ciclo interno solo sale del **interno**; el externo sigue.

---

## 3.6. Integración de estructuras de control

### 3.6.1. Resolución de problemas combinando secuencia, decisión y repetición

Los programas reales combinan las tres estructuras de la programación estructurada:

| Estructura | Se usa para… |
|---|---|
| **Secuencia** | Pedir datos, calcular, mostrar resultados |
| **Decisión** | Validar, clasificar, elegir una opción |
| **Repetición** | Procesar muchos datos, repetir un menú, reintentar |

#### Ejemplo integrador: control de calificaciones de un grupo

> Pedir el número de alumnos. Para cada uno, leer su nombre y su calificación (validando que esté entre 0 y 100). Al final mostrar: promedio del grupo, cuántos aprobaron y reprobaron, y la calificación más alta con el nombre del alumno.

**Análisis**

| Entradas | Proceso | Salidas |
|---|---|---|
| n, nombre y calificación de cada alumno | Validar, contar aprobados/reprobados, acumular suma, buscar máximo | Promedio, aprobados, reprobados, mejor alumno |

**Código**

```csharp
const int Minima = 70;

// ---------- Secuencia: entrada inicial ----------
int n;
do
{
    Console.Write("Número de alumnos: ");
} while (!int.TryParse(Console.ReadLine(), out n) || n <= 0);   // repetición + validación

int aprobados = 0;            // contador
int reprobados = 0;           // contador
double suma = 0;              // acumulador
double mejorCal = -1;
string mejorAlumno = "";

// ---------- Repetición: un ciclo por alumno ----------
for (int i = 1; i <= n; i++)
{
    Console.Write($"\nNombre del alumno {i}: ");
    string nombre = Console.ReadLine()!;

    double cal;
    do
    {
        Console.Write($"Calificación de {nombre} (0-100): ");
    } while (!double.TryParse(Console.ReadLine(), out cal) || cal < 0 || cal > 100);

    // ---------- Decisión ----------
    if (cal >= Minima)
        aprobados++;
    else
        reprobados++;

    if (cal > mejorCal)
    {
        mejorCal = cal;
        mejorAlumno = nombre;
    }

    suma += cal;
}

// ---------- Secuencia: salida ----------
Console.WriteLine("\n===== RESULTADOS =====");
Console.WriteLine($"Promedio del grupo: {suma / n:F2}");
Console.WriteLine($"Aprobados:          {aprobados}");
Console.WriteLine($"Reprobados:         {reprobados}");
Console.WriteLine($"Mejor calificación: {mejorCal} ({mejorAlumno})");
```

#### Estrategia para problemas integradores

1. **Lee el problema dos veces** y subraya entradas, condiciones y resultados.
2. Pregúntate: *¿qué se repite?* → eso va dentro del ciclo. *¿Qué se decide?* → eso es un `if`/`switch`. *¿Qué se hace una sola vez?* → antes o después del ciclo.
3. Identifica qué **contadores**, **acumuladores** o **máximos/mínimos** necesitas e inicialízalos **antes** del ciclo.
4. Escribe primero el esqueleto (ciclo vacío), pruébalo, y luego agrega la lógica poco a poco.
5. Prueba con casos pequeños que puedas verificar a mano.

---

## Resumen

- `if` ejecuta un bloque si la condición es verdadera; `if-else` elige entre dos caminos; `else if` encadena varios. El orden de las condiciones importa.
- `switch` compara una variable contra valores exactos; en C# cada `case` termina con `break`.
- `while` evalúa antes (0 o más repeticiones); `do-while` evalúa después (al menos 1); `for` es ideal cuando se conoce el número de repeticiones.
- `break` sale del ciclo; `continue` salta a la siguiente iteración.
- Un **contador** suma una cantidad fija (¿cuántos?); un **acumulador** suma valores variables (¿cuánto?). Inicialízalos antes del ciclo.
- En un ciclo anidado, el interno se ejecuta completo por cada vuelta del externo.
- Los programas reales combinan **secuencia + decisión + repetición**.

