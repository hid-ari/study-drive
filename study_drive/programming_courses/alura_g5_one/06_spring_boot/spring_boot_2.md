# API Rest Java - Buenas prácticas y protección

Continuación de [Desarrollo de una API Rest](spring_boot_1.md) donde se vio:

- Creación de un API Rest
- Crud (Create, Read, Update, Delete)
- Validaciones
- Paginación y orden

### Objetivos

- Buenas prácticas al desarrollar un API
- Tratamiento de errores
- Autenticación y Autorización
- Tokens JWT

## Buenas prácticas

Se modifican las respuestas de la API


```java
public record DatosRespuestaMedico(
                        @NotNull Long id, String nombre,
                        String email, String telefono, String documento,
                        DatosDireccion direccion) {}
```


```java
@RestController
@RequestMapping("/medicos")
public class MedicoController {

    @Autowired
    private MedicoRepository medicoRepository;

    @PostMapping
    public ResponseEntity<DatosRespuestaMedico> registrarMedico(
                @RequestBody @Valid DatosRegistroMedico datosRegistroMedico,
                UriComponentsBuilder uriComponentsBuilder) {
        Medico medico = medicoRepository.save(new Medico(datosRegistroMedico));
        DatosRespuestaMedico datosRespuestaMedico = new DatosRespuestaMedico(
                medico.getId(),
                medico.getNombre(),
                medico.getEmail(),
                medico.getTelefono(),
                medico.getDocumento(),
                new DatosDireccion(
                        medico.getDireccion().getCalle(),
                        medico.getDireccion().getDistrito(),
                        medico.getDireccion().getCiudad(),
                        medico.getDireccion().getNumero(),
                        medico.getDireccion().getComplemento()
                )
        );
        URI url = uriComponentsBuilder.path("/medicos/{id}")
                    .buildAndExpand(medico.getId()).toUri();
        return  ResponseEntity.created(url).body(datosRespuestaMedico);
    }

    @GetMapping
    public Page<DatosListadoMedicos> listadoMedicos(@PageableDefault(size = 5)
                Pageable paginacion) {
        return medicoRepository.findByActivoTrue(paginacion)
                .map(DatosListadoMedicos::new);
    }

    @PutMapping
    @Transactional
    public ResponseEntity actualizarMedico(@RequestBody @Valid DatosActualizarMedico
        datosActualizarMedico) {
        Medico medico = medicoRepository.getReferenceById(datosActualizarMedico.id());
        medico.actualizarDatos(datosActualizarMedico);
        return ResponseEntity.ok(
            new DatosRespuestaMedico(
                medico.getId(),
                medico.getNombre(),
                medico.getEmail(),
                medico.getTelefono(),
                medico.getDocumento(),
                new DatosDireccion(
                        medico.getDireccion().getCalle(),
                        medico.getDireccion().getDistrito(),
                        medico.getDireccion().getCiudad(),
                        medico.getDireccion().getNumero(),
                        medico.getDireccion().getComplemento()
                )
        ));
    }

    @DeleteMapping("/{id}")
    @Transactional
    public ResponseEntity eliminarMedico(@PathVariable Long id) {
        Medico medico = medicoRepository.getReferenceById(id);
        medico.desactivarMedico();
        return ResponseEntity.noContent().build();
    }
}
```

## Codigos de respuesta del protocolo HTTP

El protocolo HTTP (***Hypertext Transfer Protocol***, *RFC 2616*) es el protocolo
encargado de realizar la comunicación entre el cliente, que suele ser un
navegador, y el servidor. De esta forma, para cada *solicitud* realizada por el
cliente, el servidor responde si tuvo éxito o no. Si no tiene éxito, la mayoría
de las veces, la respuesta del servidor será una secuencia numérica acompañada
de un mensaje.

Categoría de código
Los códigos **HTTP** (o **HTTPS**) tienen tres dígitos, y el primer dígito
representa la clasificación dentro de las cinco categorías posibles.

- **`1XX` Informativo:** la solicitud fue aceptada o el proceso aún está en curso
- **`2XX` Confirmación:** la acción se completó o se comprendió
- **`3XX` Redirección:** indica que se debe hacer o se debió hacer algo más para
completar la solicitud
- **`4XX` Error del cliente:** indica que la solicitud no se puede completar o
contiene una sintaxis incorrecta
- **`5XX` Error del servidor:** el servidor falló al concluir la solicitud.

### Principales códigos de error

Estos permiten comprender mejor la comunicación de su navegador con el servidor
de la aplicación que se intenta acceder.

#### Error 403

El código 403 es el error **"Prohibido"**. Significa que el servidor entendió
la solicitud del cliente, pero se niega a procesarla, ya que el cliente no está
autorizado para hacerlo.

#### Error 404

Mensaje de Error 404, significa que la URL no lo llevó a ninguna parte.
Puede ser que la aplicación ya no exista, que la URL haya cambiado o que haya
ingresado una URL incorrecta.

#### Error 500

Es un error menos común, pero aparece de vez en cuando. Este error significa que
hay un problema con una de las bases que hace que se ejecute una aplicación.
Básicamente, este error puede estar en el servidor que mantiene la aplicación
en línea o en la comunicación con el sistema de archivos, que proporciona la
infraestructura para la aplicación.

#### Error 503

El error 503 significa que el servicio al que se accede no está disponible
temporalmente. Las causas comunes son un servidor que está fuera de servicio
por mantenimiento o sobrecargado. Los ataques maliciosos como ***DDoS***
causan mucho este problema.

Para consultar sobre algún código HTTP, se puede usar
[http cat](https://http.cat)

ejm. consulta por código `405 Method Not Allowed`

```http
https://http.cat/405
```

## Manejando errores

- Spring boot common
[properties](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html)

- Spring boot server
[properties](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server)

#### Ocultando el stacktrace


```conf
server.error.include-stacktrace=never
```

Nuevo package infra con nueva clase

```java
@RestControllerAdvice
public class ManejadorDeErrores {

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity manejarError404(){
        return  ResponseEntity.notFound().build();
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity manejarError400(MethodArgumentNotValidException e){
        var errores = e.getFieldErrors().stream().map(DatosErrorValidacion::new).toList();
        return  ResponseEntity.badRequest().body(errores);
    }

    private record DatosErrorValidacion(String campo, String error) {
        public DatosErrorValidacion(FieldError error) {
            this(error.getField(), error.getDefaultMessage());
        }
    }
}
```

Por defecto, **Bean Validation** devuelve mensajes de error en inglés, sin
embargo, hay una traducción de estos mensajes al español ya implementada en
esta especificación.

En el protocolo HTTP hay un encabezado llamado `Accept-Language`, que sirve para
indicar al servidor el idioma preferido del cliente que activa la solicitud.
Podemos utilizar esta cabecera para indicarle a Spring el idioma deseado, para
que en la integración con Bean Validation pueda buscar mensajes según el idioma
indicado.

En Insomnia, y también en otras herramientas similares, existe una opción
llamada Header en la que podemos incluir cabeceras a enviar en la petición.
Si agregamos el encabezado `Accept-Language` con el valor `es`, los mensajes de
error de **Bean Validation** se devolverán automáticamente en español.

> Nota: Bean Validation solo traduce los mensajes de error a unos pocos idiomas.

### Personalización de mensajes de error

**Bean Validation** tiene un mensaje de error para cada una de sus anotaciones.
P.e. cuando la validación falla en algún atributo anotado con `@NotBlank`, el
mensaje de error será: `must not be blank`.

Estos mensajes de error no se definieron en la aplicación, ya que son mensajes
de error estándar de Bean Validation. Sin embargo, si lo desea, puede
personalizar dichos mensajes.

Una de las formas de personalizar los mensajes de error es agregar el atributo
del mensaje a las anotaciones de validación:

```java
public record DatosCadastroMedico(
    @NotBlank(message = "Nombre es obligatorio")
    String nombre,

    @NotBlank(message = "Email es obligatorio")
    @Email(message = "Formato de email es inválido")
    String email,

    @NotBlank(message = "Teléfono es obligatorio")
    String telefono,

    @NotBlank(message = "CRM es obligatorio")
    @Pattern(regexp = "\\d{4,6}", message = "Formato do CRM es inválido")
    String crm,

    @NotNull(message = "Especialidad es obligatorio")
    Especialidad especialidad,

    @NotNull(message = "Datos de dirección son obligatorios")
    @Valid DatosDireccion direccion) {}
```

Otra forma es aislar los mensajes en un archivo de propiedades, que debe tener
el nombre `ValidationMessages.properties` y estar creado en el directorio
`src/main/resources`:

```config
nombre.obligatorio=El nombre es obligatorio
email.obligatorio=Correo electrónico requerido
email.invalido=El formato del correo electrónico no es válido
phone.obligatorio=Teléfono requerido
crm.obligatorio=CRM es obligatorio
crm.invalido=El formato CRM no es válido
especialidad.obligatorio=La especialidad es obligatoria
address.obligatorio=Los datos de dirección son obligatorios
```

Y, en las anotaciones, indicar la clave de las propiedades por el propio atributo
message, delimitando con los caracteres `{` y `}`:

```java
public record DatosRegistroMedico(
    @NotBlank(message = "{nombre.obligatorio}")
    String nombre,

    @NotBlank(message = "{email.obligatorio}")
    @Email(message = "{email.invalido}")
    String email,

    @NotBlank(message = "{telefono.obligatorio}")
    String telefono,

    @NotBlank(message = "{crm.obligatorio}")
    @Pattern(regexp = "\\d{4,6}", message = "{crm.invalido}")
    String crm,

    @NotNull(message = "{especialidad.obligatorio}")
    Especialidad especialidad,

    @NotNull(message = "{direccion.obligatorio}")
    @Valid DatosDireccion direccion) {}
```

### Seguridad

- Autenticación
- Autorización
- Protección contra ataques (CSRF, clickjacking)

```mermaid
%%{init: {'theme': 'dark','themeVariables': {'clusterBkg': '#2b2f38'}, 'flowchart': {'curve': 'cardinal'}}}%%
flowchart
direction TB
DB[(BBDD)]
subgraph "Autenticación"
subgraph APP["App o Web"]
direction LR
WEB("User
Pass")
end
subgraph REQ["HTTP Request"]
direction TB
DT{"user
pass"}
JWT{JWT}
end
subgraph API[API REST]
direction LR
Aplicación
end
APP --"user & pass"--> DT
JWT --"token"--> APP
REQ <--> API
API <--SQL--> DB
end
```

### Hash de contraseña

Al implementar una funcionalidad de autenticación en una aplicación,
independientemente del lenguaje de programación utilizado, deberá tratar con
los datos de inicio de sesión y contraseña de los usuarios, y deberán
almacenarse en algún lugar, como, por ejemplo, una base de datos.

Las contraseñas son información confidencial y no deben almacenarse en texto
sin formato, ya que si una persona malintencionada logra acceder a la base de
datos, podrá acceder a las contraseñas de todos los usuarios. Para evitar este
problema, siempre debe usar algún algoritmo hash en las contraseñas antes de
almacenarlas en la base de datos.

**Hashing** no es más que una función matemática que convierte un texto en otro
texto totalmente diferente y difícil de deducir. Por ejemplo, el texto *"Mi
nombre es Rodrigo"* se puede convertir en el texto
`8132f7cb860e9ce4c1d9062d2a5d1848`, utilizando el algoritmo ***hash MD5***.

Un detalle importante es que los algoritmos de hash deben ser unidireccionales,
es decir, no debe ser posible obtener el texto original a partir de un hash.
Así, para saber si un usuario ingresó la contraseña correcta al intentar
autenticarse en una aplicación, debemos tomar la contraseña que ingresó y
generar su hash, para luego compararla con el hash que está almacenado en la
base de datos.

Hay varios algoritmos hashing que se pueden usar para transformar las contraseñas
de los usuarios, algunos de los cuales son más antiguos y ya **no se consideran
seguros** en la actualidad, como **MD5** y **SHA1**. Los principales algoritmos
actualmente recomendados son:

- Bcrypt
- Scrypt
- Argon2
- PBKDF2

Se utilizará el algoritmo **BCrypt**, que es bastante popular hoy en día. Esta
opción también tiene en cuenta que ***Spring Security*** ya nos proporciona una
clase que lo implementa.

**Spring Data** usa su propio patrón de nomenclatura de métodos a seguir para
que pueda generar consultas SQL correctamente.

Hay algunas palabras reservadas que debemos usar en los nombres de los métodos,
como `findBy` y `existBy`, para indicarle a **Spring Data** cómo debe ensamblar
la consulta que queremos. Esta característica es bastante flexible y puede ser
un poco compleja debido a las diversas posibilidades existentes.

Para conocer más detalles y comprender mejor cómo ensamblar consultas dinámicas
con Spring Data, acceda a su
[documentación oficial](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/).

Bcrypt [online](https://www.browserling.com/tools/bcrypt)

#### Autenticación API

Se agregan las dependencias

```xml
    ...
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    ...
```

Creación de clases `Usuario`, `UsuarioRepository` y `DatosAutenticacionUsuario`
en *package*

```java
@Table(name = "usuarios")
@Entity(name = "Usuario")
@Getter
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(of = "id")
public class Usuario implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String login;
    private String clave;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return List.of(new SimpleGrantedAuthority("ROLE_USER"));
    }

    @Override
    public String getPassword() { return clave; }

    @Override
    public String getUsername() { return login; }

    @Override
    public boolean isAccountNonExpired() { return true; }

    @Override
    public boolean isAccountNonLocked() { return true; }

    @Override
    public boolean isCredentialsNonExpired() { return true; }

    @Override
    public boolean isEnabled() { return true; }
}
```



```java
public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    UserDetails findByLogin(String login);
}
```



```java
public record DatosAutenticacionUsuario(String login, String clave) {}
```

> En este punto no retorna *token*

```java
package med.voll.api.controller;
@RestController
@RequestMapping("/login")
public class AutenticacionController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @PostMapping
    public ResponseEntity autenticarUsuario(
      @RequestBody @Valid DatosAutenticacionUsuario datosAutenticacionUsuario) {
        Authentication token = new UsernamePasswordAuthenticationToken(
                datosAutenticacionUsuario.login(),
                datosAutenticacionUsuario.clave());
        authenticationManager.authenticate(token);
        return ResponseEntity.ok().build();

    }
}
```

Creación de clases `AutenticationService` y `SecurityConfigurations` en *package*


```java
package med.voll.api.infra.security;
@Service
public class AutenticacionService implements UserDetailsService {

    @Autowired
    private UsuarioRepository usuarioRepository;

    @Override
    public UserDetails loadUserByUsername(String login)
      throws UsernameNotFoundException {
        return usuarioRepository.findByLogin(login);
    }
}
```


```java
@Configuration
@EnableWebSecurity
public class SecurityConfigurations {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity)
      throws Exception {
        return httpSecurity.csrf().disable().sessionManagement()
                    .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                    .and().build();
    }

    @Bean
    public AuthenticationManager authenticationManager(
      AuthenticationConfiguration authenticationConfiguration)
      throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public PasswordEncoder passwordEncoder () {
        return new BCryptPasswordEncoder();
    }

}
```


```sql
create table usuarios(
    id bigint not null auto_increment,
    login varchar(100) not null unique,
    clave varchar(300) not null,
    primary key(id)
);
```

## JSON Web Token


```xml
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>4.4.0</version>
</dependency>
```


```java
@Service
public class TokenService {

    @Value("${api.security.secret}")
    private String apiSecret;

    public String generarToken(Usuario usuario) {
        try {
            Algorithm algorithm = Algorithm.HMAC256(apiSecret) ;
            return JWT.create()
                    .withIssuer("voll med")
                    .withSubject(usuario.getLogin())
                    .withClaim("id", usuario.getId())
                    .withExpiresAt(generarFechaExpiracion())
                    .sign(algorithm);
        } catch (JWTCreationException exception){
            throw new RuntimeException();
        }

    }

    private Instant generarFechaExpiracion() {
        return LocalDateTime.now()
                    .plusHours(2)
                    .toInstant(ZoneOffset.of("-03:00"));
    }
}
```

Creación de propiedades manejadas por variables de entorno/ambiente

```config
spring.datasource.url=jdbc:mysql://${DB_URL}/vollmed_api2
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASS}

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

server.error.include-stacktrace=never

api.security.secret=${JWT_SECRET}
```


```java
public record DatosJWTtoken(String jwTtoken) {}
```


```java
@RestController
@RequestMapping("/login")
public class AutenticacionController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private TokenService tokenService;

    @PostMapping
    public ResponseEntity autenticarUsuario(
      @RequestBody @Valid DatosAutenticacionUsuario datosAutenticacionUsuario) {
        Authentication authtoken = new UsernamePasswordAuthenticationToken(
                datosAutenticacionUsuario.login(),
                datosAutenticacionUsuario.clave());
        var usuarioAutenticado = authenticationManager.authenticate(authtoken);
        var JWTtoken = tokenService.generarToken(
                (Usuario) usuarioAutenticado.getPrincipal()
        );
        return ResponseEntity.ok(new DatosJWTtoken(JWTtoken));
    }
}
```

## Control de acceso

### Filters

**Filter** es una de las características que componen la especificación 
Servlets, que estandariza el manejo de solicitudes y respuestas en aplicaciones
web en Java. Es decir, dicha función no es específica de Spring y, por lo tanto,
puede usarse en cualquier aplicación Java.

Es una característica muy útil para aislar códigos de infraestructura de la
aplicación, como por ejemplo, seguridad, logs y auditoría, para que dichos
códigos no se dupliquen y se mezclen con códigos relacionados con las reglas
comerciales de la aplicación.

Para crear un **Filter**, simplemente cree una clase e implemente la interfaz
Filter en ella (paquete `jakarta.servlet`). Por ejemplo:

```java
@WebFilter(urlPatterns = "/api/**")
public class LogFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest,
                         ServletResponse servletResponse,
                         FilterChain filterChain
                         ) throws IOException, ServletException {
        System.out.println("Solicitud recibida el: " + LocalDateTime.now());
        filterChain.doFilter(servletRequest, servletResponse);
    }

}
```

El método `doFilter` es llamado por el servidor automáticamente, cada vez que
este filter tiene que ser ejecutado, y la llamada al método `filterChain.doFilter`
indica que los siguientes filters, si hay otros, pueden ser ejecutados. La
anotación `@WebFilter`, agregada a la clase, indica al servidor en qué
solicitudes se debe llamar a este filter, según la URL de la solicitud.

En este proyecto se utiliza otra forma de implementar un filter, utilizando los
recursos de Spring que facilitan su implementación.
#### Importante!

En la versión final `3.0.0` de Spring Boot se realizó un cambio en Spring Security,
en cuanto a códigos que restringen el control de acceso. A lo largo de las clases,
el método `securityFilterChain(HttpSecurity http)`, declarado en la clase
`SecurityConfigurations`, tenía la siguiente estructura:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) 
  throws Exception {
    return http.csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and().authorizeRequests()
            .antMatchers(HttpMethod.POST, "/login").permitAll()
            .anyRequest().authenticated()
            .and().build();
}
```
Sin embargo, desde la versión final `3.0.0` de Spring Boot, el método
`authorizeRequests()` ha quedado obsoleto y debe ser reemplazado por el nuevo
método `authorizeHttpRequests()`. Asimismo, el método `antMatchers(`) debería
ser reemplazado por el nuevo método `requestMatchers()`:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http.csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and().authorizeHttpRequests()
            .requestMatchers(HttpMethod.POST, "/login").permitAll()
            .anyRequest().authenticated()
            .and().build();
}
```

## Control de acceso

```java
@Component
public class SecurityFilter extends OncePerRequestFilter {

    @Autowired
    private TokenService tokenService;
    @Autowired
    private UsuarioRepository usuarioRepository;

    @Override
    protected void doFilterInternal(
                        HttpServletRequest request,
                        HttpServletResponse response,
                        FilterChain filterChain)
                        throws ServletException, IOException {
        var authHeader = request.getHeader("Authorization");
        if (authHeader != null) {
            var token = authHeader.replace("Bearer ", "");
            var subject = tokenService.getSubject(token);
            if (subject != null) {
                // token válido
                var usuario = usuarioRepository.findByLogin(subject);
                var authentication = new UsernamePasswordAuthenticationToken(
                        usuario,
                        null,
                        usuario.getAuthorities() // forzar inicio de sesión
                );
                SecurityContextHolder
                .getContext().setAuthentication(authentication);
            }
        }
        filterChain.doFilter(request, response);
   }

}
```


```java
@Configuration
@EnableWebSecurity
public class SecurityConfigurations {

    @Autowired
    private SecurityFilter securityFilter;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity)
      throws Exception {
        return httpSecurity.csrf().disable().sessionManagement()
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and().authorizeHttpRequests()
                .requestMatchers(HttpMethod.POST, "/login")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .addFilterBefore(securityFilter, UsernamePasswordAuthenticationFilter.class)
                .build();
    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authenticationConfiguration)
      throws  Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public PasswordEncoder passwordEncoder () {
        return new BCryptPasswordEncoder();
    }

}
```


```java
@Service
public class TokenService {

    @Value("${api.security.secret}")
    private String apiSecret;

    public String generarToken(Usuario usuario) {
        try {
            Algorithm algorithm = Algorithm.HMAC256(apiSecret) ;
            return JWT.create()
                    .withIssuer("voll med")
                    .withSubject(usuario.getLogin())
                    .withClaim("id", usuario.getId())
                    .withExpiresAt(generarFechaExpiracion())
                    .sign(algorithm);
        } catch (JWTCreationException exception){
            throw new RuntimeException();
        }
    }

    public String getSubject(String token) {
        if (token == null) {
            throw new RuntimeException("token nulo");
        }
        DecodedJWT verifier = null;
        try {
            Algorithm algorithm = Algorithm.HMAC256(apiSecret);
            verifier = JWT.require(algorithm)
                    // specify an specific claim validations
                    .withIssuer("voll med")
                    // reusable verifier instance
                    .build()
                    .verify(token);
            verifier.getSubject();
        } catch (JWTVerificationException exception) {
            // Invalid signature/claims
            System.out.println(exception.toString());
        }
        if (verifier.getSubject() == null) {
            throw new RuntimeException("Verifier inválido");
        }
        return verifier.getSubject();
    }

    private Instant generarFechaExpiracion() {
        return LocalDateTime.now().plusHours(2).toInstant(ZoneOffset.of("-03:00"));
    }
}
```

En este proyecto no se tienen diferentes perfiles de acceso para los usuarios.
Sin embargo, esta característica se usa en algunas aplicaciones y se puede
configurar **Spring Security** que solo los usuarios que tienen un perfil
específico pueden acceder a ciertas URL.

P.e., suponiendo que en la aplicación tiene un perfil de acceso llamado
**ADMIN**, y solo los usuarios con ese perfil pueden eliminar médicos y
pacientes. Podemos indicar dicha configuración a **Spring Security**
cambiando el método `securityFilterChain`, en la clase
`SecurityConfigurations`, de la siguiente manera:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http)
  throws Exception {
    return http.csrf().disable()
        .sessionManagement()
        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        .and().authorizeRequests()
        .antMatchers(HttpMethod.POST, "/login").permitAll()
        .antMatchers(HttpMethod.DELETE, "/medicos").hasRole("ADMIN")
        .antMatchers(HttpMethod.DELETE, "/pacientes").hasRole("ADMIN")
        .anyRequest().authenticated()
        .and().addFilterBefore(
            securityFilter,
            UsernamePasswordAuthenticationFilter.class
            )
        .build();
}
```

Tener en cuenta que se agregaron dos líneas al código anterior, indicando a
**Spring Security** que las solicitudes de tipo **DELETE** de las URL `/médicos`
y `/pacientes` solo pueden ser ejecutadas por usuarios autenticados y cuyo perfil
de acceso es **ADMIN**.

### Control de acceso a anotaciones

Otra forma de restringir el acceso a ciertas funciones, según el perfil del
usuario, es usar una función de Spring Security conocida como Method Security,
que funciona con el uso de anotaciones en los métodos:

```java
@GetMapping("/{id}")
@Secured("ROLE_ADMIN")
public ResponseEntity detallar(@PathVariable Long id) {
    var medico = repository.getReferenceById(id);
    return ResponseEntity.ok(new DatosDetalladoMedico(medico));
}
```

En el ejemplo de código anterior, el método se anotó con
`@Secured("ROLE_ADMIN")`, de modo que sólo los usuarios con el rol **ADMIN**
pueden activar solicitudes para detallar a un médico. La anotación `@Secured`
se puede agregar en métodos individuales o incluso en la clase, lo que sería
el equivalente a agregarla en todos los métodos.

***¡Atención!*** Por defecto esta característica está deshabilitada en
**Spring Security**, y para usarla debemos agregar la siguiente anotación en la
clase `Securityconfigurations` del proyecto:

```java
@EnableMethodSecurity(securedEnabled = true)
```

Más detalles sobre la función de seguridad del método en la
documentación de
[Spring Security](https://docs.spring.io/spring-security/reference/servlet/authorization/method-security.html)

---

Continua en
[Documentar, probar y preparar una API para su implementación](spring_boot_3.md)
