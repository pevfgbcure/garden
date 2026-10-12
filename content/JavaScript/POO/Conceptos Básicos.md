## ¿Qué es la Programación Orientada a Objetos?

- La Programación Orientada a Objetos (POO) es un **paradigma de programación basado en el concepto de objetos**. Es decir, es una forma de estilizar nuestro código y dicta cómo lo escribimos y organizamos.
- Usamos dichos objetos para **modelar conceptos del mundo real u otros constructos más abstractos** (por ejemplo: un botón, una ventana, un carrito de compras, etc.)
- Los objetos pueden contener datos (**propiedades**) y código (**métodos**). Usando objetos, podemos empacar los datos y el comportamiento de nuestra aplicación en bloques de código estructurados.
- En la POO, los objetos **son piezas o bloques de código autónomos**.
- Los objetos son los bloques de construcción de las aplicaciones y pueden interactuar unos con otros.
- Las interacciones entre objetos **ocurren mediante una interfaz pública** (conocida como API, por su nombre en Inglés); estos son métodos que el código externo puede acceder para comunicarse con el objeto.
- La POO fue creada con el propósito de **organizar nuestro código**, para así hacerlo más flexible y fácil de mantener, con lo que evitamos la proliferación del código espagueti.
## Clases e instancias (POO tradicional)

Podemos pensar en una clase como un plano que puede ser usado para crear nuevos objetos. De esta manera, solo necesitamos definir el código una vez y a partir de esta definición podremos crear múltiples objetos. Como analogía, se puede pensar en la clase como el plano de una casa y cada casa que se construya a partir de este plano será el equivalente a un nuevo objeto en la POO.

El siguiente es un ejemplo de una clase, donde se especifica los tipos de datos que contendrá y las acciones que se podrán realizar con cada usuario. Sin embargo, y esto es muy importante, la definición de la clase no contiene ningún dato del usuario. La clase contiene el plano del usuario, pero no implementa a ningún usuario en específico.

```
Usuario {
	usuario
	contraseña
	correo
	
	iniciarSesión(contraseña) {
		// Lógica de inicio de sesión.
	}
	
	enviarMensaje(mensaje) {
		// Lógica para enviar mensaje.
	}
}
```

A los objetos creados a partir de una clase se les llama **instancias** de una clase. Para crear nuevas instancias, en muchos lenguajes de programación es común utilizar la palabra clave `new` seguida del nombre de la clase y los datos que contendrá. Al proceso por el que se crea una nueva instancia se le conoce como **instanciación**, por ejemplo:

```
usuario1 = new Usuario("Tomás", "abc1234", "tomas@email.com")
usuario2 = new Usuario("Naomi", "lorem123", "naomi@email.com")
```

La instanciación anterior podría darnos el siguiente objeto para `usuario1`:

```
{
	usuario = "Tomás"
	contraseña = "abc1234"
	correo = "tomas@email.com"
	
	iniciarSesión(contraseña) {
		// Lógica de inicio de sesión.
	}
	
	enviarMensaje(mensaje) {
		// Lógica para enviar mensaje.
	}
}
```

Nótese que cada objeto pude contener distintos datos para las propiedades de usuario, contraseña y correo. Sin embargo, los objetos que se desprenden de una clase suelen conservar los mismos métodos que se definieron en la clase (es decir, tienen el mismo comportamiento y funcionalidad). En el ejemplo anterior, todas las instancias de la clase `Usuario` podrán usar los métodos `iniciarSesión` y `enviarMensaje`.

## Los 4 principios fundamentales de la POO

Existen muchas formas en las que podríamos diseñar nuestras clases para modelar el mundo real. De hecho, cada lenguaje de programación tiene sus particularidades en cuanto a la forma en que permite implementar la POO, y JavaScript no es la excepción.

Sin embargo, existen cuatro pilares fundamentales con los que nos podemos guiar para construir nuestras clases, los cuales son independientes del lenguaje de programación que deseemos usar. Estos principios son la **abstracción, encapsulación, herencia y polimorfismo**. 

### Abstracción

Consiste en ignorar u ocultar aquellos detalles que no importan, lo que nos permite obtener una perspectiva generalizada y más simple de lo que estamos implementando, en lugar de enfocarnos en los detalles que realmente no son relevantes para nuestra implementación. La abstracción es usada para crear una separación entre nuestra aplicación y los programas externos que accederán a ella.

Por ejemplo, cuando usamos un microondas para calentar nuestra comida, simplemente apretamos algunos botones para programar el tiempo y tipo de cocción. Los componentes internos del microondas, que son bastante complejos, están ocultos para nosotros y realmente no es importante que sepamos cómo funcionan para poder usar el aparato.

La abstracción es frecuentemente usada en la programación, no solo en este paradigma. Por ejemplo, cuando usamos funciones predefinidas como `console.log()` para mostrar datos en pantalla, el funcionamiento de dicho método nos es desconocido. Nosotros no implementamos esta función ni sabemos lo que hace detrás de escenas; lo único que nos interesa es que produzca el resultado esperado para poder usarla en nuestro código.

### Encapsulación

Consiste en agrupar los datos y los métodos que operan sobre dichos datos en una sola unidad, que se conoce como clase. Esto a su vez nos permite restringir el acceso directo a los componentes internos de un objeto, para evitar que programas externos modifiquen accidentalmente o corrompan la información contenida. Algunos métodos pueden ser expuestos para su uso por otros programas mediante una interfaz pública.

En la práctica, los lenguajes de programación suelen tener modificadores de acceso con los que se puede permitir o restringir la manipulación del estado interno de un objeto. Por ejemplo, en Java se pueden usar `private` y `public` para controlar si los datos o métodos pueden ser accedidos interna o externamente a la clase.

Otra forma de implementar la encapsulación es mediante *getters* y *setters*, los cuales son métodos públicos que permiten ver ("*get*") o modificar ("*set*") el estado interno del objeto. Esto también facilita que el objeto realice algún tipo de validación antes de cambiar su estado.

### Herencia

Este es un mecanismo que permite que una clase (llamada clase hija o subclase) adquiera las propiedades y comportamientos de una clase de orden superior (llamada clase padre o superclase). Esto es útil para evitar la duplicación de código, para modelar relaciones del mundo real y para poder extender la funcionalidad de una clase sin tener que modificar el funcionamiento de otra clase de orden superior.

Como ejemplo de herencia tenemos la siguiente subclase llamada `Administrador`, que se desprende de la clase `User` que definimos anteriormente:

```
Administrador extiende Usuario {
	permisos
	
	borrarUsuario(nombreDeUsuario) {
		// Lógica para borrar un usuario.
	}
}
```

Esta clase no solo tendrá la propiedad `permisos` y el método `borrarUsuario`, sino que también tendrá acceso a las propiedades y métodos que fueron definidos en la clase `Usuario`. Es decir, podrá acceder a los campos de `usuario`, `contraseña` y `correo`, así como a las funciones de `iniciarSesión` y `enviarMensaje`. La herencia nos ahorra el trabajo de tener que volver a implementar esta funcionalidad en la clase hija.