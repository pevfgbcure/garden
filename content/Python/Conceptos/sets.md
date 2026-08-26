---
title: Sets (conjuntos)
date: 2025-11-06
---
Los conjuntos (o _sets_) son **colecciones [mutables](https://realpython.com/python-mutable-vs-immutable-types/) de objetos sin un orden específico**, a diferencia de las listas o las tuplas. Los elementos dentro de un _set_ deben de ser [_hasheables_](https://docs.python.org/3.7/glossary.html#term-hashable), por lo que un _set_ no puede contener listas o diccionarios, por ejemplo.

Cada miembro de un _set_ **debe ser distinto**, es decir, no se permiten miembros duplicados. Un _set_ puede contener diferentes estructuras de datos como miembros o incluso estructuras anidadas (p. ej. una tupla que contiene tuplas). Sin embargo, todos los elementos de un _set_ deben de ser *hasheables*. Otra variante de los _sets_ son los [_frozensets_](https://docs.python.org/3/library/stdtypes.html#frozenset), los cuales son objetos inmutables.
## Principales usos

Los _sets_ son comúnmente usados para remover de manera rápida los miembros duplicados dentro de otras estructuras de datos. También son usados para comparar eficientemente objetos cuando la secuencia de los elementos o mantener el registro de los miembros duplicados no es relevante.
## Operaciones soportadas

Al igual que otros tipos de colecciones (diccionarios, listas, tuplas), los _sets_ soportan:

- Iteración de sus elementos por medio de `for <miembro> in <set>`
- Comprobar si un elemento se encuentra en el _set_ mediante `in` y `not in`
- Cálculo de su tamaño usando `len()`
- Copias superficiales (_shallow copies_) a través de `copy()`

Por otro lado, los _sets_ no permiten:

- La indexación de sus elementos
- La ordenación de sus elementos o la inserción de nuevos elementos en un índice en específico
- Obtener "rebanadas" del contenido (p. ej. usando la notación `[start:stop:step]` de las listas)
- Concatenación mediante el operador "+"

Las comprobaciones de pertenencia en los _sets_ (p. ej. `<miembro> in <set>`) tienen una complejidad de tiempo constante (en promedio) si lo comparamos con hacer lo mismo en una lista o cadena (_string_), donde la complejidad temporal tiende a aumentar mientras más aumente el tamaño de los datos contenidos. Otros métodos como `<set>.union()`, `<set>.intersection()` o `<set>.difference()` también tienen (en promedio) una complejidad de tiempo constante.

## Creando sets

### Usando literales

Un _set_ puede crearse directamente usando la notación de conjunto con llaves `{}` y poniendo comas entre los elementos. A esta notación se le llama _literal_. Cualquier elemento duplicado es ignorado silenciosamente:

```python
>>> un_elemento = {'Hola'}
>>> un_elemento
{'Hola'}

# El orden de los elementos no siempre se conserva.
>>> multiples_elementos = {'A', 'B', 'C', 'D', 'E'}
>>> multiples_elementos
{'B', 'E', 'D', 'C', 'A'}

# Los duplicados son eliminados.
>>> con_duplicados = {1, 2, 2, 3, 3, 3}
>>> con_duplicados
{1, 2, 3}
```

Los literales de conjuntos usan la misma notación que los `dict` literales. Por esta razón, para crear un conjunto vacío es necesario usar el constructor `set()`.

### Usando el constructor

El constructor de la clase `set`, `set()`, puede ser usado con cualquier estructura de datos [iterable](https://docs.python.org/3/glossary.html#term-iterable) que sea pasada como argumento. El constructor procesa los elementos del iterable y los añade al conjunto de manera individual. No se conserva el orden de los elementos y cualquier duplicado es omitido:

```python
# Para crear un conjunto vacío se usa el constructor de clase.
>>> conjunto_vacio = set()
>>> conjunto_vacio
set()

# La tupla es desempaquetada, cada elemento es añadido al conjunto
#   y los duplicados son removidos.
>>> conjunto_de_tupla = set(('abeja', 231, False, 'abeja'))
>>> conjunto_de_tupla
{False, 'abeja', 231}

# La lista es desempaquetada, cada elemento es añadido al conjunto
#   y los duplicados son removidos.
>>> conjunto_de_lista = set([1, 2, 2, 3, 3, 3])
>>> conjunto_de_lista
{1, 2, 3}
```

### Complicaciones al crear conjuntos

Debido a su comportamiento al desempaquetar iterables, el uso de `set()` puede dar lugar a resultados inesperados:

```python
# Los elementos de la cadena de texto son añadidos individualmente
#   sin conservar duplicados.
>>> conjunto_de_texto = set('A mimir')
>>> conjunto_de_texto
{'i', 'A', 'r', 'm', ' '}

# Los separadores de Unicode y otros códigos de posición (como la letra ñ)
#   también son añadidos individualmente.
>>> conjunto_de_texto_unicode = set('Ok mañana')
>>> conjunto_de_texto_unicode
{'k', 'm', 'a', 'n', 'ñ', ' ', 'O'}
```

Los _sets_ pueden contener diferentes estructuras de datos y estructuras anidadas. Sin embargo, todos los elementos de un _set_ deben de ser _hasheables_, de lo contrario ocurrirá in error:

```python
# Si intentamos incluir listas como miembros de un conjunto,
#   se producirá un TypeError.
>>> conjunto_con_listas = {['A', 'B'], ['C', 'D', 'E']}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    conjunto_con_listas = {['A', 'B'], ['C', 'D', 'E']}
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: unhashable type: 'list'

# Lo mismo pasa si intentamos agregar otros conjuntos como miembros de
#   un conjunto, ya que los conjuntos son mutables y no son hasheables.
>>> conjunto_con_conjuntos = {{1, 2}, {3, 4, 5}}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    conjunto_con_conjuntos = {{1, 2}, {3, 4, 5}}
                             ^^^^^^^^^^^^^^^^^^^
TypeError: unhashable type: 'set'
```

## Trabajando con conjuntos

Los _sets_ tienen métodos que se asemejan a las operaciones que podemos realizar con conjuntos matemáticos (unión, intersección, diferencia, etc.). La mayoría de estos métodos tienen operadores equivalentes (`>=`, `<=`, `-`, `^`, etc.). Los métodos generalmente pueden tomar cualquier iterable como argumento, mientras que los operadores requieren que ambos miembros de la operación sean `sets` o `frozensets`.

### Conjuntos disjuntos

El método `<set>.isdisjoint(<otro_iterable>)` es utilizado para comprobar si los elementos de un conjunto se solapan con los elementos de otro conjunto. Este método puede aceptar cualquier iterable o conjunto como argumento. Devuelve `True` si los dos conjuntos **no tienen ningún elemento en común** y `False` si comparten elementos. No existe un operador equivalente para este método:

```python
# Tanto vertebrados como otros_vertebrados son listas.
>>> vertebrados = ['Ballena', 'Oveja', 'Víbora', 'Iguana', 'Pato']
>>> otros_vertebrados = ['Cocodrilo', 'Elefante', 'Canario']
# Mamíferos es un diccionario.
>>> mamiferos = {'Ballena': 'azul', 'Oveja': 'blanca', 'Elefante': 'gris'}
# Invertebrados es un conjunto.
>>> invertebrados = {'Mosca', 'Oruga', 'Mariposa'}

# Invertebrados y vertebrados no comparten ningún elemento.
>>> invertebrados.isdisjoint(vertebrados)
True

# Invertebrados tampoco comparte elementos con otros_vertebrados.
>>> invertebrados.isdisjoint(otros_vertebrados)
True

# Mamíferos y vertebrados SÍ comparten elementos.
# NOTA: Como mamíferos es un diccionario, primero debemos transformarlo
#   a un conjunto usando set() para poder usar el método isdisjoint().
>>> set(mamiferos).isdisjoint(vertebrados)
False
```

### Subconjuntos y superconjuntos

Para comprobar si todos los elementos de un conjunto (`<set>`) se encuentran también se dentro de otro conjunto (`<otro_iterable>`) se utiliza el método `<set>.issubset(<otro_iterable>)`.  Otra forma de realizar esta misma operación es usando el operador `<set> <= <otro_set>`, pero ambos miembros de la operación deben ser _sets_:

```python
# Vertebrados es una lista, mamíferos es un diccionario e invertebrados
#   es un conjunto.
>>> vertebrados = ['Ballena', 'Oveja', 'Víbora', 'Iguana', 'Pato', 'Elefante']
>>> mamiferos = {'Ballena': 'azul', 'Oveja': 'blanca', 'Elefante': 'gris'}
>>> invertebrados = {'Mosca', 'Oruga', 'Mariposa'}

# issubset() es un método de <set> que puede tomar cualquier iterable
#   como argumento. Como mamiferos es un diccionario, primero debemos
#   convertirlo a un set. Todos los miembros de mamiferos también son
#   miembros de los vertebrados, por lo que obtenemos True.
>>> set(mamiferos).issubset(vertebrados)
True

# Ambos objetos deben ser sets para poder usar el operador `<=`.
>>> set(mamiferos) <= set(vertebrados)
True
>>> invertebrados <= set(vertebrados)
False

# Algo curioso es que un conjunto siempre es subconjunto de sí mismo.
>>> invertebrados <= invertebrados
True
```

`<set>.issuperset(<otro_iterable>)` es la operación inversa de `issubset()`. Podemos utilizar este método para comprobar si todos los elementos de `<otro_iterable>` también se encuentran en `<set>`. El operador correspondiente para este método es `<set> >= <otro_set>`:

```python
# Usando las mismas colleciones de objetos anteriores (vertebrados, mamiferos e invertebrados).

# Vertebrados necesita ser convertido a <set> para poder usar el método
#   issuperset(). Por otro lado, este método acepta cualquier iterable
#   como argumento. Todos los miembros de mamiferos también aparecen en
#   vertebrados, por lo que obtenemos True.
>>> set(vertebrados).issuperset(mamiferos)
True

# Para poder usar el operador `>=` ambos miembros de la operación deben
#   ser <sets>.
>>> set(vertebrados) >= set(mamiferos)
True

# No todos los miembros del conjunto de vertebrados aparecen en el conjunto
#   de mamiferos, por lo que mamiferos no es superconjunto de vertebrados.
>>> set(mamiferos) >= set(vertebrados)
False

# Un conjunto siempre es superconjunto de sí mismo.
>>> invertebrados >= invertebrados
True
```
### Intersecciones de conjuntos

Dos conjuntos se intersecan si ambos tienen elementos en común. El método `<set>.intersection(*<otros_iterables>)` devuelve un nuevo conjunto con elementos comunes al conjuntos original y a todos los `<otros_iterables>` (es decir, devuelve el conjunto en el que todo se interseca). El operador usado para esta operación es "`&`" (_ampersand_), por ejemplo: `<set> & <otro_set> & <otro_set_2> & ... & <otro_set_n>`.

```python
# nombres_profesores es un <set> y nombres_alumnos es una lista.
>>> nombres_profesores = {'Ana', 'Eva', 'Juan', 'José', 'Mario', 'Iván'}
>>> nombres_alumnos = ['Juan', 'María', 'Leo', 'Ana', 'Linda', 'Iván']

# intersection() es un método que puede tomar cualquier iterable o iterables
#   como argumento.
>>> nombres_profesores.intersection(nombres_alumnos)
{'Juan', 'Ana', 'Iván'}

# El operador `&` requiere que todos los miembros de la operación sean <sets>.
>>> nombres_profesores & set(nombres_alumnos)
{'Juan', 'Ana', 'Iván'}
```

### Uniones de conjuntos

El método `<set>.union(*<otros_iterables>)` devuelve un nuevo conjunto con todos los elementos de `<set>` y todos los elementos en `<otros_iterables>`. El operador equivalente para este método es "`|`" (_pipe_), por ejemplo: `<set> | <set_2> | ... | <set_n>`.

```python
# mamiferos es un <set>, reptiles es una lista y aves es una tupla.
>>> mamiferos = {'Orca', 'Perro', 'Capibara'}
>>> reptiles = ['Iguana', 'Tortuga', 'Víbora']
>>> aves = ('Cuervo', 'Lechuza', 'Loro')

# El método union() puede tomar iterables como argumento(s).
>>> vertebrados = mamiferos.union(reptiles, aves)
>>> vertebrados
{'Orca', 'Tortuga', 'Lechuza', 'Iguana', 'Víbora', 'Capibara', 'Loro', 'Perro', 'Cuervo'}

# El operador `|` requiere que los miembros de la operación sean <sets>.
>>> mamiferos | set(reptiles) | set(aves)
{'Orca', 'Tortuga', 'Lechuza', 'Víbora', 'Iguana', 'Capibara', 'Loro', 'Perro', 'Cuervo'}
```

### Diferencias de conjuntos

El método `<set>.difference(*<otros_iterables>)` devuelve un nuevo conjunto con los elementos del conjunto original (`<set>`) que no se encuentran en `<otros_iterables>`. El operador equivalente para esta operación es "`-`", por ejemplo: `<set> - <set_2> - ... <set_n>`.

```python
# medios_de_transporte es un <set> y medios_terrestres es una tupla.
>>> medios_de_transporte = {'automóvil', 'camión', 'avión', 'bicicleta', 'cohete', 'barco', 'lancha'}
>>> medios_terrestres = ('automóvil', 'camión', 'bicicleta')

# El método difference() puede tomar cualquier número de iterables como argumento.
>>> medios_no_terrestres = medios_de_transporte.difference(medios_terrestres)
>>> medios_no_terrestres
{'barco', 'cohete', 'lancha', 'avión'}

# El operador `-` solo puede ser usado en <sets>.
>>> medios_de_transporte - set(medios_terrestres)
{'avión', 'barco', 'cohete', 'lancha'}
```

### Diferencia simétrica

Otra forma de obtener la diferencia de dos conjuntos es usando `<set>.symmetric_difference(<otro_iterable>)`. Este método devuelve un nuevo conjunto que contiene **elementos que están ya sea en `<set>` o en `<otro_iterable>`, pero no en ambos**. El operador que realiza esta misma operación es "`^`" (*caret*), por ejemplo: `<set> ^ <otro_set>`.

```python
# pizza_mexicana es un <set> y pizza_vegetariana es una tupla.
>>> pizza_mexicana = {'chorizo', 'chile jalapeño', 'cebolla', 'quesillo', 'pimiento morrón'}
>>> pizza_vegetariana = ('pimiento morrón', 'mozzarella', 'cebolla', 'calabacín', 'berenjena')

# El método puede tomar cualquier iterable como argumento.
>>> ingredientes_diferentes = pizza_mexicana.symmetric_difference(pizza_vegetariana)
>>> ingredientes_diferentes
{'calabacín', 'chorizo', 'mozzarella', 'chile jalapeño', 'berenjena', 'quesillo'}

# El operador requiere que ambos miembros sean <sets>.
>>> pizza_mexicana ^ set(pizza_vegetariana)
{'calabacín', 'mozzarella', 'chile jalapeño', 'berenjena', 'quesillo', 'chorizo'}
```

### Diferencia simétrica con más de dos conjuntos

Si realizamos una diferencia simétrica con más de dos conjuntos obtendremos como resultado un conjunto que incluye tanto los elementos exclusivos de cada conjunto como los elementos compartidos entre más de dos conjuntos de la serie.

Por lo tanto, para obtener solo los elementos exclusivos de cada conjunto de la serie, es necesario que en un primer paso obtengamos las intersecciones entre todas las uniones de dos conjuntos, y luego las eliminemos. Por ejemplo, supongamos que un grupo de tres amigos se van a reunir para tener un picnic y cada uno lleva lo siguiente:

```python
ana = {'ensalada', 'limonada', 'brownies', 'frituras'}
juan = {'pizza', 'frituras', 'refresco', 'brownies'}
mario = {'ensalada', 'refresco', 'galletas', 'brownies'}
```

Si quisiéramos obtener los platillos que son únicos para los tres conjuntos, intuitivamente podríamos hacer esto:

```python
platillos_unicos = ana ^ juan ^ mario
platillos_unicos
{'pizza', 'galletas', 'brownies', 'limonada'}
```

Lo anterior nos da un resultado incorrecto, ya que "brownies" no es un platillo único (los tres amigos van a llevar brownies). Esto ocurre porque la diferencia simétrica incluye los elementos únicos de cada conjunto y todos los elementos que son compartidos por los tres conjuntos. Para solucionar este problema, debemos hacer lo siguiente:

```python
# Paso 1: Agregar todas las intersecciones entre pares de conjuntos y unir
#   estos conjuntos en un único conjunto.
interseccion_aj = ana & juan
interseccion_jm = juan & mario
interseccion_ma = mario & ana
platillos_compartidos = interseccion_aj | interseccion_jm | interseccion_ma

# Paso 2: Obtener la unión de todos los conjuntos originales.
todos_los_platillos = ana | juan | mario

# Paso 3: Eliminar los elementos compartidos del conjunto total de los
#   elementos originales.
platillos_unicos = todos_los_platillos - platillos_compartidos
platillos_unicos
{'limonada', 'pizza', 'galletas'}
```
