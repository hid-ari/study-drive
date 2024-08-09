# Enums

Tipo de datos especiales donde una variable puede contener el valor de un grupo
de constantes predefinidas.

- Java [enum](https://www.youtube.com/watch?v=EoPvlE85XAQ)

```java
public enum Dia {
    LUNES, MARTES, MIERCOLES, JUEVES, VIERNES, SABADO, DOMINGO
}
```

```java
public class Principal {

    public static void main(String[] args) {
        for (Dia dia : Dia.values()) {
            System.out.println("El dia de la semana es: "+dia);
        }
        Dia domingo = Dia.DOMINGO;
        System.out.println(domingo.name());
        System.out.println(domingo.ordinal());
        System.out.println(domingo.toString());
    }
}
```

# Anotaciones

Alura [blog](https://www.aluracursos.com/blog/crear-anotaciones-en-java)

Clase Usuario

```java
import java.time.LocalDate;
import java.time.Period;

public class Usuario {

    private String nombre;
    private String identidad;
    @EdadMinima(valor=10)
    private LocalDate fechaNacimiento;

    public Usuario(String nombre, String identidad, LocalDate fechaNacimiento) {
        this.nombre = nombre;
        this.identidad = identidad;
        this.fechaNacimiento = fechaNacimiento;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getIdentidad() {
        return identidad;
    }

    public void setIdentidad(String identidad) {
        this.identidad = identidad;
    }

    public LocalDate getFechaNacimiento() {
        return fechaNacimiento;
    }

    public void setFechaNacimiento(LocalDate fechaNacimiento) {
        this.fechaNacimiento = fechaNacimiento;
    }

    public static boolean usuarioValido(Usuario usuario) {
        if (!usuario.getNombre().matches("[a-zA-Záàâãéèêíïóôõöúçñ\\s]+")) {
            System.out.println("Fail RegexNombre");
            return false;
        }
        if (!usuario.getIdentidad().matches("[^0-9]+")) {
            System.out.println("Fail RegexIdentidad");
            return false;
        }
        return Period.between(usuario.getFechaNacimiento(), LocalDate.now()).getYears() >= 18;
    }

}
```

Clase Validador
```java
import java.lang.reflect.Field;
import java.time.LocalDate;
import java.time.Period;

public class Validador {
    public static <T> boolean validador(T objeto) {
        Class<?> clase = objeto.getClass();
        for (Field field : clase.getDeclaredFields()) {
            if (field.isAnnotationPresent(EdadMinima.class)) {
                EdadMinima edadMinima = field.getAnnotation(EdadMinima.class);
                try {
                    field.setAccessible(true);
                    LocalDate fechaNacimiento = (LocalDate) field.get(objeto);
                    return Period.between(fechaNacimiento, LocalDate.now()).getYears() >= edadMinima.valor();
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
        }
        return false;
    }
}
```

Anotacion EdadMinima
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface EdadMinima {
    public int valor();
}
```

Clase TestCrearUsuario

```java
import java.time.LocalDate;
import java.time.Month;

public class TestCrearUsuario {
    public static void main(String[] args) {
        Usuario usuario1 = new Usuario("Maria", "123456789", LocalDate.of(1995, Month.MARCH, 14));
        Usuario usuario2 = new Usuario("Mario", "987654321", LocalDate.of(2020, Month.MARCH, 14));
        //System.out.println(Usuario.usuarioValido(usuario1));
        System.out.println(Validador.validador(usuario1));
        //System.out.println(Usuario.usuarioValido(usuario2));
        System.out.println(Validador.validador(usuario2));

    }
}
```
