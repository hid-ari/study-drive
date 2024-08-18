# Desarrollo de API Rest Java

### Proyecto

Desarrollo de "clínica médica"

### Objetivos

- Creación de una API Rest
- CRUD (Create, Read, Update, Delete)
- Validaciones
- Paginación y orden

### Tecnologias

- Spring Boot 3
- Java 17
- Lombok
- MySQL/Flyway
- JPA/Hibernate
- Maven
- Insomnia

## Spring initializr

Spring [initializr](https://start.spring.io/)

> Artículo
[cambios principales en versiones](https://www.aluracursos.com/blog/caracteristica-destacables-java8-delante)
de java

### Spring y Spring Boot

Spring es un framework para desarrollar aplicaciones en Java, creado a mediados
de 2002 por *Rod Johnson*, que se ha vuelto muy popular y adoptado en todo el
mundo debido a su simplicidad y facilidad de integración con otras tecnologías.

Se desarrolló de forma modular, en el que cada recurso que proporciona está
representado por un módulo, que se puede agregar a una aplicación según sea
necesario. Con esto, en cada aplicación podemos agregar solo los módulos que
tengan sentido, haciéndola así más liviana. Hay varios módulos en Spring,
cada uno con un propósito diferente, tales como:

- módulo MVC, para desarrollar aplicaciones Web y API's Rest
- módulo de Security, para manejar el control de autenticación y autorización
de las aplicaciones
- módulo Transactions, para gestionar el control transaccional

Sin embargo, uno de los mayores problemas de las aplicaciones que usaban Spring
era la parte de configuración de sus módulos, que se hacía íntegramente con
archivos XML, y después de unos años el framework también comenzó a soportar
configuraciones a través de clases Java, utilizando principalmente anotaciones.
En ambos casos, dependiendo del tamaño y complejidad de la aplicación, así
como de la cantidad de módulos Spring utilizados en ella, dichas configuraciones
eran bastante extensas y difíciles de mantener.

Además, iniciar un nuevo proyecto con Spring era una tarea bastante complicada,
debido a la necesidad de realizar este tipo de configuraciones en el proyecto.

Precisamente para solventar tales dificultades, a mediados de 2014 se creó un
nuevo módulo Spring, denominado ***Boot***, con el objetivo de agilizar la
creación de un proyecto que utilice Spring como framework, así como simplificar
las configuraciones de sus módulos.

El lanzamiento de Spring Boot fue un hito para el desarrollo de aplicaciones
Java, ya que hizo más simple y ágil esta tarea, facilitando mucho la vida de
las personas que utilizan el lenguaje Java para desarrollar sus aplicaciones.

La versión 3 de Spring Boot se lanzó en noviembre de 2022 y algunas de sus
nuevas características principales son:

- Compatibilidad con Java 17
- Migración de especificaciones Java EE a Jakarta EE
- Compatibilidad con imágenes nativas
- Lista completa de las novedades de Spring Boot 3 en:
[Release Notes 3.0](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Release-Notes)

### Creación del proyecto

Configuración adicional en IntelliJ IDE para evitar reinicio manual

![img](study_drive/07_java_spring_boot/imgs/intelli_complr_set.png)
![img](study_drive/07_java_spring_boot/imgs/intelli_adv_set.png)

Estructura del proyecto vall_api

```txt
api
├── .idea/
├── .mvn/
├── src
│   ├── main
│   │   ├── java
│   │   │   └── med
│   │   │       └── voll
│   │   │           └── api
│   │   │               ├── controller
│   │   │               │   └── HelloController.java
│   │   │               └── ApiApplication.java
│   │   └── resources/
│   └── test/
├── target/
├── .gitignore
├── HELP.md
├── mvnw
├── mvnw.cmd
└── pom.xml
```


### Utilizando Insomnia

AppImage de Insomnia para probar API

Test post a `http://127.0.0.1:8080/medicos`, **json**

```json
{
    "nombre": "Rodrigo Lopez",
    "email": "rodrigo.lopez@voll.med",
    "documento": "123456",
    "especialidad": "ortopedia",
    "direccion": {
        "calle": "calle 1",
        "distrito": "distrito 1",
        "ciudad": "Lima",
        "numero": "1",
        "complemento": "a"
    }
}
```

#### JSON

**JSON** (JavaScript Object Notation) es un formato utilizado para representar
información, al igual que **XML** y **CSV**.

Una API necesita recibir y devolver información en algún formato que represente
los recursos que administra. **JSON** es uno de estos posibles formatos, popular
por su ligereza, sencillez, facil lectura (humana y máquina), así como por su
soporte para diferentes lenguajes de programación.

Representación de información en formato **XML**

```xml
<producto>
    <nombre>Mochila</nombre>
    <precio>89.90</precio>
    <descripcion>Mochila para notebooks de hasta 17 pulgadas</descripcion>
</producto>
```

Representación en formato **JSON**

```json
{
    "nombre" : "Mochila",
    "precio" : 89.90,
    "descripcion" : "Mochila para notebooks de hasta 17 pulgadas"
}
```

Observe cómo el formato JSON es mucho más compacto y legible. Precisamente por eso, se ha convertido en el formato universal utilizado en la comunicación de aplicaciones, especialmente en el caso de las API REST.

Más detalles sobre JSON
#### CORS

Al desarrollar una API y se busca que sus recursos estén disponibles para
cualquier ***cliente HTTP***.

**CORS** (Cross-Origin Resource Sharing) o “Intercambio de recursos con
diferentes orígenes”. Es común tener errores CORS al consumir y poner a
disposición una API.

![img](study_drive/07_java_spring_boot/imgs/spring_boot_cors_error.png)

**CORS** es un mecanismo utilizado para agregar encabezados HTTP que indica a
los navegadores permitir que una aplicación web se ejecute en un origen y acceda
a los recursos desde un origen diferente. Este tipo de acción se denomina
***cross-origin HTTP request***.
En la práctica, les informa a los navegadores si se puede acceder o no a un
recurso en particular.

Entendiendo los errores

***`Same-origin policy`***  
Por defecto, una aplicación Front-end, escrita en JavaScript, solo puede acceder
a los recursos ubicados en el mismo origen de la solicitud. Esto sucede debido
a la política del mismo origen (*same-origin policy*), que es un mecanismo de
seguridad de los navegadores que restringe la forma en que un documento o script
de un origen interactúa con los recursos de otro.
Esta política tiene como objetivo detener los ataques maliciosos.

Dos URL comparten el mismo origen si el **protocolo**, el **puerto** (si se
especifica) y el **host** son los mismos. Comparemos posibles variaciones
considerando la URL `https://cursos.alura.com.br/category/programacao`:

| URL | Resultado | Motivo |
| - | - | - |
| `https://cursos.alura.com.br/category/front-end` | Mismo origen | Solo camino diferente |
| `http://cursos.alura.com.br/category/programacao` |Error de CORS | Protocolo diferente (http) |
| `https://faculdade.alura.com.br:80/category/programacao` | Error de CORS | Host diferente |

#### ¿Como consumir una API con una URL diferente sin tener problemas CORS?

Por ejemplo, si se quiere consumir una API que se ejecuta en el puerto `8000` desde
una aplicación React corriendo en el puerto `3000`.

Al enviar una solicitud a una API de origen diferente, la API debe devolver un
header llamado `Access-Control-Allow-Origin`. Dentro de esta, es ella es necesario
informar los diferentes orígenes que serán permitidas de consumir la API, en
este caso: `Access-Control-Allow-Origin: http://localhost:3000`.

Para permitir el acceso desde cualquier origen se utiliza el símbolo `*`:
`Access-Control-Allow-Origin: *`. Esta es una medida **no recomendada**, ya que
permite que fuentes desconocidas accedan al servidor, a menos que sea intencional,
como en el caso de una API pública.

**¿Cómo hacer esto en Spring Boot correctamente?**

#### Habilitando diferentes orígenes en Spring Boot

Configurar el **CORS** para permitir que un origen específico consuma la API,
creando una clase de configuración como la sgte.

```java
@Configuration
public class CorsConfiguration implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://localhost:3000")
            .allowedMethods("GET", "POST", "PUT", "DELETE",
                    "OPTIONS", "HEAD", "TRACE", "CONNECT");
    }
}
```

`http://localhost:3000` sería la dirección de la aplicación Front-end y
`.allowedMethods` los métodos que se permitirán ejecutar. Con esto se podrá
consumir la API sin problemas desde una aplicación front-end.

#### Restricciones o validaciones

|| Médico ||
| :- | :- | :- |
| Nombre | Solo letras | No puede llegar vacío |
| Especialidad | Ortopedia, Ginecología,<br>Cardiología, Pediatria | No puede llegar vacío |
| Documento | Solo números | No puede llegar vacío |
| Email | Formato de email | No puede llegar vacío |
| Teléfono | Solo números | No puede llegar vacío |

|| Dirección ||
| :- | :- | :- |
| Calle | Letras y números | No puede llegar vacío |
| Número | Solo números | No puede llegar vacío |
| Complemento | Letras y números | |
| Cuidad | Letras y números | No puede llegar vacío |

### Java Record

Lanzado oficialmente en Java 16, pero disponible experimentalmente desde Java 14.
**Record** es un recurso que ***permite representar una clase inmutable, que
contiene solo atributos, constructor y métodos de lectura***, de una manera muy
simple y ágil.

Este tipo de clase encaja perfectamente para representar **clases DTO**, ya que
su objetivo es únicamente representar datos que serán recibidos o devueltos por
la API, sin ningún tipo de comportamiento.

Para crear una clase DTO inmutable, sin la utilización de Record, era necesario
escribir mucho código. El sgte. es un ejemplo de una clase DTO que representa
un teléfono:

```java
public final class Telefono {

    private final String ddd;
    private final String numero;

    public Telefono(String ddd, String numero) {
        this.ddd = ddd;
        this.numero = numero;
    }

    @Override
    public int hashCode() {
        return Objects.hash(ddd, numero);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        } else if (!(obj instanceof Telefono)) {
            return false;
        } else {
            Telefono other = (Telefono) obj;
            return Objects.equals(ddd, other.ddd)
              && Objects.equals(numero, other.numero);
        }
    }

    public String getDdd() {
        return this.ddd;
    }

    public String getNumero() {
        return this.numero;
    }
}
```

Con **Record** el código anterior se resume en una sola línea:

```java
public record Telefono(String ddd, String numero) {}
```

Internamente, Java transforma este registro en una clase inmutable, muy similar
al código que se muestra arriba.

Documentación Java
[record](https://docs.oracle.com/en/java/javase/17/language/records.html)


## Agregando dependencias

Copiar sección del `xml` de Spring [initializr](https://start.spring.io/), y
pegar en `pom.xml`

Modificar `resources/application.properties`

```ini
spring.datasource.url=jdbc:mysql://<URL>/vollmed-api
spring.datasource.username=<USER>
spring.datasource.password=<PASS>
```

Recomendaciones [12 Factor App](https://12factor.net/es/), que define las 12
mejores prácticas para crear aplicaciones modernas, escalables y de mantenimiento
sencillo.

En algunos proyectos Java, dependiendo de la tecnología elegida, es común
encontrar clases que siguen el patrón DAO, usado para aislar el acceso a los
datos. Sin embargo, en este curso usaremos otro patrón, conocido como
***Repositorio***.

***¿cuál es la diferencia entre los dos enfoques y por qué esta elección?***

#### Patrón DAO

El patrón de diseño DAO, también conocido como **Data Access Object**, se
utiliza para la persistencia de datos, donde su objetivo principal es separar
las reglas de negocio de las reglas de acceso a la base de datos. En las clases
que siguen este patrón, aislamos todos los códigos que se ocupan de conexiones,
comandos SQL y funciones directas a la base de datos, para que dichos códigos
no se esparzan a otras partes de la aplicación, algo que puede dificultar el
mantenimiento del código y también el intercambio de tecnologías y del mecanismo
de persistencia.

Implementación  
Supongamos que tenemos una tabla de productos en nuestra base de datos. La
implementación del ***patrón DAO*** sería la sgte:

Primero, será necesario crear una clase básica de dominio Producto:

```java
public class Producto {
    private Long id;
    private String nombre;
    private BigDecimal precio;
    private String descripcion;

    // constructores, getters y setters
}
```

A continuación, necesitaríamos crear la clase ProductoDao, que proporciona
operaciones de persistencia para la clase de dominio Producto:

```java
public class ProductoDao {

    private final EntityManager entityManager;

    public ProductoDao(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public void create(Producto producto) {
        entityManager.persist(producto);
    }

    public Producto read(Long id) {
        return entityManager.find(Producto.class, id);
    }

    public void update(Producto producto) {
        entityManger.merge(producto);
    }

    public void remove(Producto producto) {
        entityManger.remove(producto);
   }

}
```

En el ejemplo anterior, se utilizó JPA como tecnología de persistencia de datos
de la aplicación.

**Patrón Repository** según el famoso libro *Domain-Driven Design* de *Eric
Evans*:

> *El repositorio es un mecanismo para encapsular el almacenamiento, recuperación y
comportamiento de búsqueda, que emula una colección de objetos.*

En pocas palabras, un repositorio también maneja datos y oculta consultas
similares a DAO. Sin embargo, se encuentra en un nivel más alto, más cerca de
la lógica de negocio de una aplicación. Un repositorio está vinculado a la
regla de negocio de la aplicación y está asociado con el agregado de sus objetos
de negocio, devolviéndolos cuando es necesario.

Pero debemos estar atentos, porque al igual que en el patrón DAO, las reglas de
negocio que están involucradas con el procesamiento de información no deben estar
presentes en los repositorios. Los repositorios no deben tener la responsabilidad
de tomar decisiones, aplicar algoritmos de transformación de datos o brindar
servicios directamente a otras capas o módulos de la aplicación. Mapear entidades
de dominio y proporcionar funcionalidades de aplicación son responsabilidades muy
diferentes.

Un repositorio se encuentra entre las reglas de negocio y la capa de persistencia:

1. Proporciona una interfaz para las reglas comerciales donde se accede a los
objetos como una colección.
2. Utiliza la capa de persistencia para escribir y recuperar datos necesarios
para persistir y recuperar objetos de negocio.

Por lo tanto, incluso es posible utilizar uno o más DAOs en un repositorio.

***¿Por qué el patrón repositorio en lugar de DAO usando Spring?***

El patrón de repositorio fomenta un diseño orientado al dominio, lo que
proporciona una comprensión más sencilla del dominio y la estructura de datos.
Además, al usar el repositorio de Spring, no tenemos que preocuparnos por usar
la API de JPA directamente, simplemente creando los métodos, que Spring crea la
implementación en tiempo de ejecución, lo que hace que el código sea mucho más
simple, pequeño y legible.

### Validaciones

Bean Validation se compone de varias ***anotaciones*** que se deben agregar a los
atributos en los que queremos realizar las validaciones. Se implementaron algunas
de estas anotaciones, como **`@NotBlank`**, que indica que un atributo String no
puede ser nulo o vacío. (DatosRegistroMedico.java)

Sin embargo, existen decenas de otras anotaciones que se pueden utilizar, para
los más diversos tipos de atributos.

Se puede consultar una lista de las principales anotaciones de **Bean Validation**
en la
[documentación oficial](https://jakarta.ee/specifications/bean-validation/3.0/jakarta-bean-validation-spec-3.0.html#builtinconstraints).

### Migraciones

Estas se crean en el directorio
/src/db/migration/. Es
importante detener siempre el proyecto al crear los archivos de migración, para
evitar que **Flyway** los ejecute antes de tiempo, con el código aún incompleto,
lo que podría causar problemas.

En caso de:

```sql
# Soft
DELETE FROM flyway_schema_history WHERE success = 0;

# Hard
DROP TABLE flyway_schema_history;
```

### Lombok

Es una biblioteca de Java especialmente enfocada en la reducción de código y
en la productividad en el desarrollo de proyectos.

Utiliza la idea de ***anotaciones***  para generar códigos en el tiempo de
compilación. Pero el código generado no es visible, y tampoco es posible cambiar
lo que se ha generado.

Puede ser una buena herramienta aliada a la hora de escribir clases complejas,
siempre que el desarrollador tenga conocimiento sobre ella. Para más información
ver la documentación de [Lombok](https://projectlombok.org/)

### Anotacion Autowired

En el contexto del framework Spring, que utiliza como una de sus bases el patrón
de diseño *"Inyección de Dependencias"*, la idea sirve para definir una
inyección automática en un determinado componente del proyecto Spring, ese
componente puede ser atributos, métodos e incluso constructores.

Esta anotación se permite por el uso de la anotación `@SpringBootApplication`,
en el archivo de configuración de Spring, disponible cada vez que se crea un
proyecto Spring.

Al marcar un componente con la anotación `@Autowired` le estamos diciendo a
Spring que el componente es un punto donde se debe inyectar una dependencia,
en otras palabras, el componente se inyecta en la clase que lo posee,
estableciendo una colaboración entre componentes.

Para más información sobre la anotación, ver la
[documentación oficial](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

## Get

### Consideraciones

Información requerida del médico

- Nombre
- Especialidad
- Documento
- Email

Reglas del negocio

- Orden ascendente
- Paginado, máximo 10 registros por página

<br>

En MedicoController
se utilizo DTO para representar los datos recibi4os y devueltos a través de la
API, *¿Por qué, en lugar de crear un DTO, no devolvemos directamente la entidad
JPA en el Controller?*. Para esto, basta con cambiar método `listadoMedicos()`
en el Controller a:

```java
@GetMapping
public List<Medico> listarMedicos() {
    return repository.findAll();
}
```

De esa forma, el código sería más ligero y no necesitaríamos crear el DTO
en el proyecto. Pero, **¿es esto realmente una buena idea?**

#### Problemas de recepción/devolución de la entidad JPA

De hecho, es mucho más simple y cómodo no usar DTO, sino tratar directamente
con entidades JPA en los Controllers. Sin embargo, este enfoque tiene algunas
desventajas, incluida la vulnerabilidad de la aplicación a los ataques de
*Mass Assignment*.

Uno de los problemas, es que al devolver una entidad JPA en un método del
Controller, Spring generará el JSON que contiene todos sus atributos, y este no
siempre es el comportamiento deseado.

Eventualmente pueden tener atributos que no se requiere que sean devueltos en el
JSON, ya sea por razones de seguridad, en el caso de datos sensibles, o incluso
porque no son utilizados por clientes API.

#### Uso de la anotación `@JsonIgnore`

En esta situación, se puede usar la anotación `@JsonIgnore`, que nos ayuda a
ignorar ciertas propiedades de una clase Java cuando se serializa en un objeto
JSON.

Su uso consiste en agregar la anotación a los atributos que se quieren ignorar
cuando se genera el JSON. Por ejemplo, supongamos que tenemos una entidad JPA
`Empleado`, en la que se quiere ignorar el atributo `salario`:

```java
@Getter
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
@Entity(name = "Empleado")
@Table(name = "empleados")
public class Empleado {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    private String email;

    @JsonIgnore
    private BigDecimal salario;
    ...
}
```

En el ejemplo anterior, el atributo `salario` de la clase `Empleado` no será
mostrado en las respuestas JSON y el problema estaría resuelto.

Sin embargo, puede haber algún otro endpoint de la API en el que necesitemos
enviar el salario de los empleados en el JSON, en cuyo caso se tendrían problemas,
ya que con la anotación `@JsonIgnore` tal atributo nunca se enviará en el JSON,
y al eliminar la anotación se enviará el atributo siempre. Por lo tanto,
se pierde la flexibilidad de controlar cuando se deben enviar ciertos atributos
en el JSON y cuando no.
#### DTO

El patrón **DTO** (***Data Transfer Object***) es un patrón arquitectónico que
se usó ampliamente en aplicaciones Java distribuidas (arquitectura
cliente/servidor) para representar los datos que eran enviados y recibidos entre
aplicaciones cliente y servidor.

El patrón **DTO** puede (y debe) usarse cuando no queremos exponer todos los
atributos de alguna entidad en nuestro proyecto, una situación similar a los
salarios de los empleados que discutimos anteriormente. Además, con la
flexibilidad y la opción de filtrar qué datos se transmiten, podemos ahorrar
tiempo de procesamiento.

#### Bucle infinito que causa StackOverflowError

Otro problema muy recurrente cuando se trabaja directamente con entidades JPA
ocurre cuando una entidad tiene alguna *auto-relación* o *relación
bidireccional*. Por ejemplo, considere las siguientes entidades JPA:

```java
@Getter
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
@Entity(name = "Producto")
@Table(name = "productos")
public class Producto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    private String descripcion;
    private BigDecimal precio;

    @ManyToOne
    @JoinColumn(name = “id_categoria”)
    private Categoria categoria;

    ...
}
```

```java
@Getter
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
@Entity(name = "Categoria")
@Table(name = "categorias")
public class Categoria {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany(mappedBy = “categoria”)
    private List<Producto> productos = new ArrayList<>();

    ...
}
```

Al devolver un objeto de tipo `Producto` en el Controller, Spring tendría
problemas para generar el JSON de este objeto, lo que provocaría una excepción
de tipo `StackOverflowError`. Este problema ocurre porque el objeto producto
tiene un atributo de tipo `Categoria`, que a su vez tiene un atributo de tipo
`Lista<Producto>` lo que provoca un bucle infinito en el proceso de serialización
a JSON.

Este problema se puede resolver usando la anotación `@JsonIgnore` o usando las
anotaciones `@JsonBackReference` y `@JsonManagedReference`, pero también se puede
evitar usando un DTO que represente solo los datos que se deben devolver en el
JSON.

### Paginación y Orden

Para la paginación se utiliza la interfase `Pageable`

```java
    ...
    @GetMapping
    public Page<DatosListadoMedicos> listadoMedicos(
                    @PageableDefault(size = 5) Pageable paginacion
                    ) {
        return medicoRepository.findAll(paginacion).map(DatosListadoMedicos::new);
    }
}
```

Estos pueden pre-establecerse con la anotación `@PageableDefault`, o en el archivo
application.properties

```ini
spring.data.web.pageable.page-parameter=0
spring.data.web.pageable.size-parameter=5
spring.data.web.sort.sort-parameter=nombre
```

Estos además pueden ser sobrescritos al hacer el request:

```http
http://127.0.0.1:8080/medicos?size=3&page=2&sort=documento,desc
```

#### Ver y formatear querys en el IDE

En archivo application.properties

```ini
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

### Put

Información permitida para actualización

- Nombre
- Documento
- Direccion

### Mass Assignment Attack

Ataque de asignación masiva, ocurre cuando un usuario logra inicializar o
reemplazar parámetros que no deben ser modificados en la aplicación.
Al incluir parámetros adicionales en una solicitud, si dichos parámetros son
válidos, un usuario malintencionado puede generar un efecto secundario no deseado
en la aplicación.

El concepto de este ataque se refiere a cuando se inyecta un conjunto de valores
directamente en un objeto, de ahí la asignación masiva de nombres, que, sin la
debida validación puede causar serios problemas.

Ejemplo práctico. Se tiene el siguiente método, en una clase Controller,
utilizado para registrar un usuario en la aplicación:

```java
@PostMapping
@Transactional
public void registrar(@RequestBody @Valid Usuario usuario) {
    repository.save(usuario);
}
```

Y la entidad JPA que representa al usuario:

```java
@Getter
@Setter
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
@Entity(name = "Usuario")
@Table(name = "usuarios")
public class Usuario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    private String email;
    private Boolean admin = false;

    ...
}
```

Notar que el atributo admin de la clase `Usuario` se inicializa como falso, lo
que indica que un usuario siempre debe estar registrado como administrador. Sin
embargo, si se envía el siguiente JSON en la solicitud:

```json
{
    "nombre" : "Rodrigo",
    "email" : "rodrigo@email.com",
    "admin" : true
}
```

El usuario se registrará con el atributo `admin` con valor `true`. Esto sucede
porque el atributo `admin` enviado en el JSON existe en la clase que se está
recibiendo en el Controller, considerándose un atributo válido y que se llenará
en el objeto `Usuario` que será instanciado por Spring.

**¿Cómo prevenir este problema?**

El uso del patrón **DTO** ayuda a evitar este problema, ya que al crear un DTO
se definen solo los campos que se pueden recibir en la API, y en el ejemplo
anterior el DTO no tendría el atributo admin.

Esta es una ventaja más de usar el patrón DTO para representar los datos que
entran y salen de la API.
### PATCH

Elegir entre el método HTTP **PUT** o **PATCH** es una pregunta común que surge
al desarrollar APIs y se necesita crear un endpoint para la actualización de
recursos.
#### Diferencias entre las dos opciones y cuando usar cada una

- **PUT**  
Reemplaza todos los datos actuales de un recurso con los datos enviados en la
solicitud, es decir, una actualización completa de un recurso en una sola
solicitud.

- **PATCH**  
Aplica modificaciones parciales a un recurso. Por lo tanto, es posible modificar
solo una parte de un recurso, lo que flexibiliza las opciones de actualización.

**¿Cuál elegir?**

En la práctica, es difícil saber qué método usar, porque no siempre se sabrá
si un recurso se actualizará parcial o completamente en una solicitud, a menos
que lo verifiquemos, algo que no se recomienda.

Entonces, lo más común en las aplicaciones es usar el método PUT para las
solicitudes de actualización de recursos en una API, que es la elección para
el proyecto ***voll med api***.
### Delete

Exclusión de Médicos

- El registro no debe ser borrado de la base de datos
- El listada debe retornar solo Médicos activos

Mapeo de solicitude PUT con la anotación `@PutMapping`

```java
    @PutMapping
    @Transactional
    public void actualizarMedico(@RequestBody @Valid DatosActualizarMedico datosActualizarMedico) {
        Medico medico = medicoRepository.getReferenceById(datosActualizarMedico.id());
        medico.actualizarDatos(datosActualizarMedico);
    }
```

Mapeo de solicitud **DELETE** con la anotación `@DeleteMapping`.
Y mapeo de parámetros dinámicos en la URL con la anotación `@PathVariable`.

```java
    @DeleteMapping("/{id}")
    @Transactional
    public void eliminarMedico(@PathVariable Long id) {
        Medico medico = medicoRepository.getReferenceById(id);
        medico.desactivarMedico();
    }
```

```java
package med.voll.api.medico;

import jakarta.validation.constraints.NotNull;
import med.voll.api.direccion.DatosDireccion;

public record DatosActualizarMedico(
            @NotNull Long id,
            String nombre,
            String documento,
            DatosDireccion direccion
) {}
```

----
## Requests a la API

Prueba inicial método **GET** a URL `http://127.0.0.1:8080/hello` según mapeo en
HelloController
### POST

<details>

<summary markdown="span">Registrar médicos con método <b>POST</b> a URL
<code>http://127.0.0.1:8080/medicos</code></summary>

```json
{
    "nombre": "Primer Medico",
    "email": "medico1@voll.med",
    "telefono": "1111111",
    "documento": "111111",
    "especialidad": "PEDIATRIA",
    "direccion": {
        "calle": "calle 1",
        "distrito": "distrito 1",
        "complemento": "uno",
        "ciudad": "Santiago",
        "numero": "1"
    }
}
```

```json
{
    "nombre": "Segundo Medico",
    "email": "medico2@voll.med",
    "telefono": "222222",
    "documento": "222222",
    "especialidad": "ORTOPEDIA",
    "direccion": {
        "calle": "calle 2",
        "distrito": "distrito 2",
        "complemento": "dos",
        "ciudad": "Santiago",
        "numero": "2"
    }
}
```

```json
{
    "nombre": "Tercer Medico",
    "email": "dr_3@voll.med",
    "telefono": "33333",
    "documento": "333333",
    "especialidad": "GINECOLOGIA",
    "direccion": {
        "calle": "calle 3",
        "distrito": "distrito 3",
        "ciudad": "Santiago",
        "numero": "3"
    }
}
```

</details>



<details>

<summary markdown="span">Registrar pacientes con método <b>POST</b> a URL
<code>http://127.0.0.1:8080/pacientes</code></summary>

```json
{
    "nombre": "Primer Paciente",
    "email": "paciente1@private.cl",
    "telefono": "1111111",
    "documento": "111111",
    "direccion": {
        "calle": "calle 1",
        "distrito": "distrito 1",
        "complemento": "uno",
        "ciudad": "Santiago",
        "numero": "1"
    }
}
```

```json
{
    "nombre": "Segundo Paciente",
    "email": "paciente2@private.cl",
    "telefono": "222222",
    "documento": "222222",
    "direccion": {
        "calle": "calle 2",
        "distrito": "distrito 2",
        "complemento": "dos",
        "ciudad": "Santiago",
        "numero": "2"
    }
}
```

```json
{
    "nombre": "Tercer Paciente",
    "email": "paciente3@private.cl",
    "telefono": "33333",
    "documento": "333333",
    "direccion": {
        "calle": "calle 3",
        "distrito": "distrito 3",
        "ciudad": "Santiago",
        "numero": "3"
    }
}
```

</details>

### GET

Listar medicos con método **GET** a URL
`http://127.0.0.1:8080/medicos?size=20&sort=id`

Listar pacientres con método **GET** a URL
`http://127.0.0.1:8080/pacientes?sort=id,desc`

### PUT

<details>

<summary markdown="span">Modificar médico con método <b>PUT</b> a URL
<code>http://127.0.0.1:8080/medicos</code></summary>

```json
{
    "id": 3,
    "nombre": "3er Medico"
}
```

</details>

<details>

<summary markdown="span">Modificar paciente con método <b>PUT</b> a URL
<code>http://127.0.0.1:8080/paciente</code></summary>

```json
{
    "id": 3,
    "nombre": "3er Paciente"
}
```

</details>
### DELETE

Borrar médico con método **DELETE** a URL `http://127.0.0.1:8080/medicos/3`

Borrar paciente con método **DELETE** a URL `http://127.0.0.1:8080/pacientes/3`

---

Continua en [Buenas prácticas y protección de una API Rest](study_drive/07_java_spring_boot/spring_boot_2.md)
