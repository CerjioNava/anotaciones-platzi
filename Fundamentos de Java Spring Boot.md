# Fundamentos de Java Spring Boot

## INDICE:

1. Introducción a Spring Boot

	- Qué es Spring Boot?
	- Características Principales de Spring Boot

2. Dependencias en Spring Boot

	- Qué es una Dependencia?
	- Inversión de Control y el patrón de Inyección de Dependencias
	- Autoconfiguration y Runtime
	- Anotaciones para indicar dependencias en Spring Boot
	- Creación de proyecto bajo arquitectura de dependencias
	- Inyección de dependencia "Component"
	- Ejemplo de creación de dependencia propia

3. Configuración general de Spring Boot

	- Cambio de puerto y path
	- Uso de properties y valores
	- Uso de properties con ejemplo de generación de POJO
	- Qué son los logs y cómo usarlos

4. JPA con Spring y Spring Data

	- Modelado de entidades con JPA
	- Configuración de datasource con properties y classes
	- Registro en base de datos con JpaRepository
	- Uso de JPQL en anotación query
	- Uso de anotación value para apuntar a properties
	- Obtención de información utilizando Query methods
	- Uso de Query methods con Or, And, OrderBy, Between, Sort
	- Uso de Query methods con named parameters
	- Uso de anotación transactional
	- Rollback con la anotación transactional

5. REST con Spring Boot

	- CRUD bajo arquitectura REST
	- Métodos CREATE, UPDATE y DELETE
	- Probando la API REST
	- Pagination con Spring Boot

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

## Inversión de Control y el patrón de inyección de dependencias

### Inversión de control

- Es un principio que transfiere el control (de flujo) de objetos de un programa a un contenedor o framework.

- Un contenedor puede ser (por ejemplo) un usuario que interactua con la aplicación. O un framework encargado de instanciar, inicializar los beans, dependencias, etc.

- A diferencia del flujo de un programa tradicional (linea por linea).

Ejemplo flujo tradicional:

	public class Generals {
		public static void main(String[] args) {
			List<Integer> lista = Arrays.asList(1, 2, 3);
			int number = 5;
			for (int value : lista) {
				//
			}
			System.out.print(number);
		}
	}

### Ventajas de la Inversión de Control

- Desacoplamiento de la lógica en pequeñas clases u objetos a nivel de dependencias.

- Se oculta la implementación de las dependencias, teniendo un beneficio de segregación de interfaces.

- Facilita el testing por componentes o mocks de dependencias. 

- Mayor modularidad de un programa.

Ejemplo Inversión de Control:

	public interface Dependencia {
		void implementation();
	}	
	public class Objeto implements Dependencia {
		@Override
		public void implementation() {
			//
		}
	}
	public class Objeto2 implements Dependencia {
		@Override
		public void implementation() {
			//
		}
	}

### Inversión de Control en el contexto de Spring Boot

- Los objetos que son administrados por el contenedor Spring IoC se denominan Beans.

- Un Bean es un objeto/dependencia que es instanciado, ensamblado y administrado por un contenedor Spring IoC.

Ejemplo:

	@Bean
	public MyBean anyNameMethod() {
		return new MyBeanImpl();
	}

Los beans básicamente son módulos desarrollados en Spring encargados de brindar la lógica necesaria para nuestra aplicación. 

Por ejemplo, si necesitamos referenciar que nuestra clase es un modelo hacemos uso del bean @entity. Esto nos permite usar propiedades creadas para este tipo de modulo que nos agilizan nuestro desarrollo. 

Al hacer inversión de control y llamar a los beans se referencian los módulos funcionales creados por Spring, además Spring Boot facilita el fácil instanciamiento de estos a nuestra aplicación.

### Inyección de Dependencias

- Es el proceso con el que los objetos definen sus dependencias.

- Patrón que implementa el principio de inversión de control para utilizar las dependencias inicializadas con el contenedor Spring.

- Permite un código más limpio y modularizado, además del desacoplamiento más efectivo cuando cada objeto cuenta con su dependencia. Es decir, serán clases más fáciles de probar, en particular cuando son interfaces.

Ejemplo:

	@Bean
	public MyBean myBean() {
		return new MyBeanImpl();
	}

	public class ClaseNecesitoDependencia() {
		
		private final MyBean dependencie;

		@AutoWired
		public ClaseNecesitoDependencia(MyBean dependencie){ 		// Constructor que inicializa la dependencia
			this.dependencie = dependencie;
		}

		//
	}

NOTA: Un Bean es una única instancia que se crea al inicio de la ejecución del programa y puede ser usada en otras clases llamándola con @Autowired (aquí le decimos “dependencia”), también se puede llamar en el constructor de la clase y el mismo framework hace la “magia” de ir por ella sin necesidad de nosotros pasarle la instancia en el constructor cuando usemos la clase.

Hay 3 formas de aplicar la inyeccion de dependencias:

- Por medio de atributo.
- Por medio de metodos sets.
- El mas famoso es por medio de constructor.

-------------------------------------------------------------------------------------------------

## Autoconfiguration y Runtime

- Spring Boot configura automáticamente tus aplicaciones basadas en dependencias JAR que agregaste mediante el pom.xml.

- Sin embargo, la autoconfiguración no es invasiva, siempre que queramos podemos configurar nuestros propios beans, es decir, si nosotros realizamos una configuración manual esta es priorizada por Spring Boot.

Por ejemplo:

	- Agregamos una base de datos H2 dentro de nuestro gestor de dependencias.
	- Spring Boot autoconfigura todo lo necesario para la inicialización de esta dependencia.

Sin embargo, se puede agregar la configuración manual, por ejemplo:

	@Bean
	public DataSource dataSource() {
		DataSourceBuilder dataSourceBuilder = DataSourceBuilder.create();
		dataSourceBuilder.driverClassName("org.h2.Driver");
		dataSourceBuilder.url("jdbc:h2:mem:test");
		dataSourceBuilder.username("SA");
		dataSourceBuilder.password("");
		return dataSourceBuilder.build();
	}

-------------------------------------------------------------------------------------------------

## Anotaciones para indicar Dependencias en Spring Boot

Una Anotación es una forma de añadir metadatos al código fuente Java que están disponibles para la aplicación en tiempo de ejecución o de compilación. Es una alternativa mas sencilla al uso de XML.

### TIPOS DE ANOTACIONES

1. @Component: 

	Es la notación genérica y se usa cuando tenemos una clase abstracta. De desta nacen las siguientes anotaciones.

2. @Controller: 

	Para indicar que esta será la clase que gestionara las peticiones del usuario por get, post, put, patch o delete.

	La usamos en las clases que se encargarán tanto de recibir las peticiones HTTP por parte de la parte frontal como devolver las respuestas de esas peticiones procesadas al frontal.

3. @Service: 

	Con esta notación especificamos que en esta clase se encontrara toda nuestra lógica de negocio, cálculos o llamadas a otras API externas. Es decir, validaciones.

4. @Repository: 
	
	Se usa para las clases o interfaces que funcionaran con el acceso a la base de datos, donde nos ocuparemos de la obtención y persistencia de datos.

Ejemplo de inyección de dependencia:

	@Component
	public class Component {
		public void myMethod() {
			// Implementación más general
		}
	}
	@Controller
	public class Controller {
		public void myMethod() {
			// Respuesta http, implementación para vista o GUI
		}
	}
	@Service
	public class Service {
		public void myMethod() {
			// Implementación de Lógica de negocio
		}
	}
	@Repository
	public class Repository {
		public void myMethod() {
			// Implementación de persistencia de datos
		}
	}

Si nuestra clase o interfaz no tiene una especificación clara como @Service, @Repository o @Controller, simplemente recurrimos a @Component y le indicamos que sencillamente es un componente.

Por otro lado, no es estrictamente necesario que cumplas con colocar una notación especifica, pero es una buena practica.

Ejemplo:

	@Service
	public class DatabaseAccountService implements AccountService {

		private final RiskAssessor riskAssessor;

		@Autowired
		public DatabaseAccountService(RiskAssesor riskAssessor) {
			this.riskAssesor = riskAssessor;
		}

		// ...

		public void myServiceFunction() {
			riskAssessor.myDependencyFunction();
		}
	}

IMPORTANTE: Al aplicarle una anotación a una clase, estamos indicando que dicha clase será una dependencia de otro objeto o clase en el futuro.

### RESUMEN (IMPORTANTE)

	1. Se hace petición HTTP desde la parte frontal a nuestra aplicación.
	2. @Controller correspondiente recibe esa petición y llama al @Service.
	3. @Service realiza las validaciones correspondientes y además, llame al @Repository si fuera necesario.
	4. @Repository realiza la persistencia o recuperación de datos.
	5. @Service devuelve respuesta al @Controller.
	6. @Controller devuelve respuesta al frontal.

-------------------------------------------------------------------------------------------------

##

-------------------------------------------------------------------------------------------------
