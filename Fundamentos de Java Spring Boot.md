# Fundamentos de Java Spring Boot

## INDICE:

- Qué es Spring Boot?
- Características Principales de Spring Boot

-------------------------------------------------------------------------------------------------

## ¿Qué es Spring Boot?

- Proyecto basado en Spring. No es lo mismo que Spring. Es un proyecto que forma parte del core de Spring, al igual que Spring Cloud, etc.

- El objetivo principal es que sólo te centres en "correr" la aplicación, sin preocuparte por temas de configuración, etc.

- Tiene la gran ventaja poder integrar librerías de terceros de manera muy sencilla.

- No tendremos que preocuparnos por configuraciones a nivel de XML, sólo configuraciones mínimas a nivel de properties (ponerle el puerto, etc).

- No tendremos que preocuparnos de desplegar nuestra aplicación en un servidor web local cuando queramos hacer pruebas, Spring Boot también contempla esto y lleva incorporado un servidor web dónde se desplegará nuestra aplicación automáticamente.

-------------------------------------------------------------------------------------------------

## Características principales de Spring Boot

- Independiente: 

	No tenemos que preocuparnos de las dependencias del core de Spring ni de la compilación de estas.

- Incrustado Tomcat, Jetty o Undertow: 

	Spring Boot trae consigo un servidor web como los tres mencionados donde podemos correr nuestra aplicación sin preocuparnos de generar un artefacto WAR o JAR y desplegarlo nosotros mismos.

- Proporción de dependencias: 
	
	No debemos preocuparnos por las configuraciones de dependencias de terceros o del core de Spring, Spring Boot se encargará de inyectar todo lo necesario.

- Sin generación de XML: 

	No debemos preocuparnos de configuración XML para que nuestra aplicación funcione.

- Métricas de salud del aplicativo: 

	Podemos validar el estado de un servicio desplegado, sus dependencias, estado de memoria, magnitud de configuración, etc.

-------------------------------------------------------------------------------------------------

## ¿Qué es una Dependencia?

- Son objetos definidos como una funcionalidad. Sin dicha funcionalidad los otros objetos no podrán trabajar, ya que "dependen" de ellas.

- Se puede definir como una pequeña característica de un objeto especifico, que puede impactar de manera particular en el funcionamiento de una unidad.

- Al trabajar con dependencias, se pueden trabajar en cada una por individual como módulos.

EJEMPLO: 
	- Tenemos una aplicación "Carro" cuyo objetivo es moverse. 
	- Para cumplir el objetivo, el carro "depende" de poseer ruedas, motor, etc. 
	- Dichas características se interrelacionan entre sí para cumplir el objetivo. 

Nota: Hay dependencias que tienen mayor impacto dentro de la aplicación, en este caso, una rueda tiene mayor impacto que una puerta.

### SISTEMAS DE ALTA COHESIÓN Y BAJO ACOPLAMIENTO

- Alta cohesión: 

	Involucra que la entidad ejecute sus acciones sin involucrar otra clase o entidad.

- Bajo acoplamiento y Alto acoplamiento:

	Hablamos de acoplamiento bajo cuando existe una independencia entre los componentes entre si, por el contrario un alto acoplamiento es cuando tenemos varias dependencias relacionadas a un solo componente.

Entonces podemos afirmar que en la definición de un buen diseño de software se debe tener una ALTA COHESIÓN y un BAJO ACOPLAMIENTO.

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------
