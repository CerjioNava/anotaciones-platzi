# FUNDAMENTOS DE NODE.JS

# INDICE:

1. Conceptos Básicos de NodeJS:

	- Orígenes y Filosofía
	- EventLoop: Asíncrona por diseño
	- Monohilo: Implicaciones en diseño y seguridad
	- Variables de entorno
	- Nodemon

2. Manejo de Asincronía

	- Callbacks
	- Callback Hell: Refactorización
	-
	-
	-



-------------------------------------------------------------------------------------------------

## ORIGENES Y FILOSOFÍA

Es un entorno de ejecución de Javascript creado en 2009, orientado a servidores y es una de las formas más rápidas y escalables para correr código desde el servidor.

Puedo ejecutarlo en servidores, para herramientas, transpiladores, scrapping, automatización, etc. 

### Características de NodeJS

1. Concurrente (Varios a la vez): 

	- Monohilo con entradas y salidas asíncronas. 
	- Un proceso por cada núcleo del procesador.

2. Motor V8:
	
	- Entorno de ejecución de JS, creado por Google y liberado en el 2008.
	- Escrito en C++.
	- Convierte el Javascript en código máquina en lugar de interpretarlo (Más rápido).

3. Módulos:

	- Todo lo que no sea sintaxis propia de programación, son módulos.
	- Muchos módulos vienen por defecto en el paquete de Node.
	- Puedes crear tus propios módulos. 

4. Orientado a eventos:

	- Hay un bucle de eventos que se ejecuta constantemente.
	- Puedes orientar tu código de forma reactiva (interacciones puntuales).

-------------------------------------------------------------------------------------------------

## EVENTLOOP: ASÍNCRONA POR DISEÑO

Un Event Loop es un proceso con un bucle que gestiona, de forma asíncrona, todos los eventos de tu aplicación.

1. Event Queue (Cola de eventos): 
	
	Contiene todos los eventos que se generan por nuestro código (Funciones, peticiones, etc.), estos eventos quedan en una cola que van pasando uno a uno al Event Loop.

2. Event Loop (Bucle de eventos): 

	Se encarga de resolver los eventos ultra rápidos que llegan desde el Event Queue. En caso de no poder resolverse rápido, enviá el evento al Thread Pool. Todo evento del código para por acá, sean funciones, request, etc.

3. Thread Pool (Piscina de hilos): 
	
	Se encarga de gestionar los eventos de forma asíncrona. Una vez terminado lo devuelve al Event Loop. El Event Loop vera si lo pasa a Event Queue o no. 
	Aquí se manda eventos lentos, como gestiones a bases de datos, lecturas de archivos, etc.
	Para cada petición en el Thread Pool, se crea un nuevo Thread para terminar cada evento por separado.

-------------------------------------------------------------------------------------------------

## MONOHILO: IMPLICACIONES EN DISEÑO Y SEGURIDAD

El hecho de que sea monohilo lo hace delicado en el sentido de que puede ejecutarse algo que corte el código y detenga el programa, como la ausencia de sintaxis o una variable pendiente por definir.

Código de ejemplo:

	console.log('Hola mundo');

	// SetInterval me permtie ejecutar una función cada n cantidad de tiempo en milisegundos.
	// Esta instrucción es asíncrona, por lo que se ejecuta en n momento.

	let i = 0;
	setInterval(function() {
	    console.log(i);
	    i++;

		// Al ser monohilo el peligro recae en que si el código se ejecuta y ocurre un error, se puede detener el proceso. Comprobar funciones y revisar lo que posiblemente puede fallar.
		// Todo lo que sea asíncono y lo pueda llevar a ello, pues llevalo, y todo lo que no, revisar que no corte el programa.

	    // if (i === 5) {
	    //     console.log('forzamos error');
	    //     var a = 3 + z;
	    // }
	}, 1000);

	console.log('Segunda instrucción');

-------------------------------------------------------------------------------------------------

## VARIABLES DE ENTORNO

Las variables de entorno son una forma de llamar información de afuera a nuestro software, sirve para definir parámetros sencillos de configuración de los programas de modo que puedan ejecutarse en diferentes ambiente sin necesidad de modificar el código fuente de un script.

- Process: El objeto "process" nos da información sobre el procesos que está ejecutando este script.

- Env: La propiedad "env" es la que nos da acceso a las variables de entorno de manera sencilla.

Ejemplo:

	let ejemplo ="Alejandro-sin" //Esta variable la declaré aquí y puedo llamarla aquí.

	// ¿Qué ocurre cuando quiero llamar un valor que no va dentro del software, si quiereo llamar una API, o que necesito una clave, o un token?

	// El código no  debería guardar este tipo de valores credenciales y por esto existen las variables de entorno. Si grabasemos estos datos estaríamos obligados a cambiar el código del programa cada vez que se ejecutan en lugares diferentes, dificultando el despliegue.

	// Por buenas prácticas heredadas de Linux las variables de entorno que vengan desde fuera (process.env.VARIABLE ) se ponen en mayúscula y se separan mediante guion bajo en vez de espacio.

	let nombre = process.env.NOMBRE || 'Sin nombre'; 
	let web = process.env.MI_WEB || 'no tengo web';
	var apiKey = process.env.APIKEY || 'HF33o9oERVERVEEEEs';

	console.log('Hola '+ nombre);
	console.log('Mi web es '+ web);

	// Me retornara { Hola "Sin nombre", mi web es "no tengo web"} ya que tiene  la expresión OR || me asigna estas variables por defecto

	// Desde la terminal (bash) asignarle las variables antes del codigo:

	< NOMBRE:"Alejandro" MI_WEB"123QWERTY" APIKEY=10937472 node conceptos/entorno.js >


-------------------------------------------------------------------------------------------------

## NODEMON

Nodemon (Demons en linux), es una herramienta que nos ayuda a detectar los cambios y los ejecutarlos automáticamente.

	npm install -g nodemon

### PM2

PM2 nos ayuda a ver los datos de nuestra aplicacion en producción, como el uso de CPU, Memoria, cuantas veces se ha reiniciado, etc. No es recomendable ocupar PM2 en desarrollo ya que con la cantidad de datos y controles nos dará mas problemas que soluciones.

	npm install -g pm2

Para detener el proceso que está en ejecución con un ID:

	pm2 stop + id

NOTA: Nodemon es mejor para desarrollo, PM2 es mejor para producción.

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------
