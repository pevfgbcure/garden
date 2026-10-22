Como se mencionó [[La Programación Orientada a Objetos en JavaScript|anteriormente]], existen tres formas en las que podemos implementar la POO en JavaScript, y la más común es usando funciones constructoras.

Las funciones constructoras no son otra cosa que las típicas funciones que podemos crear en JavaScript (ya sea usando la sintaxis de declaración o de expresión). Sin embargo, no podemos crear funciones constructoras usando la sintaxis de funciones flecha, ya que estas funciones no tienen su propio `this` definido sino que lo heredan del entorno.

Lo que realmente hace diferente a las funciones constructoras es que con ellas podemos usar `new` para crear un nuevo objeto, por ejemplo:

```js
const Persona = function(nombre, edad) {
  // Propiedades del objeto:
  this.nombre = nombre;
  this.edad = edad;
  
  // Podemos agregar métodos, pero esto no es recomendable:
  this.muestraPersona = function() {
    return `${this.nombre} tiene ${this.edad} años.`;
  }
};

// Crea un nuevo objeto usando la función constructora.
const persona1 = new Persona("Juan", 23);
```

>[!info] ¿Por qué no agregar métodos a `this`?
> Porque cada instancia creada a partir de la función constructora tendría su propia copia del método, lo cual podría usar demasiada memoria en caso de que queramos crear múltiples instancias. Para solucionar esto, se usa la cadena de prototipos y su capacidad de delegar funcionamiento. Esto se explica en la siguiente sección.

Cuando usamos la sintaxis `new`, detrás de escenas ocurren cuatro pasos clave:

1. Un nuevo objeto es creado (sin atributos ni métodos).
2. La función es llamada y la palabra clave `this` apunta al nuevo objeto, en su contexto de ejecución.
3. El nuevo objeto se vincula a un prototipo.
4. La función retorna de manera automática el nuevo objeto que se creó.

El último paso es la clave del funcionamiento de esta técnica, pues como la palabra `this` quedó apuntando al nuevo objeto, todo lo que forme parte de `this` estará vinculado al nuevo objeto. En el ejemplo anterior, el texto `Juan` y el número `23` quedarán vinculados con las propiedades `nombre` y `edad` de la instancia `persona1`. Asimismo, el método `muestraPersona` formará parte del objeto y podrá ser llamado como cualquier otra función:

![https://res.cloudinary.com/s49rzrxq/image/upload/v1792478434/Captura_de_pantalla_2026-10-20_004015.png](https://res.cloudinary.com/s49rzrxq/image/upload/v1792478434/Captura_de_pantalla_2026-10-20_004015.png)

Por último, para comprobar si un objeto es la instancia de una función constructora, podemos usar `instanceof` (el cual devuelve `True` o `False`). Por ejemplo:

```js
console.log(persona1 instanceof Persona); // Verdadero.

const persona2 = "Tobias";
console.log(persona2 instanceof Persona); // Falso, es un valor primitivo no una instancia.
```

## Los prototipos

Para entender los prototipos, es importante tener en cuenta que todas las funciones en JavaScript, no únicamente las funciones constructoras, tienen de manera automática una propiedad llamada `prototype`. Esto permite que todos los objetos que sean creados a partir de cierta función constructora hereden los métodos y propiedades que hayan sido definidos en su `prototype`.

Revisitando el ejemplo de la sección anterior, podemos definir métodos y propiedades para el `prototype` de la función constructora haciendo lo siguiente:

```js
const Persona = function(nombre, edad) {
  // Propiedades del objeto:
  this.nombre = nombre;
  this.edad = edad;
};

// Agregamos métodos haciendo lo siguiente:
Persona.prototype.calcularNacimiento = function(fechaActual) {
  return fechaActual - this.edad;
};
// Para añadir propiedades al prototipo:
Persona.prototype.especie = "Homo Sapiens";
```

Lo anterior permite que las instancias que generemos tengan acceso al mismo `prototype`, sin necesidad de copiar métodos o propiedades que rápidamente podrían llenar la memoria de nuestra computadora. De igual manera, la palabra clave `this` apuntará únicamente al objeto del que forma parte.

En el siguiente ejemplo, `persona1` y `persona2` tienen acceso al mismo `prototype` que acabamos de definir, por lo que pueden hacer uso de la función `calcularNacimiento`:

![https://res.cloudinary.com/s49rzrxq/image/upload/v1792646317/Captura_de_pantalla_2026-10-21_231825.png](https://res.cloudinary.com/s49rzrxq/image/upload/v1792646317/Captura_de_pantalla_2026-10-21_231825.png)

### ¿Cómo funcionan los prototipos?

Cada objeto tiene una propiedad llamada `__proto__`, que lo vincula a cierto prototipo en especifico, dándole acceso a las propiedades y métodos que ahí se definan. Es decir, le da acceso al objeto a las direcciones de memoria donde se encuentran esas propiedades y métodos:

![https://res.cloudinary.com/s49rzrxq/image/upload/v1792647291/Captura_de_pantalla_2026-10-21_233431.png](https://res.cloudinary.com/s49rzrxq/image/upload/v1792647291/Captura_de_pantalla_2026-10-21_233431.png)

Cuando creamos un nuevo objeto, éste es vinculado al `prototype` de su función constructora de manera automática por el motor de JavaScript. Por lo tanto, el `__proto__` de la instancia contendrá exactamente lo mismo que definamos para el `prototype` de la función constructora.

> [!info] Diferencia entre `__proto__` y `prototype`
> Aquí suele surgir una duda común, pues la función constructora también tiene un atributo `__proto__`, pero este atributo no es modificado cuando nosotros agregamos funcionalidad a `prototype`. El `prototype` de una función constructora solo es usado al momento de crear una nueva instancia, para enlazar el nuevo objeto a la cadena de prototipos, pero no modifica el `__proto__` de la propia función constructora.

Para saber si un objeto esta vinculado al `prototype` de una función constructora podemos usar `isProtorypeOf`, por ejemplo:

```js
Persona.prototype.isPrototypeOf(persona1); // Verdadero
Persona.prototype.isPrototypeOf(Persona); // Falso
```

Por último, si queremos saber si una propiedad fue heredada del `prototype` o forma parte de los atributos internos del objeto usamos `hasOwnProperty`:

```js
persona1.hasOwnProperty("nombre") // Verdadero
persona1.hasOwnProperty("especie") // Falso, es una propiedad heredada del prototipo
```