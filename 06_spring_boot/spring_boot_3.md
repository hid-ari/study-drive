# API Rest Java - Documentar, Probar y Preparar una API para su Implementación

Continuación de
[Buenas Prácticas y Protección de una API Rest](spring_boot_2.md) donde se
vio:

- Creación de un API Rest
- Crud (Create, Read, Update, Delete)
- Validaciones
- Paginación y orden
- Buenas prácticas REST
- Tratamiento de errores
- Control de acceso con JWT

### Objetivos

- Funcionalidad agendar consultas
- Documentación de la API
- Tests Automatizados
- Build del proyecto
## Nuevas Funcionalidades

- Controller
- DTOs
- Entidades JPA
- Repository
- Migration
- Security
- Reglas de negocio

Proyecto Voll_Med API

Para implementar esta u otras funcionalidades, siempre es necesario crear los
siguientes tipos de códigos:

- **Controller**(s), para mapear la solicitud de la nueva funcionalidad
- **DTO**s, que representan los datos que llegan y salen de la API
- **Entidad**(es) **JPA**
- **Repository**(s), para aislar el acceso a la base de datos
- **Migration**, para hacer las alteraciones en la base de datos

Estos son los cinco tipos de código que **siempre** se desarrollan para una nueva
funcionalidad. Esto también se aplica al agendamiento de las consultas,
incluyendo un sexto elemento a la lista, ***las reglas de negocio***.
En este proyecto, se implementan las reglas de negocio con algoritmos más
complejos.

### Implementando la funcionalidad

Se desarrolla la funcionalidad por partes. Empezaremos por los primeros cinco
elementos de la lista, que son más básicos. Luego, la parte de reglas de negocio.

Se crea un nuevo `ConsultaController` en el paquete
`src.main.java.med.voll.api.controller`.

La idea es tener un **Controller** para recibir esas solicitudes relacionadas con
el agendado de consultas.

Es una clase Controller, con las ***anotaciones*** de Spring `@Controller`,
`@ResponseBody`, `@RequestMapping("consultas")` o `@RestController`. Mapea las
solicitudes que llegan con la **URI** `/consultas`, que debe llamar a este
controller y no a los otros.

Luego, el método anotado con `@PostMapping`. Entonces, la solicitud para programar
una consulta será del tipo **POST**, como en otras funcionalidades.

ConsultaController

```java
@Controller
@ResponseBody
@RequestMapping("/consultas")
public class ConsultaController {

    @Autowired
    private AgendaDeConsultaService service;

    @PostMapping
    @Transactional
    public ResponseEntity agendar(
                @RequestBody @Valid DatosAgendarConsulta datos) {
        System.out.println(datos);
        service.agendar(datos);
        return ResponseEntity.ok(new DatosDetalleConsulta(null, null, null, null));
    }
}
```

DatosAgendarConsulta
se trata de un registro similar a los que visto anteriormente. Tiene los campos
que provienen de la API (`Long idMedico`, `Long idPaciente` y
`LocalDateTime fecha`) y las anotaciones de **BEAN validation** `@NotNull` para el
`Id` del paciente y para la `fecha`, además de que la fecha debe ser en el
**futuro** (`@Future`), es decir, no se podrá programar una consulta en días
pasados.

```java
@Controller
@ResponseBody
@RequestMapping("/consultas")
public class ConsultaController {

    @Autowired
    private AgendaDeConsultaService service;

    @PostMapping
    @Transactional
    public ResponseEntity agendar(@RequestBody @Valid DatosAgendarConsulta datos) {
        System.out.println(datos);
        service.agendar(datos);
        return ResponseEntity.ok(new DatosDetalleConsulta(null, null, null, null));
    }
}
```

Al volver al Controller, el otro DTO es el de respuesta, llamado
`DatosDetalleConsulta`. Devuelve el `ID` de la consulta creada, del **médico**,
del **paciente** y la **fecha de la consulta** registrada en el sistema.

En el paquete `src.main.java.med.voll.api.domain`, se crea el subpaquete
`consulta`, que abarca las clases relacionadas con el dominio de consulta.

Entre ellas, la ***entidad JPA*** Consultaque contiene las anotaciones de ***JPA*** y ***Lombok***, así como la información de la consulta: `medico`, `paciente` y `fecha`.

```java
@Table(name = "consultas")
@Entity(name = "Consulta")
@Getter
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(of = "id")
public class Consulta {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "medico_id")
    private Medico medico;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "paciente_id")
    private Paciente paciente;

    private LocalDateTime fecha;

}
```

En este caso, `medico` y `paciente` son relaciones con las otras entidades
`Medico` y `Paciente`.

También se crea ConsultaRepository que está vacío por el momento.

```java
@Repository
public interface ConsultaRepository extends JpaRepository<Consulta, Long> {}
```

Por último, la migración número 6 (`V6`) en
`src.main.java.med.voll.api.resources.db.migration`, que crea la tabla de
consultas.

```sql
CREATE TABLE consultas(

    id BIGINT NOT NULL AUTO_INCREMENT,
    medico_id BIGINT NOT NULL,
    paciente_id BIGINT NOT NULL,
    fecha DATETIME NOT NULL,

    PRIMARY KEY(id),
    CONSTRAINT fk_consultas_medico_id FOREIGN KEY(medico_id)
    REFERENCES medicos(id),
    CONSTRAINT fk_consultas_paciente_id FOREIGN KEY(paciente_id)
    REFERENCES pacientes(id)

);
```

Los campos de `Id` de **consulta**, `Id` de **paciente**, `Id` de **médico** y
`fecha`, donde `medico_id` y `paciente_id` son ***claves foráneas*** que apuntan
a las tablas `medicos` y `pacientes`.

Estos son los códigos estándar para cualquier funcionalidad, con sus respectivos
cambios de acuerdo con el proyecto. Cada uno creará un controlador o una entidad
distinta, pero el funcionamiento es el mismo.

Ahora se puede intentar enviar una solicitud a la dirección `/consultas` y
verificar si se redirige al `ConsultaController` y comprobando el `System.out`
que muestra los datos que llegaron en el JSON de la solicitud.

```json
{
    "idPaciente": 1,
    "idMedico": 1,
    "fecha": "2023-09-14T10:00"
}
```

Esta es la ipmplementación del esqueleto de la funcionalidad. Ahora se deben
implementar las reglas de negocio.

El trabajo es un poco diferente a lo ya realizado con la validación de campos
de formulario vía ***BEAN validation***. Estas validaciones son más complejas.

***¿Cómo implementarlas?***

Observando `ConsultaController.java`, se podrían hacer todas las validaciones
en el método `agendar()`, antes del retorno. Sin embargo, esa no es una buena
práctica.

La clase controller no debe traer las reglas de negocio de la aplicación.
Es solo una clase que controla el flujo de ejecución: cuando llega una solicitud,
llama a la clase X, devuelve la respuesta Y. Si la condición es Z, devuelve otra
respuesta y así sucesivamente. Es decir, solo controla el flujo de ejecución y,
por lo tanto, no debería tener reglas de negocio.

Así, aislando las reglas de negocio, los algoritmos, los cálculos y las
validaciones en otra clase que será llamada por el Controller.

En el paquete `consulta`. Se creas la clase para contener las reglas de agendado
de consultas llamada AgendaDeConsultasService.

El nombre es muy autoexplicativo, esta clase contendrá la agenda de consultas.
Se pueden tener otras funcionalidades en esta clase, relacionadas con el
agendamiento de consultas.

Esta no es una clase ***Controller*** ni una clase de ***configuraciones***. Esta
clase representa un servicio de la aplicación, el de agendado de consultas. Por
lo tanto, será una ***clase de servicios*** (**Service**) y llevará la anotación
`@Service`. El objetivo de esta anotación es declarar el componente de servicio
a Spring Boot.

Dentro de esta clase, se crear un método `public void agendar()`, que recibe
como parámetro el **DTO** `DatosAgendarConsulta`.

```java
@Service
public class AgendaDeConsultaService {

    @Autowired
    private ConsultaRepository consultaRepository;
    @Autowired
    private MedicoRepository medicoRepository;
    @Autowired
    private PacienteRepository pacienteRepository;

    public void agendar(DatosAgendarConsulta datos) {

        if (pacienteRepository.findById(datos.idPaciente()).isPresent()) {
            throw  new ValidacionDeIntegridad("Id de paciente no encontrado");
        }
        if (datos.idMedico() != null && medicoRepository.existsById(datos.idMedico())) {
            throw  new ValidacionDeIntegridad("Id de médico no encontrado");
        }

        var paciente = pacienteRepository.findById(datos.idPaciente()).get();
        var medico = medicoRepository.findById(datos.idMedico()).get();
        var consulta = new Consulta(null, medico, paciente, datos.fecha());
        consultaRepository.save(consulta);
    }
}
```

La clase Service ejecuta las reglas de negocio y las validaciones de la aplicación.

Esta clase se utliza en `ConsultaController`, con `@Autowired` se comuníca a
Spring que instancie este objeto

Con esto, se inyecta la clase `AgendaDeConsultas` en el Controller. En el método
`agendar` del `Controller`, se obtiene el objeto `agenda` y se llama al método
`agendar()`, pasando como parámetro los datos que llegan al `Controller`.
Todo esto antes del retorno.

El **Controller** recibe la información, hace solo la validación de
***BIN validation*** y llama a la clase **Service** `AgendaDeConsultas`, que
ejecutará las reglas de negocio. Esta es la forma correcta de manejar las reglas
de negocio.

En la clase `AgendaDeConsultas` y están todas las validaciones para el método
`agendar()`.

El requerimiento especifica que se debe recibir la solicitud con los datos de
agendamiento y se deben guardar en la tabla de consultas.

Por lo tanto, se necesita acceder a la base de datos y a la tabla de consultas
en esta clase. Así que se declara un atributo `ConsultaRepository`, llamándolo
`consultaRepository`.

Se usa la anotación `@Autowired` para que el Spring Boot inyecte este repository
en la clase `Service`.

Al final del método `agendar()`, se inserta `consultaRepository.save()` pasandole
un objeto del tipo consulta, la entidad **JPA**. Obviamente, solo se puede llamar
a este método si todas las validaciones se han ejecutado de acuerdo con las
reglas de negocio.

La entidad `Consulta` está anotada con `@AllArgsConstructor` de ***Lombok***,
que genera un constructor con todos los atributos. Se puede usar este mismo
constructor en `AgendamientoDeConsultas`.
El primer parámetro es el `Id null`, ya que es la base de datos la que pasará
el `Id`. El segundo es `medico`, `paciente` y `fecha`. Esta última viene en el
**DTO**, a través del parámetro `datos`.

Sucede que el médico y el paciente no llegan en la solicitud, sino el `Id` del
médico y el `Id` del paciente. Por lo tanto, es necesario establecer el objeto
completo en la entidad y no solo el `Id`.

Por lo tanto, es necesario cargar médico y paciente desde la base de datos.
Se necesita inyectar, entonces, dos Repositories más en `Service`:
`MedicoRepository` y `PacienteRepository`.

En el método `agendar()`, se crea un objeto paciente también. Usando
`pacienteRepository.findById()` para buscar el objeto por `Id`, que está dentro
del DTO datos.

En la solicitud solo viene el `Id`, pero se necesita cargar el objeto completo.
Por lo tanto, se usa el Repository para cargar por el `Id` de la base de datos.
El médico seguirá la misma dinámica (
AgendaDeConsultasService
).

Aparecerá un error de compilación porque el método `findById()` no devuelve la
entidad, sino un Optional. Por lo tanto, al final de la línea, antes del punto
y coma, es necesario incluir `.get()` junto a `findById()`. Esto hace que tome
la entidad cargada.

El método `agendar()` en la clase `Service` obteniene el `Id`, cargar el paciente
y el médico desde la base de datos creando una entidad consulta pasando el médico,
el paciente y la fecha que viene en el DTO, y se guarda en la base de datos.

Pero antes de esto, se necesita escribir el código para realizar todas las
validaciones que forman parte de las reglas de negocio.

A continuación, se aborda cómo realizar las validaciones de la mejor manera
posible.

### Validaciones

Para verificar el ID del paciente, se usa un `if`. La idea es comprobar si el `Id`
del paciente existe. El Repository es el que consulta la base de datos.

Se puede lanzar una excepción dentro del `if`, que muestre un mensaje indicando
el problema. Incluso se puede crear una excepción personalizada para el proyecto
llamada `ValidacaoException()`. Con un mensaje como
`"El ID del paciente proporcionado no existe"` o similar (DatosDetalleConsulta)


En *package* `med.voll.api.infra.errores` se crea la clase
ValidacionDeIntegridad

```java
package med.voll.api.infra.errores;

public class ValidacionDeIntegridad extends RuntimeException {
    public ValidacionDeIntegridad(String s) {
        super(s);
    }
}
```

En la clase
AgendaDeConsultasService,
se realiza la verificación con. Primero, se verifica si existe un paciente con
el `Id` que está llegando a la base de datos. Si no existe, se lanzará una
excepción con un mensaje de error.

Se utiliza el método de la ***interfaz Repository*** en Spring Data llamado
`existsById`. Realiza una consulta a la base de datos para verificar si existe
un registro con un determinado `Id` que devuelve un booleano, `true` si existe,
`false` si no.
Pasando el parámetro `datos.idMedico()`. Se niega la expresión. Con esto, si no
hay un paciente con el `Id` en la base de datos, se debe detener la ejecución
del resto del código.

Recordando la última regla de negocio.

***La elección del médico es opcional, y en ese caso el sistema debe elegir
aleatoriamente algún médico disponible en la fecha/hora indicada.***

Por lo tanto, es posible que un `Id` de médico no llegue en la solicitud.
No se puede llamar a `existsById` si el `Id` del médico es nulo. Esto resultará
en un error para **JPA**.

Solo se puede llamar al `if` si el `Id` no es nulo. Por lo tanto, se agrega una
condición al if antes de la condición actual:

```java
if(datos.idMedico()!=null && !medicoRepository.existsById(datos.idMedico())){
   throw new ValidacionDeIntegridad("Id de medico no encontrado");
}
```

En el caso del médico, al ser un campo opcional, puede ser que la línea
`var medico = medicoRepository.findById(dados.idMedico()).get()` tenga un
`idMedico` nulo.

De acuerdo con la regla de negocio analizada, se necesita escribir un algoritmo
que elija aleatoriamente un médico en la base de datos. Por lo tanto, la línea
anterior necesita ser reemplazada. Pare ello se llama al método privado
`seleccionarMedico(datos)` que recibe un objeto `DatosAgendarConsulta` como
parametro y retorna un objeto `Medico`.

Esto sirve para aislar el algoritmo y evitar que esté suelto dentro del método
de programación de citas. En el método `seleccionarMedico()`, se necesita
verificar si está llegando el `Id` del médico en la solicitud o no.

Si lo está, se obtiene el médico correspondiente de la base de datos. Si no es
así, se debe elegir aleatoriamente un profesional de la salud, según lo indica
la regla de negocio.

Para implementar el algoritmo que elige al médico de manera aleatoria, se deben
cubrir todos los posibles escenarios. La primera comprobación es que si la
persona eligió un médico al hacer la solicitud, usando
`if (dados.idMedico() != null)`.

En este caso, se carga la información de la base de datos con
`return medicoRepository.getReferenceById(dados.idMedico())`. En lugar de usar
`findById()`, se podemos usar `getReferenceById()` y no es necesario llamar al
`.get()` usamdo anteriormente.

También se puedemo cambiar `findById()` por `getReferenceById()` en la variable
paciente, ya que no se quiere cargar el objeto para manipularlo, sino, solo para
asignarlo a otro objeto.

En el método `seleccionarMedico()` , lo primero es verificar si se realiza la
con un médico específico para su atención. Si es así, simplemente se carga la
información del médico que viene de la base de datos.

DatosAgendarConsulta

```java
public record DatosAgendarConsulta(
        @NotNull Long idPaciente,
        Long idMedico,
        @NotNull @Future LocalDateTime fecha,
        Especialidad especialidad) {
}
```

***¿Cómo elegir un médico aleatorio de la especialidad elegida, disponible en
la fecha y hora seleccionadas?***

Existen varias formas de hacer esto. Se podrían cargar los médicos, filtrarlos por
especialidad y comprobar la fecha de la consulta en Java.

Lo ideal es cargar un profesional aleatorio directamente de la base de datos.
Sin embargo, esta consulta es específica para nuestro proyecto, es decir, no está
lista en Spring Data JPA.

Se necesita crear un método para hacer esto:

```java
return medicoRepository.seleccionarMedicoConEspecialidadEnFecha(
                                datos.especialidad(),datos.fecha()
                            );
```

Creación del método `seleccionarMedicoConEspecialidadEnFecha(especialidad, fecha)`
en MedicoRepository.

Como el nombre del método está en español. No se estam siguiendo el estándar
de nomenclatura, como el `findAllByAtivoTrue()`. De esta manera, Spring Data no
podrá construir el SQL automáticamente. La idea es precisamente esa, para este método,
se escribe el comando SQL manualmente.

Para hacerlo, se usa la anotación `@Query()` justo encima del método. Que viene
del paquete `org.springframework.data.jpa`. Y entre paréntesis, se construye la
consulta utilizando la sintaxis del ***Java Persistence Query Language (JPQL)***.

```java
    @Query("""
            select m from Medico m
                where m.activo= true\s
                and
                m.especialidad=:especialidad\s
                and
                m.id not in( \s
                    select c.medico.id from Consulta c
                    where
                    c.fecha=:fecha
                )
                order by rand()
                limit 1
            """)
    Medico seleccionarMedicoConEspecialidadEnFecha(
                            Especialidad especialidad,
                            LocalDateTime fecha
                        );
}
```

### En resumen

Creación de nuevo *package*
`domain.consulta`
donde se crean entidad **Consulta**, clases `ConsultaRepository`,
`DatosAgendarConsulta` y `DatosDetalleConsulta`.

- Consulta
- ConsultaRepository
- DatosAgendarConsulta
  - DatosDetalleConsulta
  - AgendaDeConsultasService
- migración

---

### Anotación @JsonAlias

En caso que uno o mas campos enviados en el formato JSON a la API no correspondan
con los atributos de las clases DTO, se pueden utilizar alias

```java
public record DatosCompra(
        @JsonAlias("producto_id") Long idProducto,
        @JsonAlias("fecha_compra") LocalDate fechaCompra
        ){}
```

La anotación `@JsonAlias` sirve para mapear *alias* alternativos para los campos
que se recibirán del JSON, y es posible asignar múltiples alias:

```java
public record DatosCompra(
        @JsonAlias({"producto_id", "id_producto"}) Long idProducto,
        @JsonAlias({"fecha_compra", "fecha"}) LocalDate fechaCompra
        ){}
```

### Formato fechas

Spring usa un formato definido para las fechas `LocalDateTime`, sin embargo, estas
se pueden personalizar. Por ejemplo, para aceptar el formato `dd/mm/yyy hh:mm`.
Para ello se utiliza la anotación `@JsonFormat`

```java
@NotNull
@Future
@JsonFormat(pattern = "dd/MM/yyyy HH:mm")
LocalDateTime fecha
```

Esta anotación también se puede utilizar en las clases DTO que representan la
información que devuelve la API, para que el JSON devuelto se formatee de
acuerdo con el patrón configurado.
Además, no se limita solo a la clase `LocalDateTime`, sino que también se puede
utilizar en atributos de tipo `LocalDate` y `LocalTime`.

---

### Patrón Service

El ***Service pattern*** es muy utilizado en la programación y su nombre es muy
conocido. Pero a pesar de ser un nombre único, **Service** puede ser interpretado
de varias maneras:

- Puede ser un caso de uso, **Application Service**
- Un **Domain Service**, que tiene reglas de su dominio
- Un **Infrastructure Service**, que utiliza algún paquete externo para realizar
tareas
- etc

A pesar de que la interpretación puede ocurrir de varias formas, la idea detrás
del patrón es separar las reglas de negocio, las reglas de la aplicación y las
reglas de presentación para que puedan ser fácilmente probadas y reutilizadas
en otras partes del sistema.

Existen dos formas más utilizadas para crear **Services**. Puede crear
**Services** más genéricos, responsables de todas las asignaciones de un
**Controller**. O ser aún más específico, aplicando así la S del ***SOLID***:
***Single Responsibility Principle*** (*Principio de Responsabilidad Única*).
Este principio nos dice que una clase/función/archivo debe tener sólo una
única responsabilidad.

Piense en un sistema de ventas, en el que probablemente tendríamos algunas
funciones como:

- Registrar usuario
- Iniciar sesión
- Buscar productos
- Buscar producto por nombre
- etc

Entonces, se podrían crear los siguientes **Services**:

- RegistroDeUsuarioService
- IniciarSesionService
- BusquedaDeProductosService
- etc

Pero es importante estar atentos, ya que muchas veces no es necesario crear un
**Service** y, por lo tanto, agregar otra capa y complejidad innecesarias a
una aplicación. Una regla que se puede utilizar es la siguiente:

***si no hay reglas de negocio, simplemente se puede realizar la comunicación
directa entre los controllers y los repositories de la aplicación.***

---

## Principios SOLID

**SOLID** es un acrónimo que representa cinco principios de programación

- Principio de **Responsabilidad Única** (*Single Responsibility Principle*)
- Principio **Abierto-Cerrado** (*Open-Closed Principle*)
- Principio de **Sustitución de Liskov** (*Liskov Substitution Principle*)
- Principio de **Segregación de Interfaces** (*Interface Segregation Principle*)
- Principio de **Inversión de Dependencia** (*Dependency Inversion Principle*)

Cada principio representa una buena práctica de programación que, cuando se
aplica en una aplicación, facilita mucho su mantenimiento y extensión. Estos
principios fueron creados por *Robert Martin*, conocido como *Uncle Bob*, en su
artículo ***Design Principles and Design Patterns***.


---

## Reglas de negocio

Para cada validación de las reglas de negocio, se crea una clase específica.
La primera regla trata sobre el horario de la clínica: ***"El horario de
funcionamiento de la clínica es de lunes a sábado, de 07:00 a 19:00"***.
Si llega una solicitud a nuestra API con una fecha programada para una consulta,
¿qué sucede si el cliente intenta programar una consulta para un domingo?
¿O para un lunes a las 4 de la mañana? Por esto es necesario validar el horario
de la consulta.

Creción de subpaquete `consulta.validaciones`.

Dentro de `validaciones`, se crean las clases. Primero, una nueva clase llamada
`HorarioDeFuncionamientoClinica`. La idea es crear un método dentro de esta
clase para realizar la validación del horario de funcionamiento de la clínica.
Crearemos el método `validar()` y recibiremos el DTO `DatosAgendarConsulta` como
parámetro.

```java
    public class HorarioDeFuncionamientoClinica{
        public void validar(DatosAgendarConsulta datos) {
            var domingo = DayOfWeek.SUNDAY.equals(datos.fecha().getDayOfWeek());
            var antesdDeApertura=datos.fecha().getHour()<7;
            var despuesDeCierre=datos.fecha().getHour()>19;
            if(domingo || antesdDeApertura || despuesDeCierre){
                throw  new ValidationException(
                        "El horario de atención de la clínica es de lunes a
                       +"sábado, de 07:00 a 19:00 horas");
            }
        }
    }
```

Esto concluye la primera validación. El único objetivo de esta clase es
ejecutar esa única validación. El código queda pequeño, simple y fácil de dar
mantenimiento y de probar de manera automatizada.

La siguiente validación de la lista es ***"Las consultas tienen una duración
fija de 1 hora"***. Esta validación será implícita, la aplicación estará
disponible de una en una hora para agendar la consulta.

Siguiente validación: ***"Las consultas deben ser agendadas con un mínimo de 30
minutos de antelación"***.

Dentro del paquete de validaciones, se crea una nueva clase llamada
`HorarioDeAnticipacion`.Con un método similar al creado anteriormente.

```java
public void validar(DatosAgendarConsulta datos) {
    var ahora = LocalDateTime.now();
    var horaDeConsulta= datos.fecha();
    var diferenciaDe30Min= Duration.between(ahora,horaDeConsulta).toMinutes()<30;
    if(diferenciaDe30Min){
        throw new ValidationException(
                "Las consultas deben programarse con al "
                +"menos 30 minutos de anticipación");
    }
}
```

En la clase `MedicoActivo`, la única diferencia es el uso del Repositorio.
Se está buscando solo el atributo activo del médico, filtrando por el Ii.
Y si el médico no tiene ID, lanza una excepción.

Se creó el método `findAtivoByID`. En este caso, no se quiere cargar el objeto
completo del médico solo para verificar si el atributo activo es `true`.
Entonces, se puede hacer una consulta personalizada trayendo solo un único
atributo, `SELECT m.activo`.

Luego en la clase `MedicoConConsulta` también es necesario consultar la base de
datos para verificar si hay una consulta con este médico en la misma fecha.

Se realiza la consulta usando el patrón de nomenclatura de SpringData:
`existsByMedicoIdAndFecha(idMedico, fecha)`.

`PacienteActivo` se parece a `ValidadorMedicoActivo`.
Con la validación para que el paciente no tenga consulta en la misma fecha,
`PacienteSinConsulta`. También consulta la base de datos para ver si hay una
consulta para este paciente, colocando la fecha de inicio y la fecha de
finalización de ese día, tomando la primera y la última hora del día.

Se crea una interfáz `ValidadorDeConsultas` y dentro de esta se declara el
método que las clases tienen en común, `validar(DatosAgendarConsulta datos)`.
No es necesario usar la palabra clave `public` ya que ***es implícito que todos
los métodos de una interfaz son públicos***.

```java
    public interface ValidadorDeConsultas {
        public void validar(DatosAgendarConsulta datos);
    }
```

De esta manera, se estandariza el proyecto. Cada validador debe implementa esta
interfaz y obligatoriamente, deben implementar el método
`validar(DatosAgendarConsulta)`. Solo el cuerpo del método de cada clase será
diferente.

Otra cosa importante: para poder inyectar estas clases en algún lugar, Spring
necesita conocerlas. Entonces, encima de ellas debe haber alguna anotación de
Spring.

Se podría usar la anotación `@Service` para indicar que es un servicio, una
validación. Pero se usa la anotación `@Component` que es para componentes
genéricos. Porque en ocasiones se tiene una clase que no es ni una clase de
configuración, ni un controlador ni un servicio. Entonces el `@Component`
indica a Spring que esta clase es un componente genérico y lo cargará en la
inicialización del proyecto. Podría ser `@Service` también. No olvidar las
respectivas anotaciones `@Autowired`

En la interfaz no es necesario colocar ninguna anotación porque el Spring la
carga automáticamente.

Para inyectar estos validadores en la clase service, la clase
`AgendaDeConsultaService`, se usa un esquema de Spring que facilita la vida.

Se puede pensar que se necesita inyectar cada uno de los validadores de la misma
manera que se están inyectando los repositorios. Pero ese es el problema que
se quiere evitar, no se desea declararlos uno por uno. Así que se hace un "truco"
que Spring permite hacer.

En el código de AgendaDeConsultaService, se declara un atributo, con la anotación
`@Autowired`, pero este atributo es declarado como una **lista** `java.util.List`
y, dentro de la lista, se declara la **interfaz** `ValidadorDeConsultas` y es
llamada `validadores`.

```java
@Autowired
List<ValidadorDeConsultas> validadores;
```

Spring identifica automáticamente que se esta inyectando una lista y buscará
todas las clases que implementan la interfaz `ValidadorDeConsultas`. Así, no
importa la cantidad de validadores, Spring inyectará uno por uno. Es mucho más
práctico hacerlo de esta manera.

En el método agendar(), antes de crear las variables del paciente y del médico,
se obtener esta lista de validadores y se recorre con
`forEach(v -> v.validar(datos))`.

```java
validadores.forEach(v-> v.validar(datos));
```

De esta forma se logran inyectar todos los validadores y el código queda bastante
flexible.

Si se quiere excluir un validador, basta con eliminar la clase de ese validador.
No es necesario modificar la clase `AgendaDeConsultaService`, la lista simplemente
quedará con una clase menos.

Si cambia una validación o si deja de existir, no es necesario modificar la clase
de servicio.

Probar agendar una cita, **POST** `http://localhost:8080/consultas`

```json
{
    "idPaciente": 1,
        "idMedico": 1,
        "fecha": "2023-09-18T10:00,
}
```

```java
public DatosDetalleConsulta agendar(DatosAgendarConsulta datos){
   if(!pacienteRepository.findById(datos.idPaciente()).isPresent()){
       throw new ValidacionDeIntegridad("este id para el paciente no fue encontrado");
   }
   if(datos.idMedico()!=null && !medicoRepository.existsById(datos.idMedico())){
       throw new ValidacionDeIntegridad("este id para el medico no fue encontrado");
   }
   validadores.forEach(v-> v.validar(datos));
   var paciente = pacienteRepository.findById(datos.idPaciente()).get();
   var medico = seleccionarMedico(datos);
   if(medico==null){
       throw new ValidacionDeIntegridad("No hay especialistas disponibles "
                                       +" para este horario");
   }
   var consulta = new Consulta(medico,paciente,datos.fecha());
   consultaRepository.save(consulta);
   return new DatosDetalleConsulta(consulta);
}
```

El constructor que recibe el objeto consulta en DatosDetalleConsulta

```java
public record DatosDetalleConsulta(Long id,
                                   Long idPaciente,
                                   Long idMedico,
                                   LocalDateTime fecha) {
   public DatosDetalleConsulta(Consulta consulta) {
       this(consulta.getId(),
            consulta.getPaciente().getId(),
            consulta.getMedico().getId(),
            consulta.getFecha());
   }
}
```

Se tiene un DTO con los datos siendo devueltos correctamente.

En `ConsultaController`, el método `agendar()` devuelve el DTO. Se guarda el DTO
en una variable y en `ResponseEntity.ok` se pasa el DTO que fue devuelto por el
service.

```java
@PostMapping
@Transactional
public ResponseEntity agendar(@RequestBody @Valid DatosAgendarConsulta datos) {
   var dto= service.agendar(datos);
   return ResponseEntity.ok(dto);
}
```

El envio de un paciente que no existe en la base de datos:

**POST** `http://localhost:8080/consultas`

```json
{
    "idPaciente": 88888,
    "idMedico": 1,
    "fecha": "2023-10-10T10:00"
}
```

En el paquete errores en clase
ManejadorDeErrores
se crea un nuevo método para `ValidationException`:

```java
@ExceptionHandler(ValidationException.class)
public ResponseEntity errorHandlerValidacionesDeNegocio(Exception e){
   return ResponseEntity.badRequest().body(e.getMessage());
}
```

---

## Documentación

[SpringDoc](https://springdoc.org/)

La documentación es algo muy importante en un proyecto, especialmente si se trata
de una API Rest, ya que en este caso podemos tener varios clientes que necesiten
comunicarse con ella y necesiten documentación que les enseñe cómo realizar esta
comunicación de manera correcta.

Durante mucho tiempo no existió un formato estándar para documentar una API Rest,
hasta que en 2010 surgió un proyecto conocido como ***Swagger***, cuyo objetivo
era ser una especificación **open source** para el diseño de APIs Rest. Después
de un tiempo, se desarrollaron algunas herramientas para ayudar a los
desarrolladores a implementar, visualizar y probar sus APIs, como ***Swagger UI,
Swagger Editor y Swagger Codegen***, lo que lo convirtió en un proyecto muy
popular y utilizado en todo el mundo.

En 2015, Swagger fue comprado por la empresa ***SmartBear Software***, que donó
la parte de la especificación a la ***Fundación Linux***. A su vez, la
fundación renombró el proyecto a **OpenAPI**. Después de esto, se creó la
***OpenAPI Initiative***, una organización centrada en el desarrollo y la
evolución de la especificación OpenAPI de manera abierta y transparente.

OpenAPI es actualmente la especificación más utilizada y también la principal
para documentar una API Rest. La documentación sigue un patrón que puede ser
descrito en formato **YAML** o **JSON**, lo que facilita la creación de
herramientas que puedan leer dichos archivos y automatizar la creación de
documentación, así como la generación de código para el consumo de una API.

Más detalles en el sitio web oficial de la
[OpenAPI Initiative](https://www.openapis.org/).

Dependencia en pom.xml

```xml
   <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.2.0</version>
   </dependency>
```

Permitir acceso a urls de **SpringDoc** sin token

SecurityConfigurations

```java
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity)
        throws Exception {
                ...
                .requestMatchers(HttpMethod.POST, "/login").permitAll()
                .requestMatchers(HttpMethod.GET,
                                    "/swagger_ui.html",
                                    "/swagger_ui/**",
                                    "/v3/api-docs/**").permitAll()
                ...
```

URLS:

- `http://localhost:8080/v3/api-docs`
- `localhost:8080/swagger-ui.html`

Nuevo *package*
`api.infra.springdoc`
y clase
SpringDocConfigurations

```java
@Configuration
public class SpringDocConfigurations {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI().components(new Components()
                .addSecuritySchemes(
                        "bearer-key",
                        new SecurityScheme().type(SecurityScheme.Type.HTTP)
                                .scheme("bearer")
                                .bearerFormat("JWT")));
    }

    @Bean
    public void message() {
        System.out.println("bearer is working");
    }
}
```

Se agrega la anotacion `@SecurityRequirement(name = "bearer-key")` en las clases
*controller* para que el **Swagger** habilite la funcionalidad de autentificar
por token.

---

## Pruebas Automatizadas

***¿Que se prueba?***

- Cotroller -> API
- Services -> Reglas de Negocio
- Repository -> Queries

#### Tipos de Tests

- Test de caja negra
- Test de caja blanca

### Propiedades de los Tests

application-test.yml
```yml
spring:
  datasource:
    url: jdbc:mysql://${TEST_DB_URL}?createDatabaseIfNotExist=true&serverTimezone=UTC
    username: ${DB_USER}
    password: ${DB_PASS}
```

### Creación de Tests

#### Test MedicoRepository

Se crea el *Package*
`test.java.med.voll.api.domain.medico`
y la clase
MedicoRepositoryTest
```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
@ActiveProfiles("test")
class MedicoRepositoryTest {

    @Autowired
    private MedicoRepository medicoRepository;
    @Autowired
    private TestEntityManager em;

    @Test
    @DisplayName("Debe retornar null cuando el médico tenga una consulta en ese horario")
    void seleccionarMedicoConEspecialidadEnFechaEscenario1() {

        var proximoLunes10H = LocalDate.now()
                .with(TemporalAdjusters.next(DayOfWeek.MONDAY))
                .atTime(10, 0);

        var medico = registrarMedico("Jose",
                                     "jose@mail.com",
                                     "123456",
                                     Especialidad.CARDIOLOGIA);
        var paciente = registrarPaciente("antonio","antonio@mail.com","654321");
        registrarConsulta(medico, paciente, proximoLunes10H);

        var medicoLibre = medicoRepository.seleccionarMedicoConEspecialidadEnFecha(
                                     Especialidad.CARDIOLOGIA,
                                     proximoLunes10H);
        assertNull(medicoLibre);
    }

    @Test
    @DisplayName("Debe retornar el médico ingresado como disponible en ese horario")
    void seleccionarMedicoConEspecialidadEnFechaEscenario2() {

        var proximoLunes10H = LocalDate.now()
                .with(TemporalAdjusters.next(DayOfWeek.MONDAY))
                .atTime(10, 0);

        var medico = registrarMedico("Jose",
                                     "jose@mail.com",
                                     "123456",
                                     Especialidad.CARDIOLOGIA);
        var medicoLibre = medicoRepository.seleccionarMedicoConEspecialidadEnFecha(
                                     Especialidad.CARDIOLOGIA,
                                     proximoLunes10H) ;

        assertEquals(medicoLibre, medico);
    }

    private void registrarConsulta(Medico medico,
                                   Paciente paciente,
                                   LocalDateTime fecha) {
        em.persist(new Consulta(null, medico, paciente, fecha, null));
    }

    private Medico registrarMedico(String nombre,
                                   String email,
                                   String documento,
                                   Especialidad especialidad) {
        var medico = new Medico(datosMedico(nombre, email, documento, especialidad));
        em.persist(medico);
        return medico;
    }

    private Paciente registrarPaciente(String nombre,
                                       String email,
                                       String documento) {
        var paciente = new Paciente(datosPaciente(nombre, email, documento));
        em.persist(paciente);
        return paciente;
    }

    private DatosRegistroMedico datosMedico(String nombre,
                                            String email,
                                            String documento,
                                            Especialidad especialidad) {
        return new DatosRegistroMedico(
                nombre,
                email,
                "61999999999",
                documento,
                especialidad,
                datosDireccion()
        );
    }

    private DatosRegistroPaciente datosPaciente(String nombre,
                                                String email,
                                                String documento) {
        return new DatosRegistroPaciente(
                nombre,
                email,
                "61999999999",
                documento,
                datosDireccion()
        );
    }

    private DatosDireccion datosDireccion() {
        return new DatosDireccion(
                "Loca",
                "Azul",
                "Acapulco",
                "321",
                "12"
        );
    }

}
```

### Test ConsultaController

Clase
ConsultaControllerTest
en *package*
`test.java.med.voll.api.controler`

```java
package med.voll.api.controller;

import med.voll.api.domain.consulta.AgendaDeConsultaService;
import med.voll.api.domain.consulta.DatosAgendarConsulta;
import med.voll.api.domain.consulta.DatosDetalleConsulta;
import med.voll.api.domain.medico.Especialidad;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.json.AutoConfigureJsonTesters;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.json.JacksonTester;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.web.servlet.MockMvc;

import java.time.LocalDateTime;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;

@SpringBootTest
@AutoConfigureMockMvc
@AutoConfigureJsonTesters
@ActiveProfiles("test")
class ConsultaControllerTest {

    @Autowired
    private MockMvc mvc;

    @Autowired
    private JacksonTester<DatosAgendarConsulta> agendarConsultaJacksonTester;

    @Autowired
    private JacksonTester<DatosDetalleConsulta> detalleConsultaJacksonTester;

    @MockBean
    private AgendaDeConsultaService agendaDeConsultaService;

    @Test
    @DisplayName("Debe retornar estado http 400 cuando datos ingresados no sean válidos")
    @WithMockUser
    void agendarEscenario1() throws Exception {
        // given - when
        var response  = mvc.perform(post("/consultas")).andReturn().getResponse();
        // then
        assertEquals(HttpStatus.BAD_REQUEST.value(), response.getStatus());
    }

    @Test
    @DisplayName("Debe retornar estado http 200 cuando datos ingresados sean válidos")
    @WithMockUser
    void agendarEscenario2() throws Exception {

        //given
        var fecha = LocalDateTime.now().plusHours(1);
        var especialidad = Especialidad.CARDIOLOGIA;
        var datos = new DatosDetalleConsulta(null,2l,5l,fecha);

        // when
        when(agendaDeConsultaService.agendar(any())).thenReturn(datos);

        var response = mvc.perform(post("/consultas")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(agendarConsultaJacksonTester.write(
                            new DatosAgendarConsulta(
                                2l,
                                5l,
                                fecha,
                                especialidad))
                            .getJson()))
                        .andReturn().getResponse();

        //then
        assertEquals(HttpStatus.OK.value(), response.getStatus());

        var jsonEsperado = detalleConsultaJacksonTester.write(datos).getJson();

        assertEquals(response.getContentAsString(), jsonEsperado);
    }

}
```

---

## Build del proyecto

Migración de configuración de desarrollo `application.properties` (xml) a
`application.yml`. Con esto se tienen 3 perfiles de configuración:

- **DEV** application.yml
- **TEST** application-test.yml
- **PROD** application-prod.yml

Una forma de hacer la ***build*** del proyecto es en la pestañaa de maven en:
`api -> Lifecycle -> package` *click-secundario* y `Run Maven Build`.

Para correr la aplicación pasando manualmente la configuración y variables de
entorno:

```sh
java -DDATASOURCE=jdbc:mysql//<DB_URL>/vollmed_api \
     -DDATASOURCE_USERNAME=<USER> \
     -DDATASOURCE_PASSWORD=<PASSWORD> \
     -jar target/api-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

#### WAR

Los proyectos que utilizan Spring Boot generalmente usan el formato `jar` para
empaquetar la aplicación. Sin embargo, Spring Boot brinda soporte para empaquetar
en formato `war`, que era ampliamente utilizado en aplicaciones Java antiguas.

Para hacer el build del proyecto empaquete la aplicación en un archivo en formato
`war`, se deben realizar los siguientes cambios:

1. Agregar la etiqueta `<packaging>war</packaging>` al archivo `pom.xml` del
proyecto, y esta etiqueta debe ser hija de la etiqueta raíz `<project>`

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>3.0.0</version>
   <relativePath/> <!-- lookup parent from repository -->
   </parent>
   <groupId>med.voll</groupId>
   <artifactId>api</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <name>api</name>
   <packaging>war</packaging>
   ```

2. Agregar la siguiente dependencia:

   ```xml
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-tomcat</artifactId>
     <scope>provided</scope>
   </dependency>
   ```

3. Modificar en la clase principal del proyecto `ApiApplication` para heredar de
la clase `SpringBootServletInitializer`, y sobrescribir el método `configure`:

   ```java
   @SpringBootApplication
   public class ApiApplication extends SpringBootServletInitializer {
       @Override
       protected SpringApplicationBuilder configure(
                                           SpringApplicationBuilder application) {
           return application.sources(ApiApplication.class);
       }

       public static void main(String[] args) {
           SpringApplication.run(ApiApplication.class, args);
       }
   }
   ```

Ahora, al realizar el build del proyecto, se generará un archivo con la
extensión `war` en el directorio target, en lugar del `jar`.

#### GraalVM Native Image

Una de las características más destacadas de la versión 3 de Spring Boot es el
soporte para imágenes nativas, algo que reduce significativamente el consumo de
memoria y el tiempo de inicio de una aplicación. Algunos otros frameworks
competidores de Spring Boot, como ***Micronaut*** y ***Quarkus***, ya ofrecían
soporte para esta función.

De hecho, era posible generar imágenes nativas en aplicaciones con Spring Boot
antes de la versión 3, pero esto requería el uso de un proyecto llamado
***Spring Native*** que agregaba soporte para ello. Con la llegada de la versión
3 de Spring Boot, ya no es necesario utilizar este proyecto.

#### Native Image

Una imagen nativa es una tecnología utilizada para compilar una aplicación Java,
incluyendo todas sus dependencias, generando un archivo binario ejecutable que
puede ser ejecutado directamente en el sistema operativo sin necesidad de utilizar
la JVM. Aunque no se ejecute en una JVM, la aplicación también contará con sus
recursos, como la gestión de memoria, el recolector de basura y el control de la
ejecución de hilos.

> Documentación tecnología de [imágenes nativas](https://www.graalvm.org/native-image).

#### Native Imagen com Spring Boot 3

Una forma muy sencilla de generar una imagen nativa de la aplicación es mediante
un plugin de Maven, que debe incluirse en el archivo `pom.xml`:

```xml
<plugin>
  <groupId>org.graalvm.buildtools</groupId>
  <artifactId>native-maven-plugin</artifactId>
</plugin>
```

Esta es la única modificación necesaria en el proyecto. Después de esto, la
generación de la imagen debe hacerse a través de la terminal, con el siguiente
comando de Maven que se ejecuta en el directorio raíz del proyecto:
`./mvnw -Pnative native:compile`

Este comando puede tardar varios minutos en completarse, lo cual es completamente
normal.

> **NOTE** Para ejecutar el comando anterior y generar la imagen nativa del
proyecto, es necesario que tener instalado en ***GraalVM*** *(una máquina virtual
Java con soporte para la función Native Image)* en una versión igual o superior
a **22.3**.

Después de que el comando anterior termine, se generará en la terminal un registro
como el siguiente:

```log
Top 10 packages in code area:           Top 10 object types in image heap:
   3,32MB jdk.proxy4                      19,44MB byte[] for embedded resources
   1,70MB sun.security.ssl                16,01MB byte[] for code metadata
   1,18MB java.util                        8,91MB java.lang.Class
 936,28KB java.lang.invoke                 6,74MB java.lang.String
 794,65KB com.mysql.cj.jdbc                6,51MB byte[] for java.lang.String
 724,02KB com.sun.crypto.provider          4,89MB byte[] for general heap data
 650,46KB org.hibernate.dialect            3,07MB c.o.s.c.h.DynamicHubCompanion
 566,00KB org.hibernate.dialect.function   2,40MB byte[] for reflection metadata
 563,59KB com.oracle.svm.core.code         1,30MB java.lang.String[]
 544,48KB org.apache.catalina.core         1,25MB c.o.s.c.h.DynamicHu~onMetadata
  61,46MB for 1482 more packages           9,74MB for 6281 more object types
--------------------------------------------------------------------------------
    9,7s (5,7% of total time) in 77 GCs | Peak RSS: 8,03GB | CPU load: 7,27
--------------------------------------------------------------------------------
Produced artifacts:
 /home/rodrigo/Desktop/api/target/api (executable)
 /home/rodrigo/Desktop/api/target/api.build_artifacts.txt (txt)
================================================================================
Finished generating 'api' in 2m 50s.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  03:03 min
[INFO] Finished at: 2023-01-17T12:13:04-03:00
[INFO] ------------------------------------------------------------------------
```

La imagen nativa se genera en el directorio `target/`, junto con el archivo `jar`
de la aplicación, como un archivo ejecutable de nombre api.

A diferencia del archivo `jar`, que se ejecuta en la JVM mediante el comando
`java -jar`, la imagen nativa es un archivo binario y debe ejecutarse directamente
desde la terminal: `./target/api`

Al ejecutar el comando anterior se generará el registro de inicio de la
aplicación, que al final muestra el tiempo que tardó la aplicación en iniciarse:

```log
INFO 127815 --- [restartedMain] med.voll.api.ApiApplication : Started ApiApplication in 0.3 seconds (process running for 0.304)
```

La aplicación tardó menos de medio segundo en iniciarse, algo realmente
impresionante, ya que cuando se ejecuta en la JVM, a través del archivo `jar`,
este tiempo aumenta a alrededor de 5 segundos.

