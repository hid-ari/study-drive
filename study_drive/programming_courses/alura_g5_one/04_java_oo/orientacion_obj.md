# Java Entendiendo la [Orientación a Objetos](https://app.aluracursos.com/course/java-parte2-introduccion-orientada-objetos)

Intro [POO](https://www.youtube.com/watch?v=Oigen2sjagk)

Diferencias con el paradigma **procedural**: se utilizó como práctica de
programación antes de la introducción de lenguajes orientados a objetos.
La necesidad de validar el numeroIdentidad en un formulario se utilizó como
ejemplo para discutir los principales problemas que pueden aparecer en este paradigma.

En particular, a medida que otros formularios y desarrolladores necesitan la misma
validación de numeroIdentidad, no fue fácil ver que ya había procedimientos y
funciones que hicieron este trabajo, ya que los datos y las funciones no tenían
un vínculo tan fuerte. Esto podría dar lugar a otra nueva función o fragmento de
código con una responsabilidad similar.

Además, conocemos la **idea central** del paradigma **orientado a objetos**, que
es **crear unidades de código que combinen los datos asociados con cierta
información con las funcionalidades aplicadas a esos datos** (por ejemplo,
numeroIdentidad + validación). Son los atributos y métodos.

## Introduccion a Orientación a Objetos

proyecto_bytebank

- Se practican los conceptos de **Objeto**, **Atributos**, **Instancias**. Y se
ejemplifica un atributo de clase con valor predeterminado.
Cuenta.java y CrearCuenta.java

  ```java
  // Entidad Cuenta
  public class Cuenta {
      double saldo;
      //int agencia = 33;
      int agencia;
      int numero;
      String titular;
  }
  ```

  ```java
  public class CrearCuenta {
      public static void main(String[] args) {
          Cuenta primeraCuenta = new Cuenta();
          primeraCuenta.saldo = 1000;
          //primeraCuenta.pais = "Zambia"; No compila, no existe el atributo
          System.out.println(primeraCuenta.saldo);
  
          Cuenta segundaCuenta = new Cuenta();
          segundaCuenta.saldo = 500;
          System.out.println(segundaCuenta.saldo);
          //System.out.println(segundaCuenta.titular); null
          //System.out.println(segundaCuenta.agencia); 0
      }
  }
  ```

- Se ejemplifica como un Objeto es una una referencia en memoria
testReferencia

  ```java
  public class TestReferencia {
      public static void main(String[] args) {
          Cuenta primeraCuenta = new Cuenta();
          primeraCuenta.saldo = 200;
  
          //Cuenta segundaCuenta = new Cuenta();
          // System.out.println(primeraCuenta);     // Cuenta@23fc625e
          // System.out.println(segundaCuenta);     // Cuenta@3f99bd52
  
          Cuenta segundaCuenta = primeraCuenta;
          segundaCuenta.saldo = 100;
          System.out.println("Saldo primera cuenta: " + primeraCuenta.saldo);
          System.out.println("Saldo segunda cuenta: " + segundaCuenta.saldo);
  
          segundaCuenta.saldo += 400;
          System.out.println("Saldo primera cuenta: " + primeraCuenta.saldo);
  
          // Obtener referencia del objeto
          System.out.println(primeraCuenta);        // Cuenta@23fc625e
          System.out.println(segundaCuenta);        // Cuenta@23fc625e
  
          if (primeraCuenta == segundaCuenta) {
              System.out.println("Son el mismo objeto");
          } else {
              System.out.println("Son diferentes");
          }
      }
  }
  ```

- Definición de metodos con retorno y uso de *this* para atributos de clase

  ```java
  // Entidad Cuenta
  public class Cuenta {
      
      ...
  
      public void depositar(double valorDeposito){
          this.saldo += valorDeposito;
      }
  
      public boolean retirar(double valorRetiro) {
          if (this.saldo >= valorRetiro) {
              this.saldo -= valorRetiro;
              return true;
          } else {
              return false;
          }
      }
  
      public boolean transferir(double montoTransferencia, Cuenta cuenta){
          if (montoTransferencia <= this.saldo) {
              this.saldo -= montoTransferencia;
              cuenta.depositar(montoTransferencia);
              return true;
          } else {
              return false;
          }
      }
  }
  ```
  
  ```java
  public class PruebaMetodos {
      public static void main(String[] args) {
          Cuenta cuenta1 = new Cuenta();
          cuenta1.depositar(300);
          System.out.println(cuenta1.retirar(200));
          System.out.println(cuenta1.saldo);
  
          Cuenta cuenta2 = new Cuenta();
          cuenta2.depositar(2000);
  
          boolean puedeTransferir = cuenta2.transferir(400, cuenta1);
          if (puedeTransferir) {
              System.out.println("Transferencia exitosa");
          }
      }
  }
  ```

- Ejemplo de `NullPointerException` al intentar acceder a un atributo de clase
de una instancia, cuyo valor es una Clase no instanciada.
TestReferencia3.java

- Encapsulamiento, modificadores de acceso
Cuenta.java y PruebaEncapsulamiento.java  
  Atributos privados, restringiendo el acceso a los atributos.

  Encapsulación de código:  
  - **getters** Métodos de lectura de atributos
  - **setters** Métodos para modificar atributos

- Variables de clase **static**. No pertenecen a la instancia.
  ```java
    public class Cuenta {
        ...
        private static int contador = 0;
        

        public static int getContador(){
            return Cuenta.contador;
        }
  ```

- **Constructor**  Cuenta.java
  Permite recibir argumentos e inicializar atributos desde la creación de un objeto.
  Con esto, la inicialización de los atributos recibidos en el constructor se vuelve
  obligatoria. Atributos de clase, atributos estáticos. Métodos de clase, métodos
  estáticos. Ausencia de referencia del **this** dentro de los métodos estáticos.

  ```java
    public Cuenta(int agencia) {
        if (agencia <= 0) {
            System.out.println("No se permiten valores negativos");
            this.agencia = 1;
        } else {
            this.agencia = agencia;
        }
        contador++;
        System.out.println("Cuentas creadas: "+contador);
    }
  ```


Lectura: Alura blog [¿Que es POO?](https://www.aluracursos.com/blog/poo-que-es-la-programacion-orientada-a-objetos)
