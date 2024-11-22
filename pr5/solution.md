# Практическое занятие №5. Вопросы виртуализации

П.Н. Советов, РТУ МИРЭА

## Задача 1

Исследование виртуальной стековой машины CPython.

Изучите возможности просмотра байткода ВМ CPython.

```
import dis

def foo(x):
    while x:
        x -= 1
    return x + 1

print(dis.dis(foo))
```

Опишите по шагам, что делает каждая из следующих команд (приведите эквивалентное выражение на Python):

 11           0 LOAD_FAST                0 (x)
              2 LOAD_CONST               1 (10)
              4 BINARY_MULTIPLY
              6 LOAD_CONST               2 (42)
              8 BINARY_ADD
             10 RETURN_VALUE

```
LOAD_FAST 0 (x):
Команда LOAD_FAST загружает локальную переменную x в стек. В данном случае, индекс переменной 0 соответствует переменной x.

LOAD_CONST 1 (10):
Команда LOAD_CONST загружает константу 10 в стек.

BINARY_MULTIPLY:
Команда BINARY_MULTIPLY умножает два верхних значения на стеке и помещает результат обратно на стек. Операции выполняются над переменными x и 10, которые были ранее загружены.

LOAD_CONST 2 (42):
Команда LOAD_CONST загружает константу 42 в стек.

BINARY_ADD:
Команда BINARY_ADD складывает два верхних значения на стеке и помещает результат обратно на стек. Операции выполняются над результатом умножения и константой 42.

RETURN_VALUE:
Команда RETURN_VALUE возвращает верхнее значение из стека в качестве результата выполнения функции.

Итог
result = (x * 10) + 42
return result
```
## Задача 2

Что делает следующий байткод (опишите шаги его работы)? Это известная функция, назовите ее.

```
  5           0 LOAD_CONST               1 (1)
              2 STORE_FAST               1 (r)

  6     >>    4 LOAD_FAST                0 (n)
              6 LOAD_CONST               1 (1)
              8 COMPARE_OP               4 (>)
             10 POP_JUMP_IF_FALSE       30

  7          12 LOAD_FAST                1 (r)
             14 LOAD_FAST                0 (n)
             16 INPLACE_MULTIPLY
             18 STORE_FAST               1 (r)

  8          20 LOAD_FAST                0 (n)
             22 LOAD_CONST               1 (1)
             24 INPLACE_SUBTRACT
             26 STORE_FAST               0 (n)
             28 JUMP_ABSOLUTE            4

  9     >>   30 LOAD_FAST                1 (r)
             32 RETURN_VALUE
```
```
LOAD_CONST 1 (1) Загружает константу 1 в стек. Это начальное значение для переменной r.

STORE_FAST 1 (r) Сохраняет значение 1 из стека в локальную переменную r.

LOAD_FAST 0 (n) Загружает значение переменной n в стек.

LOAD_CONST 1 (1) Загружает константу 1 в стек.

COMPARE_OP 4 (>) Сравнивает n и 1 на предмет того, больше ли n единицы.

POP_JUMP_IF_FALSE 30 Если результат сравнения n > 1 — False, то переход к строке 30 (к завершению функции). Иначе выполняется тело цикла.

Тело цикла (строки 12–28): LOAD_FAST 1 (r) Загружает значение r в стек.

LOAD_FAST 0 (n) Загружает значение n в стек.

INPLACE_MULTIPLY Умножает r на n и сохраняет результат в r.

STORE_FAST 1 (r) Обновляет значение переменной r на r * n.

LOAD_FAST 0 (n) Загружает значение n в стек.

LOAD_CONST 1 (1) Загружает константу 1 в стек.

INPLACE_SUBTRACT Уменьшает n на 1.

STORE_FAST 0 (n) Обновляет значение n на n - 1.

JUMP_ABSOLUTE 4 Переходит обратно к началу цикла (в строку 4), чтобы снова проверить условие n > 1.

Завершение функции: LOAD_FAST 1 (r) Загружает значение r в стек (это результат вычисления факториала).

RETURN_VALUE Возвращает значение r как результат выполнения функции.


def factorial(n):
    r = 1
    while n > 1:
        r *= n
        n -= 1
    return r
```
## Задача 3

Приведите результаты из задач 1 и 2 для виртуальной машины JVM (Java) или .Net (C#).

Java
```bash
public int foo(int x) {
    return x * 10 + 42;
}
```

Байткод Java
```bash
public int foo(int);
  Code:
     0: iload_1         // Загружает x
     1: bipush        10  // Константа 10
     3: imul             // Умножение
     4: bipush        42  // Константа 42
     6: iadd             // Сложение
     7: ireturn          // Возврат результата
```

Java
```bash
public int factorial(int n) {
    int r = 1;
    while (n > 1) {
        r *= n;
        n -= 1;
    }
    return r;
}
```

Байткод Java
```bash
public int factorial(int);
  Code:
     0: iconst_1        // r = 1
     1: istore_2
     2: iload_1         // Загружаем n
     3: iconst_1
     4: if_icmple 17    // Если n <= 1, перейти к 17
     7: iload_2         // Загружаем r
     8: iload_1         // Загружаем n
     9: imul            // r * n
    10: istore_2        // r = r * n
    11: iload_1         // Загружаем n
    12: iconst_1
    13: isub            // n - 1
    14: istore_1        // n = n - 1
    15: goto 2          // Переход к началу цикла
    17: iload_2         // Загружаем r
    18: ireturn         // Возврат r
```

C#
```bash
public int Foo(int x)
{
    return x * 10 + 42;
}
```

IL-код C#
```bash
.method public hidebysig instance int32 Foo(int32 x) cil managed
{
    .maxstack 2
    IL_0000: ldarg.1      // Загружаем x
    IL_0001: ldc.i4.s 10  // Константа 10
    IL_0003: mul          // Умножение
    IL_0004: ldc.i4.s 42  // Константа 42
    IL_0006: add          // Сложение
    IL_0007: ret          // Возврат результата
}
```

C#
```bash
public int Factorial(int n)
{
    int r = 1;
    while (n > 1)
    {
        r *= n;
        n -= 1;
    }
    return r;
}
```

IL-код C#
```bash
.method public hidebysig instance int32 Factorial(int32 n) cil managed
{
    .maxstack 2
    .locals init ([0] int32 r)
    IL_0000: ldc.i4.1      // r = 1
    IL_0001: stloc.0
    IL_0002: br.s IL_0009  // Переход к проверке условия

    IL_0004: ldloc.0       // Загружаем r
    IL_0005: ldarg.1       // Загружаем n
    IL_0006: mul           // r * n
    IL_0007: stloc.0       // r = r * n
    IL_0008: ldarg.1       // Загружаем n

    IL_0009: ldc.i4.1      // Константа 1
    IL_000a: sub           // n - 1
    IL_000b: starg.s n     // n = n - 1

    IL_000d: ldarg.1       // Загружаем n
    IL_000e: ldc.i4.1      // Константа 1
    IL_000f: bgt.s IL_0004 // Если n > 1, перейти к началу цикла

    IL_0011: ldloc.0       // Загружаем r
    IL_0012: ret           // Возврат r
}
```
