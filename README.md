# Lenguajes de Programación — Material del curso

Material de estudio del curso, organizado por unidades. Todos los ejemplos están escritos en **C#** (.NET) y pueden ejecutarse como aplicaciones de consola.

## Contenido

| Unidad | Tema | Archivo |
|---|---|---|
| 1 | Fundamentos de programación estructurada | [unidad-1-fundamentos-programacion-estructurada.md](unidad-1-fundamentos-programacion-estructurada.md) |
| 2 | Tipos de datos, variables y operadores | [unidad-2-tipos-datos-variables-operadores.md](unidad-2-tipos-datos-variables-operadores.md) |
| 3 | Estructuras de control | [unidad-3-estructuras-de-control.md](unidad-3-estructuras-de-control.md) |
| 4 | Estructuras de datos | [unidad-4-estructuras-de-datos.md](unidad-4-estructuras-de-datos.md) |

El temario completo con todos los subtemas está en [lenguajes-programacion-completo.md](lenguajes-programacion-completo.md).

## Estructura del repositorio

```
LenguajesProgramacion/
├── README.md                                         ← este archivo
├── lenguajes-programacion-completo.md                ← temario completo e índice
├── unidad-1-fundamentos-programacion-estructurada.md
├── unidad-2-tipos-datos-variables-operadores.md
├── unidad-3-estructuras-de-control.md
├── unidad-4-estructuras-de-datos.md
└── practica/                                         ← aquí van tus prácticas
```

## Calendario académico

| Evento | Fecha |
|---|---|
| Inicio de clases | 07 de septiembre |
| Examen Primer Parcial | 01 de octubre |
| Examen Segundo Parcial | 05 de noviembre |
| Examen Tercer Parcial | 15 de diciembre |
| Exámenes ordinarios | En espera de coordinación |

## Escala de evaluación (primer, segundo y tercer parcial)

| Rubro | Porcentaje |
|---|---|
| Examen | 40% |
| Trabajo en clase | 30% |
| Proyecto escrito | 30% |

## Cómo usar este material

1. Lee la unidad en orden: cada tema se apoya en el anterior.
2. Copia los ejemplos en un proyecto de consola dentro de `practica/` y ejecútalos; modifícalos para ver qué cambia.

### Crear un proyecto de consola para practicar

```bash
cd practica
dotnet new console -n Unidad1
cd Unidad1
dotnet run
```

> Requiere tener instalado el [SDK de .NET](https://dotnet.microsoft.com/download).

## Convenciones

- El código usa *top-level statements* (C# 9+), así que no siempre verás `class Program` ni `static void Main`.
- Los comentarios `// →` indican lo que imprime la línea en consola.
- Los bloques marcados con ⚠️ señalan errores comunes.
