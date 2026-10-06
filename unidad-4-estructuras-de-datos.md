# Unidad 4. Estructuras de datos

Hasta ahora cada variable guardaba **un solo valor**. Si necesitas las calificaciones de 40 alumnos, declarar `cal1`, `cal2`, …, `cal40` es impráctico. Una **estructura de datos** es una forma de organizar **muchos valores bajo un mismo nombre** para poder guardarlos, recorrerlos y procesarlos con ciclos.

En esta unidad se ven las dos más básicas: **arreglos** y **listas**.

## 4.1. Arreglos (*arrays*)

### 4.1.1. Concepto de arreglo

Un **arreglo** es una colección de elementos:

- del **mismo tipo** (todos `int`, todos `string`…),
- guardados en posiciones **contiguas** de memoria,
- de **tamaño fijo** (se decide al crearlo y no cambia),
- a los que se accede por un **índice** numérico que empieza en **0**.

```
calificaciones
┌──────┬──────┬──────┬──────┬──────┐
│  85  │  92  │  78  │  64  │  90  │   ← valores (elementos)
└──────┴──────┴──────┴──────┴──────┘
   [0]    [1]    [2]    [3]    [4]     ← índices
```

Este arreglo tiene **longitud 5**; sus índices van del **0 al 4** (longitud − 1).

### 4.1.2. Declaración e inicialización

#### Con tamaño (valores por defecto)

```csharp
int[] numeros = new int[5];          // 5 enteros, todos en 0
string[] nombres = new string[3];    // 3 cadenas, todas en null
bool[] asistencias = new bool[10];   // 10 bool, todos en false
```

| Tipo | Valor por defecto |
|---|---|
| `int`, `double`, `decimal` | `0` |
| `bool` | `false` |
| `char` | `'\0'` (carácter nulo) |
| `string` y objetos | `null` |

#### Con valores iniciales

```csharp
int[] calificaciones = { 85, 92, 78, 64, 90 };
string[] dias = { "Lun", "Mar", "Mié", "Jue", "Vie" };

// Formas equivalentes
int[] a = new int[] { 1, 2, 3 };
int[] b = new int[3] { 1, 2, 3 };
int[] c = [1, 2, 3];                  // expresión de colección (C# 12+)
```

#### Tamaño leído en ejecución

```csharp
Console.Write("¿Cuántos alumnos? ");
int n = int.Parse(Console.ReadLine()!);

double[] notas = new double[n];       // el tamaño puede venir de una variable
```

> ⚠️ Una vez creado, el arreglo **no puede crecer ni encogerse**. Si necesitas tamaño variable, usa una lista (sección 4.3).

### 4.1.3. Acceso a elementos

Se usa el nombre del arreglo y el índice entre corchetes `[ ]`.

```csharp
int[] calificaciones = { 85, 92, 78, 64, 90 };

// Leer
Console.WriteLine(calificaciones[0]);   // → 85 (primero)
Console.WriteLine(calificaciones[4]);   // → 90 (último)

// Modificar
calificaciones[3] = 70;                 // el 64 ahora es 70

// Longitud
Console.WriteLine(calificaciones.Length);                       // → 5
Console.WriteLine(calificaciones[calificaciones.Length - 1]);   // → 90 (último)
Console.WriteLine(calificaciones[^1]);                          // → 90 (último, C# 8+)
```

> ⚠️ Acceder a un índice fuera de rango (`calificaciones[5]` o `calificaciones[-1]`) lanza `IndexOutOfRangeException`. El último índice válido es siempre `Length - 1`.

### 4.1.4. Recorrido de arreglos

**Recorrer** un arreglo es visitar todos sus elementos, uno por uno, normalmente con un ciclo.

#### Con `for` (cuando necesitas el índice)

```csharp
int[] calificaciones = { 85, 92, 78, 64, 90 };

for (int i = 0; i < calificaciones.Length; i++)
{
    Console.WriteLine($"Alumno {i + 1}: {calificaciones[i]}");
}
```

> ⚠️ La condición es `i < Length`, **no** `i <= Length` (eso se sale del arreglo).

#### Con `foreach` (solo para leer)

```csharp
foreach (int cal in calificaciones)
{
    Console.WriteLine(cal);
}
```

| | `for` | `foreach` |
|---|---|---|
| Conoces el índice | Sí (`i`) | No |
| Puedes modificar elementos | Sí (`arr[i] = …`) | **No** |
| Puedes recorrer al revés o saltar | Sí | No |
| Más simple de escribir | No | Sí |

#### Recorridos típicos (con contadores y acumuladores de la Unidad 3)

```csharp
int[] datos = { 12, 7, 25, 3, 18, 9 };

int suma = 0;
int mayor = datos[0];
int pares = 0;

for (int i = 0; i < datos.Length; i++)
{
    suma += datos[i];                    // acumulador
    if (datos[i] > mayor) mayor = datos[i];
    if (datos[i] % 2 == 0) pares++;      // contador
}

Console.WriteLine($"Suma: {suma}");                              // → 74
Console.WriteLine($"Promedio: {(double)suma / datos.Length:F2}"); // → 12.33
Console.WriteLine($"Mayor: {mayor}");                            // → 25
Console.WriteLine($"Pares: {pares}");                            // → 2
```

```csharp
// Recorrido inverso
for (int i = datos.Length - 1; i >= 0; i--)
{
    Console.Write(datos[i] + " ");
}
// → 9 18 3 25 7 12
```

### 4.1.5. Arreglos unidimensionales

Todos los ejemplos anteriores son **unidimensionales** (o **vectores**): una sola fila de elementos, un solo índice.

#### Llenar un arreglo con datos del usuario

```csharp
Console.Write("¿Cuántas temperaturas? ");
int n = int.Parse(Console.ReadLine()!);

double[] temps = new double[n];

// Llenado
for (int i = 0; i < n; i++)
{
    Console.Write($"Temperatura del día {i + 1}: ");
    temps[i] = double.Parse(Console.ReadLine()!);
}

// Procesamiento
double suma = 0;
foreach (double t in temps)
    suma += t;
double promedio = suma / n;

// Reporte: qué días estuvieron arriba del promedio
Console.WriteLine($"\nPromedio: {promedio:F1}");
for (int i = 0; i < n; i++)
{
    if (temps[i] > promedio)
        Console.WriteLine($"Día {i + 1}: {temps[i]} (arriba del promedio)");
}
```

> Este último reporte es la razón de ser de los arreglos: sin guardar los valores, no podrías compararlos contra el promedio después de calcularlo.

#### Arreglos paralelos

Dos o más arreglos del mismo tamaño en los que el **mismo índice** se refiere al mismo elemento:

```csharp
string[] productos = { "Lápiz", "Cuaderno", "Borrador" };
decimal[] precios  = { 8.50m,   45.00m,     6.00m };
int[] existencias  = { 120,     35,         80 };

for (int i = 0; i < productos.Length; i++)
{
    Console.WriteLine($"{productos[i],-10} {precios[i],8:C} {existencias[i],5}");
}
```

#### Métodos útiles de la clase `Array`

| Método | Qué hace | Ejemplo |
|---|---|---|
| `Array.Sort(a)` | Ordena de menor a mayor | `Array.Sort(numeros);` |
| `Array.Reverse(a)` | Invierte el orden | `Array.Reverse(numeros);` |
| `Array.IndexOf(a, x)` | Índice de `x` (−1 si no está) | `Array.IndexOf(dias, "Mié")` → `2` |
| `Array.Exists(a, cond)` | ¿Algún elemento cumple? | `Array.Exists(n, x => x > 10)` |
| `Array.Fill(a, v)` | Llena todo con un valor | `Array.Fill(numeros, -1);` |
| `Array.Copy(a, b, n)` | Copia `n` elementos de `a` a `b` | `Array.Copy(a, b, a.Length);` |
| `a.Contains(x)` | ¿Contiene `x`? (requiere `System.Linq`) | `numeros.Contains(7)` |
| `a.Sum()`, `a.Max()`, `a.Min()`, `a.Average()` | Cálculos (LINQ) | `numeros.Average()` |

> Conviene saber programar a mano la búsqueda y el ordenamiento (sección 4.2) antes de usar estos atajos.

### 4.1.6. Arreglos bidimensionales

Un **arreglo bidimensional** (o **matriz**) organiza los datos en **filas y columnas**, como una tabla. Cada elemento se identifica con **dos índices**: `[fila, columna]`.

```
               columna
             0     1     2
          ┌─────┬─────┬─────┐
fila  0   │  1  │  2  │  3  │
          ├─────┼─────┼─────┤
      1   │  4  │  5  │  6  │
          └─────┴─────┴─────┘

m[0, 2] = 3      m[1, 0] = 4
```

#### Declaración

```csharp
int[,] m = new int[2, 3];          // 2 filas, 3 columnas, todo en 0

int[,] matriz =
{
    { 1, 2, 3 },                   // fila 0
    { 4, 5, 6 }                    // fila 1
};
```

#### Acceso y dimensiones

```csharp
Console.WriteLine(matriz[1, 2]);        // → 6
matriz[0, 0] = 10;

int filas = matriz.GetLength(0);        // → 2
int columnas = matriz.GetLength(1);     // → 3
int total = matriz.Length;              // → 6 (todos los elementos)
```

> ⚠️ En una matriz, `Length` da el **total** de elementos (filas × columnas), no el número de filas. Usa `GetLength(0)` y `GetLength(1)`.

#### Recorrido con ciclos anidados

El ciclo externo recorre las **filas** y el interno las **columnas** (Unidad 3.5).

```csharp
for (int f = 0; f < matriz.GetLength(0); f++)
{
    for (int c = 0; c < matriz.GetLength(1); c++)
    {
        Console.Write($"{matriz[f, c],4}");
    }
    Console.WriteLine();
}
// →   10   2   3
// →    4   5   6
```

#### Ejemplo: calificaciones de varios alumnos en varias materias

```csharp
string[] alumnos = { "Ana", "Luis", "Sofía" };
string[] materias = { "Mat", "Prog", "Inglés" };

int[,] cal =
{
    { 90, 85, 78 },     // Ana
    { 70, 95, 88 },     // Luis
    { 82, 76, 91 }      // Sofía
};

// Encabezado
Console.Write($"{"Alumno",-8}");
foreach (string m in materias)
    Console.Write($"{m,8}");
Console.WriteLine($"{"Prom",8}");

// Filas: promedio por alumno
for (int f = 0; f < cal.GetLength(0); f++)
{
    Console.Write($"{alumnos[f],-8}");
    int suma = 0;

    for (int c = 0; c < cal.GetLength(1); c++)
    {
        Console.Write($"{cal[f, c],8}");
        suma += cal[f, c];
    }

    Console.WriteLine($"{(double)suma / cal.GetLength(1),8:F1}");
}

// → Alumno       Mat    Prog  Inglés    Prom
// → Ana           90      85      78    84.3
// → Luis          70      95      88    84.3
// → Sofía         82      76      91    83.0
```

```csharp
// Promedio por materia: ahora el externo recorre COLUMNAS
for (int c = 0; c < cal.GetLength(1); c++)
{
    int suma = 0;
    for (int f = 0; f < cal.GetLength(0); f++)
        suma += cal[f, c];

    Console.WriteLine($"{materias[c]}: {(double)suma / cal.GetLength(0):F1}");
}
```

#### Arreglos escalonados (*jagged*)

C# también permite "arreglos de arreglos", donde cada fila puede tener distinta longitud:

```csharp
int[][] escalonado =
{
    new int[] { 1, 2 },
    new int[] { 3, 4, 5, 6 },
    new int[] { 7 }
};

Console.WriteLine(escalonado[1][2]);     // → 5  (nota: [ ][ ], no [ , ])
Console.WriteLine(escalonado[1].Length); // → 4
```

---

## 4.2. Operaciones con arreglos

### 4.2.1. Búsqueda básica

Buscar es determinar **si un valor está** en el arreglo y, si está, **en qué posición**.

#### Búsqueda secuencial (lineal)

Revisa los elementos **uno por uno** desde el principio hasta encontrarlo o llegar al final. Funciona con arreglos **ordenados o no**.

```csharp
int[] datos = { 34, 7, 23, 32, 5, 62 };

Console.Write("Número a buscar: ");
int buscado = int.Parse(Console.ReadLine()!);

int posicion = -1;                     // -1 significa "no encontrado"

for (int i = 0; i < datos.Length; i++)
{
    if (datos[i] == buscado)
    {
        posicion = i;
        break;                         // ya lo encontramos
    }
}

if (posicion != -1)
    Console.WriteLine($"Encontrado en el índice {posicion}");
else
    Console.WriteLine("No está en el arreglo");
```

| Caso | Comparaciones (con *n* elementos) |
|---|---|
| Mejor (está al inicio) | 1 |
| Peor (al final o no está) | *n* |

#### Búsqueda binaria

Solo funciona si el arreglo está **ordenado**. Compara con el elemento del **centro** y descarta la mitad donde no puede estar; repite con la mitad restante.

```
Buscar 23 en: [ 5, 7, 23, 32, 34, 62 ]
               ↑        ↑          ↑
              ini      mid        fin     mid = 23 → ¡encontrado!

Buscar 34:     ini=0, fin=5 → mid=2 (23)  34 > 23 → buscar a la derecha
               ini=3, fin=5 → mid=4 (34)  ¡encontrado!
```

```csharp
int[] ordenado = { 5, 7, 23, 32, 34, 62 };
int buscado = 34;

int inicio = 0;
int fin = ordenado.Length - 1;
int posicion = -1;

while (inicio <= fin)
{
    int medio = (inicio + fin) / 2;

    if (ordenado[medio] == buscado)
    {
        posicion = medio;
        break;
    }
    else if (ordenado[medio] < buscado)
        inicio = medio + 1;      // está en la mitad derecha
    else
        fin = medio - 1;         // está en la mitad izquierda
}

Console.WriteLine(posicion);     // → 4
```

| | Secuencial | Binaria |
|---|---|---|
| ¿Requiere arreglo ordenado? | No | **Sí** |
| Comparaciones (peor caso, 1 000 elementos) | 1 000 | ~10 |
| Complejidad | Sencilla | Un poco más compleja |

### 4.2.2. Ordenamiento básico

Ordenar es reacomodar los elementos de menor a mayor (ascendente) o de mayor a menor (descendente).

#### Intercambio de dos elementos

Todos los algoritmos de ordenamiento básicos usan el intercambio con variable auxiliar (Unidad 1.4.2):

```csharp
int temp = arr[i];
arr[i] = arr[j];
arr[j] = temp;
```

#### Método de la burbuja (*bubble sort*)

Compara cada par de elementos **vecinos** y los intercambia si están en el orden incorrecto. En cada pasada, el mayor "sube" al final, como una burbuja.

```
Pasada 1:  [5, 3, 8, 1]  → compara 5-3 → intercambia → [3, 5, 8, 1]
                         → compara 5-8 → ok          → [3, 5, 8, 1]
                         → compara 8-1 → intercambia → [3, 5, 1, 8]  ← 8 en su lugar
Pasada 2:  [3, 5, 1, 8]  → 3-5 ok → 5-1 intercambia  → [3, 1, 5, 8]  ← 5 en su lugar
Pasada 3:  [3, 1, 5, 8]  → 3-1 intercambia           → [1, 3, 5, 8]  ✔
```

```csharp
int[] arr = { 5, 3, 8, 1, 9, 2 };
int n = arr.Length;

for (int pasada = 0; pasada < n - 1; pasada++)
{
    for (int j = 0; j < n - 1 - pasada; j++)   // los últimos ya están ordenados
    {
        if (arr[j] > arr[j + 1])               // usa < para orden descendente
        {
            int temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
        }
    }
}

Console.WriteLine(string.Join(", ", arr));     // → 1, 2, 3, 5, 8, 9
```

#### Método de selección (*selection sort*)

En cada pasada busca el **menor** de la parte no ordenada y lo coloca al inicio.

```csharp
int[] arr = { 5, 3, 8, 1, 9, 2 };

for (int i = 0; i < arr.Length - 1; i++)
{
    int indiceMenor = i;

    for (int j = i + 1; j < arr.Length; j++)
    {
        if (arr[j] < arr[indiceMenor])
            indiceMenor = j;
    }

    // intercambiar el menor encontrado con la posición i
    int temp = arr[i];
    arr[i] = arr[indiceMenor];
    arr[indiceMenor] = temp;
}

Console.WriteLine(string.Join(", ", arr));     // → 1, 2, 3, 5, 8, 9
```

#### Con métodos de C#

```csharp
int[] arr = { 5, 3, 8, 1, 9, 2 };

Array.Sort(arr);                    // ascendente: 1, 2, 3, 5, 8, 9
Array.Reverse(arr);                 // descendente: 9, 8, 5, 3, 2, 1

string[] nombres = { "Luis", "Ana", "Sofía" };
Array.Sort(nombres);                // alfabético: Ana, Luis, Sofía
```

| Algoritmo | Idea | Pasadas | Intercambios |
|---|---|---|---|
| Burbuja | Compara vecinos e intercambia | n − 1 | Muchos |
| Selección | Busca el menor y lo pone al inicio | n − 1 | Máximo n − 1 |
| `Array.Sort` | Algoritmo optimizado de .NET | — | — (mucho más rápido) |

> ⚠️ Si ordenas uno de varios **arreglos paralelos**, debes hacer los mismos intercambios en los demás o se pierde la correspondencia entre ellos.

---

## 4.3. Listas

### 4.3.1. Concepto de lista

Una **lista** es una colección de elementos del mismo tipo que, a diferencia del arreglo, tiene **tamaño dinámico**: puede **crecer y encogerse** mientras el programa se ejecuta.

En C# se usa `List<T>`, donde `T` es el tipo de los elementos (espacio de nombres `System.Collections.Generic`, incluido por defecto en proyectos de consola modernos).

```csharp
List<string> nombres = new List<string>();     // lista vacía
List<int> numeros = new List<int> { 4, 8, 15 }; // con valores iniciales
var precios = new List<decimal>();             // con var
List<int> otra = [1, 2, 3];                    // expresión de colección (C# 12+)
```

### 4.3.2. Diferencias entre arreglo y lista

| Característica | Arreglo `T[]` | Lista `List<T>` |
|---|---|---|
| Tamaño | **Fijo**, se define al crearlo | **Dinámico**, crece y se encoge |
| Agregar elementos | No se puede (solo cambiar los existentes) | `Add`, `Insert` |
| Eliminar elementos | No se puede | `Remove`, `RemoveAt`, `Clear` |
| Cantidad de elementos | `.Length` | `.Count` |
| Acceso por índice | `arr[i]` | `lista[i]` |
| Dimensiones | 1, 2 o más | Una (aunque puede haber listas de listas) |
| Rendimiento | Ligeramente más rápido y ligero | Muy rápido, con pequeño costo extra |
| Cuándo usar | Cantidad conocida y fija (días de la semana, matriz de 3×3) | Cantidad desconocida o que cambia (carrito de compras, registros que el usuario agrega) |

```csharp
// Arreglo: hay que saber el tamaño de antemano
int[] arr = new int[3];
arr[0] = 10;
// arr[3] = 40;   ⚠️ IndexOutOfRangeException: no cabe

// Lista: se agregan los que sean
List<int> lista = new List<int>();
lista.Add(10);
lista.Add(20);
lista.Add(30);
lista.Add(40);    // ✔ crece sola
Console.WriteLine(lista.Count);   // → 4
```

Conversión entre ambos:

```csharp
int[] arreglo = { 3, 1, 2 };
List<int> comoLista = new List<int>(arreglo);   // arreglo → lista
int[] deVuelta = comoLista.ToArray();           // lista → arreglo
```

### 4.3.3. Operaciones básicas (agregar, eliminar, modificar)

#### Agregar

```csharp
List<string> frutas = new List<string>();

frutas.Add("Manzana");                   // al final
frutas.Add("Plátano");
frutas.Insert(0, "Fresa");               // en una posición específica
frutas.AddRange(new[] { "Uva", "Kiwi" }); // varios a la vez

// frutas → [Fresa, Manzana, Plátano, Uva, Kiwi]
```

#### Eliminar

```csharp
frutas.Remove("Plátano");    // elimina la primera aparición de ese valor (devuelve true/false)
frutas.RemoveAt(0);          // elimina por índice
frutas.Clear();              // vacía la lista
```

```csharp
// Eliminar todos los que cumplan una condición
List<int> nums = new List<int> { 5, 12, 8, 3, 20 };
nums.RemoveAll(x => x < 10);          // → [12, 20]
```

> ⚠️ `RemoveAt` con un índice inexistente lanza `ArgumentOutOfRangeException`. Verifica antes que `indice >= 0 && indice < lista.Count`.

#### Modificar

```csharp
List<string> tareas = new List<string> { "Estudiar", "Comer", "Dormir" };

tareas[1] = "Desayunar";              // por índice
Console.WriteLine(tareas[1]);         // → Desayunar

int idx = tareas.IndexOf("Dormir");   // buscar y reemplazar
if (idx != -1)
    tareas[idx] = "Descansar";
```

#### Consultar y buscar

| Método / propiedad | Qué hace | Ejemplo |
|---|---|---|
| `Count` | Número de elementos | `frutas.Count` |
| `Contains(x)` | ¿Está `x`? | `frutas.Contains("Uva")` |
| `IndexOf(x)` | Índice de `x` (−1 si no está) | `frutas.IndexOf("Kiwi")` |
| `Find(cond)` | Primer elemento que cumple | `nums.Find(x => x > 10)` |
| `Exists(cond)` | ¿Alguno cumple? | `nums.Exists(x => x < 0)` |
| `Sort()` | Ordena la lista | `nums.Sort();` |
| `Reverse()` | Invierte el orden | `nums.Reverse();` |

> Las expresiones `x => x > 10` se llaman **lambdas**: son funciones cortas que reciben `x` y devuelven una condición.

### 4.3.4. Recorrido de listas

Igual que con arreglos, con `for` (usando `Count`) o con `foreach`.

```csharp
List<string> alumnos = new List<string> { "Ana", "Luis", "Sofía" };

// for: con índice
for (int i = 0; i < alumnos.Count; i++)
{
    Console.WriteLine($"{i + 1}. {alumnos[i]}");
}

// foreach: más simple
foreach (string a in alumnos)
{
    Console.WriteLine(a);
}
```

> ⚠️ No se puede **agregar ni eliminar** elementos de una lista mientras se recorre con `foreach` (lanza `InvalidOperationException`). Si necesitas eliminar mientras recorres, usa un `for` **de atrás hacia adelante** o `RemoveAll`.

```csharp
List<int> nums = new List<int> { 1, 2, 3, 4, 5, 6 };

// ⚠️ Mal
// foreach (int x in nums)
//     if (x % 2 == 0) nums.Remove(x);

// ✔ Bien: de atrás hacia adelante
for (int i = nums.Count - 1; i >= 0; i--)
{
    if (nums[i] % 2 == 0)
        nums.RemoveAt(i);
}
// nums → [1, 3, 5]
```

#### Ejemplo integrador: lista de compras

```csharp
List<string> compras = new List<string>();
int opcion;

do
{
    Console.WriteLine("\n=== LISTA DE COMPRAS ===");
    Console.WriteLine("1. Agregar   2. Eliminar   3. Modificar   4. Ver   5. Salir");
    Console.Write("Opción: ");
    int.TryParse(Console.ReadLine(), out opcion);

    switch (opcion)
    {
        case 1:
            Console.Write("Producto: ");
            string nuevo = Console.ReadLine()!.Trim();
            if (nuevo != "" && !compras.Contains(nuevo))
                compras.Add(nuevo);
            else
                Console.WriteLine("Vacío o repetido");
            break;

        case 2:
            Console.Write("Producto a eliminar: ");
            if (!compras.Remove(Console.ReadLine()!.Trim()))
                Console.WriteLine("No estaba en la lista");
            break;

        case 3:
            Console.Write("Número del producto a modificar: ");
            if (int.TryParse(Console.ReadLine(), out int num) && num >= 1 && num <= compras.Count)
            {
                Console.Write("Nuevo nombre: ");
                compras[num - 1] = Console.ReadLine()!.Trim();
            }
            else
                Console.WriteLine("Número inválido");
            break;

        case 4:
            if (compras.Count == 0)
                Console.WriteLine("(lista vacía)");
            for (int i = 0; i < compras.Count; i++)
                Console.WriteLine($"{i + 1}. {compras[i]}");
            break;
    }
} while (opcion != 5);
```

---

## Resumen

- Un **arreglo** guarda muchos valores del mismo tipo bajo un nombre, con **tamaño fijo** e índices de `0` a `Length - 1`.
- Se recorre con `for` (si necesitas el índice o modificar) o `foreach` (solo lectura).
- Una **matriz** `[,]` tiene filas y columnas; se recorre con ciclos anidados usando `GetLength(0)` y `GetLength(1)`.
- **Búsqueda secuencial**: revisa uno por uno (cualquier arreglo). **Binaria**: divide a la mitad (solo arreglos ordenados).
- **Burbuja** compara vecinos; **selección** busca el menor. En la práctica se usa `Array.Sort`.
- Una **lista** `List<T>` tiene **tamaño dinámico**: `Add`, `Insert`, `Remove`, `RemoveAt`, `Count`. Úsala cuando no sabes cuántos elementos habrá.

