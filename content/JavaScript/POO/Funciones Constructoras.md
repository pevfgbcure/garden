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
    return `${nombre} tiene ${edad} años.`;
  }
};

// Crea un nuevo objeto usando la función constructora.
const persona1 = new Persona("Juan", 23);
```

>[!info] ¿Por qué no agregar métodos a `this`?
> Porque cada instancia creada a partir de la función constructora tendría su propia copia del método, lo cual podría usar demasiada memoria en caso de que queramos crear múltiples instancias. Para solucionar esto, se usa la cadena de prototipos y su capacidad de delegar funcionamiento.

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