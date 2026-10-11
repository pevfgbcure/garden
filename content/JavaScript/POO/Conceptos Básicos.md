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
