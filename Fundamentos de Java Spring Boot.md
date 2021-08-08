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
	- Uso de properties y generación de POJO
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

### Inversión de Control en el contexto de Spring Boot (BEANS)

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

Los Beans en Java son clases que cumplen con ciertas normas respecto al nombre de sus propiedades y métodos. De primera instancia un bean debe tener un constructor sin argumentos, además de tener declarados todos sus atributos como privados y para cada uno ellos un método getter y setter los cuáles sirven para obtener y agregar valores a estos atributos.

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

### @AutoWired

Lo que hace un @AutoWired es buscar un objeto manejado (beans) que implementen determinada interfaz para hacer referencia a él. DE esta manera no es neceario crear una instancia nueva del objeto cada vez que se necesite la funcionalidad de determinada clase 

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

## Creación de proyecto bajo arquitectura de dependencias

Vamos a crear un proyecto, nos dirijimos a "Spring Initializr" en Google, ya que este es el inicializador por defecto para crear un proyecto de Spring Boot.

- Nos dirijimos a Spring Initializr en: https://start.spring.io/
- Gestor de Dependencias: Maven Project
- Language: Java
- Versión de Spring Boot: La más estable en el momento (2.5.3)
- Metadatos del proyecto:
	- Group: com.fundamentosplatzi.springboot
	- Artifact: fundamentos
- Java: 11
- Generamos el proyecto y se descarga.

- Abrimos el proyecto con IntelliJ.
- Creamos nuestro repositorio de Git.
- Creamos los paquetes en la carpeta main de "component", "controller", "service" y "repository".

-------------------------------------------------------------------------------------------------

## Inyección de Dependencia "Component"

Creamos en la carpeta component:
	- ComponentDependency
	- ComponentImplement
	- ComponentTwoImplement

En ComponentDependency:

	public interface ComponentDependency {
	    void saludar();
	}

En ComponentImplement:

	@Component
	public class ComponentImplement implements ComponentDependency {
	    @Override
	    public void saludar() {
	        System.out.println("Hola mundo desde mi componente");
	    }
	}

En ComponentTwoImplement:

	@Component
	public class ComponentTwoImplement implements ComponentDependency {
	    @Override
	    public void saludar() {
	        System.out.println("Hola mundo desde mi componente dos");
	    }
	}

En FundamentosApplication:

	@SpringBootApplication
	public class FundamentosApplication implements CommandLineRunner {

		private ComponentDependency componentDependency;

		public FundamentosApplication(@Qualifier("componentTwoImplement") ComponentDependency componentDependency) {
			this.componentDependency = componentDependency;
		}

		public static void main(String[] args) {

			SpringApplication.run(FundamentosApplication.class, args);

		}

		@Override
		public void run(String... args) {
			componentDependency.saludar();
		}
	}

Con Qualifier defino cual clase o implementación quiero aplicar.

-------------------------------------------------------------------------------------------------

## Ejemplo de creación de dependencia propia

- Creamos una carpeta "bean", en ella una interface/dependencia "MyBean" y una clase que la implemente llamada "MyBeanImplement".

- Creamos una carpeta "configuration" desde la cual vamos a configurar todos los beans y dependencias propias.

- Creamos "MyConfigurationBean".......

.... VEASE LA CARPETA DE FUNDAMENTOS!

Notamos que, en el ejemplo anterior utilizamos Qualifier para decidir cual implementación utilizar, sin embargo, utilizando el archivo que creamos "MyConfigurationBean", podemos decidir cual implementación devolver desde dicho archivo, lo cual es mucho mejor a nivel de arquitectura.

También hicimos una inyección de dependencia, DENTRO de otra dependencia.

-------------------------------------------------------------------------------------------------

## Cambio de Puerto y Path

Nos ubicamos en el archivo "pom.xml" y agregamos una nueva dependencia en "dependencies":

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>

Nos ubicamos en el archivo "application.properties" para añadir la siguiente configuración:

	server.port=8081
	server.servlet.context-path=/app

Nos ubicamos en Controller y creamos una clase "TestController", para verificar que el puerto y el path cambiado anteriormente fue hecho correctamente.

	@Controller
	public class TestController {
	    @RequestMapping     // Para aceptar todas las solicitudes dentro del método a nivel HTTP
	    @ResponseBody       // Responder un cuerpo a nivel de servicio
	    public ResponseEntity<String> function() {
	        return new ResponseEntity<>("Hello from controller", HttpStatus.OK);
	    }
	}

Ejecutamos y nos dirigimos al navegador en el puerto asignado: 

	http://localhost:8081/app/

Recordar que se desplega por el servidor embebido Tomcat por defecto. Puede configurarse si es necesario.

Ahora vamos a agregar una nueva dependencia (SIMILAR AL NODEMON):

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
		<optional>true</optional>
	</dependency>

Esta ultima dependencia redesplega la pagina con los cambios sin necesidad de reactivar el servidor de nuevo. Esto se hace haciendo Build.

-------------------------------------------------------------------------------------------------

## Uso de properties y generación de POJO

En el archivo "application.properties", añadimos las propiedades:

	value.name=sergio
	value.apellido=nava
	value.random=${random.value}

Estas propiedades pueden ser utilizadas a lo largo del proyecto.

En la carpeta "configuration" creamos la clase "GeneralConfiguration":

	@Configuration
	public class GeneralConfiguration {
	    @Value("${value.name}")
	    private String name;

	    @Value("${value.apellido}")
	    private String apellido;

	    @Value("${value.random}")
	    private String random;

	    @Bean
	    public MyBeanWithProperties function() {
	        return new MyBeanWithPropertiesImplement(name, apellido);
	    }
	}

- POJO (Plain Old Java Object):

	Es una instancia de una clase que no extiende ni implementa nada en especial. Sirve para enfatizar el uso de clases simples y que no dependen de un framework en especial. Por ejemplo, un Controller de Spring tiene que extender de SimpleFormController, e implementar los métodos abstractos de ese tipo, por eso no es un POJO. En cambio, si defines una clase Cliente con atributos y unas cuantas operaciones, tienes un simple y modesto POJO.

-------------------------------------------------------------------------------------------------

## Uso de properties con ejemplo de generación de POJO

En properties:

	user.email=test@mail.com
	user.password=1234
	user.age=25

Creamos una carpeta "pojo" con una clase "UserPojo":
	
	@ConstructorBinding							// Para inyectar como dependencia a partir de las propiedades
	@ConfigurationProperties(prefix = "user") 	// Prefijo de la propiedad "user"
	public class UserPojo {	
	    private String email;
	    private String password;
	    private int age;

	    public UserPojo(String email, String password, int age) {
	        this.email = email;
	        this.password = password;
	        this.age = age;
	    }

	    // getter y setter

	}

Notamos el prefijo en la anotación que hace referencia a la propiedad "user".

Para Spring Boot, la clase UserPojo aún no se ha configurado como dependencia, entonces debemos realizar esa configuración. Vamos al "GeneralConfiguration" y añadimos la anotación:

	@EnableConfigurationProperties(UserPojo.class)
	public class GeneralConfiguration { ... }

De esta manera, definimos que la clase "UserPojo" es la que se representará como propiedades de la aplicación.

-------------------------------------------------------------------------------------------------

## Qué son los logs y cómo usarlos

Los Logs son la herramienta que nos permite debuggear la información. 

Utilizaremos la librería Apache Commons, con los niveles de logs:

	- Error: Muestrea información cuando ocurre un error.
	- Info: Muestrea información general.
	- Debug: Para depurar o debuggear a nivel de código fuente.
	- Otros

Vamos al "application.properties" para programar los niveles de Logs que queremos mostrar en el servidor.

	logging.level.root=info
	logging.level.org.springframework.web=info
	logging.level.org.hibernate=error

Creamos entonces un LOGGER en "FundamentosApplication" y usamos LOGGER.error() para generar un error.

	private final Log LOGGER = LogFactory.getLog(FundamentosApplication.class);
	...
	try {
		int value = 10/0;
		LOGGER.debug("Mi valor: " + value);		// No se imprime ya que se catchea el error primero.
	} catch (Exception e) {
		LOGGER.error("Esto es un error al dividir por cero 0 " + e.getMessage());
	}

Podemos ir a una dependencia y hacer algo similar con "info" y "debug":

	Log LOGGER = LogFactory.getLog(MyBeanWithDependencyImplement.class);
	...
	LOGGER.info("Hemos ingresado al método printWithDependency");
    int numero=1;
    LOGGER.debug("El numero enviado como parámetro a la dependencia operation es: " + numero);
    ...

Y cambiamos el nivel de logs en properties, a "debug", para así mostrar el debug e info anterior.

-------------------------------------------------------------------------------------------------

## Modelado de entidades con JPA

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