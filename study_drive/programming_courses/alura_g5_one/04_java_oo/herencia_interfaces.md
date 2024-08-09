# Java [Polimorfismo, Entendiendo Herencia e Interfaces](https://app.aluracursos.com/course/java-parte-3-entendiendo-herencia-interfaces)

## Herencia

Extendiendo la funcionalidad de la clase **Funcionario** a la nueva clase **Gerente**,
heradando métodos y atributos de la case padre (Funcionario), utilzando la palabra
reservada `extends`. Reutilización de código.

Palabras reservadas `this` y `super`.

- **this**: este *keyword* se refiere al objeto catuan en un método o
constructor. El uso más común de este término es eliminnar la ambiguedad entre
los atributos de clase y los parámetros con el mismo nombre. Además sirve para:
  - Invocar al constructor de la clase actual.
  - Invocar al método de clase actual.
  - Devolver el objeto de la clase actual.
  - Pasar un argumento en la llamada a un método.
  - Pasar un arg. en la llamada al constructor.
- **super**: este *keyword* se refiere a objetos de la superclase (madre). Se
utiliza para llamar a los métodos de la superclase y para acceder al constructor
de esta. Su función más común es eliminar la ambiguedad entre superclases y
subclases que tienen métodos con el mismo nombre. Utilizada para:
  - `super`:
    - Referirse a la variable de instancia de la clase inmediatamente superior.
    - Invocar métodos de la clase inmediatamente superior (clase madre).
  - `super()`:
    - Invocar al constructor de la clase inmediatamente superior (clase madre).

Java doc, [this](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
y [super](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)

### Modificadores de acceso

Los modificadores de acceso o accesibilidad son algunas palabras claves utilizadas
en el lenguaje Java para definir el nivel de accesibilidad que los elementos de
una clase (atributos y métodos) e incluso la propia clase puede tener los mismos
elementos de otra clase.

- **Public**: Este es el modificador menos restrictivo de todos. De esta manera,
cualquier componente puede acceder a los miembros de la clase, las clases y las
interfaces.

- **Protected**: Al usar este modificador de acceso, los miembros de la clase y
las clases son accesibles para otros elementos siempre que están dentro del
mismo package o, si pertenecen a otros packages, siempre que tengan una relación
extendida (herencia), es decir, las clases secundarias pueden acceder a los
miembros de su clase principal (o clase de abuelos, etc.).

- **Private**: Este es el modificador de acceso más restrictivo de todos.
Solo se puede acceder a los miembros definidos como privados desde dentro de la
clase y desde ningún otro lugar, independientemente del paquete o la herencia.

Java doc
[modificadores de acceso](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

### Sobrecarga de metodo

```java
    public boolean autenticar(int password) {
        if (this.password == contraseña) {
            return true;
        } else {
            return false;
        }
    }

    // Nuevo método, recibiendo dos parámetros
    public boolean autenticar(String login, int password) {
        ...
    }
```

Se ha creado una nueva versión del método autenticar. Ahora tenemos dos métodos
de autenticar en la misma clase que varían en el número o tipo de parámetros.
Esto se llama sobrecarga de métodos.

La sobrecarga no tiene en cuenta la visibilidad o retorno del método, solo los
parámetros y no depende de la herencia.

Notas:

- La clase madre es llamada de super o base class.
- La clase hija también es llamada de sub class.
- Aumentar la visibilidad de un miembro (atributo, método) a través de protected.
- Acceder o llamar un miembro (atributo, método) a través de super.

## Entendiendo Polimorfismo

Clase Madre

```java
public class Vehiculo {
    public void encender() {
        System.out.println("Enciendiendo vehículo");
    }
}
```

Clases Hijas

```java
public class Automovil extends Vehiculo {
    public void encender() {
        System.out.println("Enciendiendo automovil");
    }
}
```

```java
public class Motocicleta extends Vehiculo {
    public void encender() {
        System.out.println("Enciendiendo motocicleta");
    }
}
```

Main

```java
public class TestVehiculos {
    public static void main(String[] args) {
        Vehiculo m = new Motocicleta();
        m.encender();
        Vehiculo c = new Automovil();
        c.encender();
    }
}
```

Salida

```txt
Enciendiendo motocicleta
Enciendiendo automovil
```

Notas

- Los objetos no cambian de tipo.
- La referencia puede cambiar, y ahí es donde entra el polimorfismo.
- El polimorfismo permite utilizar referencias más genéricas para comunicarse
con un objeto.
- El uso de referencias más genéricas permite **desacoplar** sistemas.
- Uso de la anotación `@Override`.
- Los constructores no se heredan.
- Se puede llamar a un constructor de clase madre mediante `super()`.

## Clases abstractas e interfaces

| [Interfáz](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html) | [Clase Abstracta](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html) |
| - | - |
| *Keyword* `implements` | *Keyword* `extends` |
| Admite herencia múltiple | No admite herencia múltiple |
| Proporciona una abstracción absoluta y no puede tener implementaciones de métodos | Puede tener métodos con implementaciones |
| No posee constructor | Posee constructor |
| No puede tener modificador de acceso, todos los accesos con públicos | Puede tener modificadores de acceso |
| Sus miembros **no** pueden ser `static` | Solo los miembros completamenten abstractos pueden ser `static` |

No hay herencia múltiple en Java, Las interfaces son una alternativa a la
herencia con respecto al polimorfismo.

Java [util](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/package-summary.html)

#### Ejemplos de clases abstractas e interfaces

Clase **Abstracta**
Funcionario

```java
public abstract class Funcionario {

    private String nombre;
    private String documento;
    private Double salario;
    private int tipo;

    public Funcionario() {
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getDocumento() {
        return documento;
    }

    public void setDocumento(String documento) {
        this.documento = documento;
    }

    public Double getSalario() {
        return salario;
    }

    public void setSalario(Double salario) {
        this.salario = salario;
    }

    public int getTipo() {
        return tipo;
    }

    public void setTipo(int tipo) {
        this.tipo = tipo;
    }

    public abstract double getBonificacion(); 

}
```

Implementada por la **clase**
Gerente

```java
public class Gerente extends Funcionario implements Autenticable {
    
    private AutenticacionUtil util;
    
    public Gerente() {
        this.util = new AutenticacionUtil();
    }
    
    public double getBonificacion() {
        return super.getSalario() + (this.getSalario() * 0.5);
    }
    
    @Override
    public void setClave(String clave) {
        this.util.setClave(clave);
    }

    @Override
    public boolean inicioSesion(String clave) {
        return this.util.inicioSesion(clave);
    }
}
```

Que implementa la **Interfáz**
Autenticable

```java
public interface Autenticable {
    
    public void setClave(String clave);
    
    public boolean inicioSesion(String clave);
    
}
```

Utilización de clase **utilitaria**
AutenticacionUtil

```java
public class AutenticacionUtil {
    
    private String clave;
    
    public boolean inicioSesion(String clave) {
        return this.clave == clave;
    }
    
    public void setClave(String clave) {
        this.clave = clave;
    }

}
```
