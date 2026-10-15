
En [[Conceptos Básicos]] se explicó la forma tradicional en la que se aplica la Programación Orientada a Objetos (POO), que se pude resumir en el proceso de diseñar clases y usar dichas clases como planos de construcción para generar múltiples objetos similares entre sí, en un proceso llamado instanciación. No obstante, la forma en la que se implementa la POO en JavaScript es bastante diferente de modelo tradicional anteriormente descrito.
## Prototipos

La manera en que se implementa la POO en JavaScript parte del concepto de prototipos. Los prototipos son en sí mismos otros objetos ya implementados y a partir de los cuales podemos vincular nuestros propios objetos para que hereden sus características (propiedades) y/o comportamientos (funciones). 

A lo anterior se le llama **herencia prototípica**, pues los métodos y propiedades del prototipo son accesibles para todos los objetos que estén enlazados a ese prototipo. También se le conoce como **delegación**, pues el objeto vinculado delega su comportamiento al prototipo al cual se enlazó.

En el siguiente ejemplo el arreglo `numbers` tiene acceso al método `map`, a pesar de que no especificamos de manera explícita el funcionamiento de este método:

```js
const numbers = [1, 2, 3]
numbers.map(number => number * 2)
```

Esto se debe a que todos los arreglos en JavaScript son objetos que están vinculados al prototipo `Array.prototype`, el cual sí implementa el método `map`:

![[Pasted image 20261015003356.png|378]]
![[Pasted image 20261015003543.png]]

## Las 3 formas de implementar herencia prototípica

### Funciones constructoras

- Permite crear nuevos objetos a partir de una función, la cual se encarga de establecer el prototipo.
- Esta es la forma en la que JavaScript implementa objetos que forman parte del lenguaje (como `Arrays`, `Maps` y `Sets`).
- Es la manera tradicional en la que se ha implementado la POO en JavaScript desde que se creó el lenguaje.
### Clases de ECMAScript 6 (ES6)

- ES6 introdujo las clases a JavaScript, las cuales son una alternativa a la sintaxis de funciones constructoras.
- Es importante señalar que las clases son simplemente "azúcar sintáctica", pues detrás de escenas **funcionan exactamente igual** que las funciones constructoras y no se comportan como en la POO tradicional de otros lenguajes de programación.
### Object.create()

- Es la forma más fácil y directa de enlazar un objeto con su prototipo, pero es la menos usada en comparación con los otros dos métodos anteriores.