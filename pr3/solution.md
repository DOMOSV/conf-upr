# Практика 3

# _____________________________________________________________
Для решения дальнейших задач потребуется программа на Питоне, представленная ниже. Разбираться в самом языке Питон при этом необязательно.

```Python
import random


def parse_bnf(text):
    '''
    Преобразовать текстовую запись БНФ в словарь.
    '''
    grammar = {}
    rules = [line.split('=') for line in text.strip().split('\n')]
    for name, body in rules:
        grammar[name.strip()] = [alt.split() for alt in body.split('|')]
    return grammar


def generate_phrase(grammar, start):
    '''
    Сгенерировать случайную фразу.
    '''
    if start in grammar:
        seq = random.choice(grammar[start])
        return ''.join([generate_phrase(grammar, name) for name in seq])
    return str(start)


BNF = '''
E = a
'''

for i in range(10):
    print(generate_phrase(parse_bnf(BNF), 'E'))

```

Реализовать грамматики, описывающие следующие языки (для каждого решения привести БНФ). Код решения должен содержаться в переменной BNF:
# _____________________________________________________________



## Задача 3
Язык нулей и единиц

### Решение
```python
BNF = '''
E = 0 | 1 E | 1
'''
```
![image](https://github.com/user-attachments/assets/ce62189e-58fb-4883-9914-9afc3dec4d2f)





## Задача 4
Язык правильно расставленных скобок двух видов

### Решение
```python
BNF = '''
E = {} | () | ( E ) | { E } | E E
'''
```
![image](https://github.com/user-attachments/assets/732992d7-66f9-420b-b221-b012ffe0858a)




## Задача 5
Язык выражений алгебры логики

### Решение
```python
BNF = '''
E = ( E B F ) | U ( E ) | F
F = P B P | U P | P
P = x | y | (x) | (y)
U = ~
B = & | V
'''
```

![image](https://github.com/user-attachments/assets/14eafc54-b7ea-4b8a-ae9b-a5211c2d7cd6)

