
Los stored procedures son programas estructurados como un lenguaje un poco más específico, que huye del estándard ANSI. Rcordemos que el estándar ANSI fue ideado para mantener a SQL con una cierta estructura definida y que se pudiesen realizar consultas. Pero el estándar ANSI por sí solo limitaba mucho este lenguaje SQL.

¿Por qué hay cambios en el delimitador para uno o más caracteres (como por ejemplo `$$`) cuando creamos un **Stored Procedure**?

Porque si continuamos empleando el punto y coma se presentará un conflicto entre entre el punto y coma del Stored Procedure y el de los comandos para crearlo.

¡Alternativa correcta! Es esto precisamente. Tenemos que cambiar el delimitador para que no se presenten conflictos durante la ejecución de los comandos del **Stored Procedure**.

Vamos a ver un poco más sobre la sintaxis para nombrar nuestros Stored Procedures. Ellos deben tener letras y números, y podemos usar el signo de moneda y también el underscore, para nombrarlos, el tamaño máximo para un Stored procedure. Para el nombre de un stored procedure es de 64 caracteres no puedo tener más de 64 caracteres.

Y este nombre debe ser único y es case sensitive, aquí sí debemos tener mucho cuidado de no ir a mezclar, por ejemplo, mayúsculas con minúsculas y al momento de llamar a este procedimiento, escribirlo, por ejemplo, solo en mayúsculas o escribirlo solo en minúsculas, porque no va a funcionar. De la forma como lo vamos a nombrar, así lo tenemos que llamar.

```sql

CREATEPROCEDURE `desafio_1`()

BEGINDECLARE ClienteVARCHAR(10);

DECLARE EdadINT;

DECLARE Fecha_NacimientoDATE;

DECLARE CostoFLOAT;

  

SET Cliente = 'Juan';

SET Edad = 10;

SET Fecha_Nacimiento = '20170110';

SET Costo = 10.23;

  

SELECT Cliente;

SELECT Edad;

SELECT Fecha_Nacimiento;

SELECT Costo;

  

END

```

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1) Abre un nuevo script MySQL.

2) Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `manipulacion`()

BEGININSERTINTO tabla_de_productos (CODIGO_DEL_PRODUCTO,NOMBRE_DEL_PRODUCTO,SABOR,TAMANO,ENVASE,PRECIO_DE_LISTA)

VALUES ('1001001','Sabor Alpino','Mango','700 ml','Botella',7.50),

         ('1001000','Sabor Alpino','Melón','700 ml','Botella',7.50),

         ('1001002','Sabor Alpino','Guanábana','700 ml','Botella',7.50),

         ('1001003','Sabor Alpino','Mandarina','700 ml','Botella',7.50),

         ('1001004','Sabor Alpino','Banana','700 ml','Botella',7.50),

         ('1001005','Sabor Alpino','Asaí','700 ml','Botella',7.50),

         ('1001006','Sabor Alpino','Mango','1 Litro','Botella',7.50),

         ('1001007','Sabor Alpino','Melón','1 Litro','Botella',7.50),

         ('1001008','Sabor Alpino','Guanábana','1 Litro','Botella',7.50),

         ('1001009','Sabor Alpino','Mandarina','1 Litro','Botella',7.50),

         ('1001010','Sabor Alpino','Banana','1 Litro','Botella',7.50),

         ('1001011','Sabor Alpino','Asaí','1 Litro','Botella',7.50);

  

SELECT *FROM tabla_de_productosWHERE NOMBRE_DEL_PRODUCTOLIKE 'Sabor Alp%';

  

UPDATE tabla_de_productosSET PRECIO_DE_LISTA= 5WHERE NOMBRE_DEL_PRODUCTOLIKE 'Sabor Alp%';

  

SELECT *FROM tabla_de_productosWHERE NOMBRE_DEL_PRODUCTOLIKE 'Sabor Alp%';

  

DELETEFROM tabla_de_productosWHERE NOMBRE_DEL_PRODUCTOLIKE 'Sabor Alp%';

  

SELECT *FROM tabla_de_productosWHERE NOMBRE_DEL_PRODUCTOLIKE 'Sabor Alp%';

END$$

DELIMITER ;COPIA EL CÓDIGO

```

3) Ejecuta:

```objectivec

CALL manipulacion;COPIA EL CÓDIGO

```

4) El SP anterior manipula una serie de comandos en la base de datos de MySQL.

Observación: Si se presenta el error 1175, hay que realizar una pequeña modificación en la configuración de MySQL. Dirígete hacia menu, y en EDIT/PREFERENCES , desmarca al final del formulario la opción **Safe Updates (Reject UPDATEs and DELETEs with no restrictions)**:


![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/16.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/16.png)

5) Podemos también usar el SP para insertar datos en la tabla usando variables. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `incluir_producto`()

BEGINDECLARE vcodigoVARCHAR(20)DEFAULT '3003001';

DECLARE vnombreVARCHAR(20)DEFAULT 'Sabor Intenso';

DECLARE vsaborVARCHAR(20)DEFAULT 'Tutti Frutti';

DECLARE vtamanoVARCHAR(20)DEFAULT '700 ml';

DECLARE venvaseVARCHAR(20)DEFAULT 'Botella PET';

DECLARE vprecioDECIMAL(4,2)DEFAULT 7.25;

INSERTINTO tabla_de_productos (CODIGO_DEL_PRODUCTO,NOMBRE_DEL_PRODUCTO,SABOR,TAMANO,ENVASE,PRECIO_DE_LISTA)

VALUES (vcodigo, vnombre, vsabor, vtamano, venvase, vprecio);

END$$

DELIMITER ;COPIA EL CÓDIGO

```
  
6) Ejecuta:

```objectivec

CALL incluir_producto;COPIA EL CÓDIGO

```

7) Se puede observar que el SP hizo referencia directamente a las variables en el comando INSERT, dentro del SP.

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/17.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/17.png)

8) Podemos usar parámetros para ingresar al SP los datos que serán usados en el comando de INSERT. Para ello digita y ejecuta:

```scss

DELIMITER $$

CREATE PROCEDURE `incluir_producto_parametros`(vcodigo VARCHAR(20), vnombre VARCHAR(20),vsabor VARCHAR(20),

vtamano VARCHAR(20), venvase VARCHAR(20), vprecio DECIMAL(4,2))

BEGIN

DECLARE mensaje VARCHAR(40);

INSERT INTO tabla_de_productos (CODIGO_DEL_PRODUCTO,NOMBRE_DEL_PRODUCTO,SABOR,TAMANO,ENVASE,PRECIO_DE_LISTA)

     VALUES (vcodigo, vnombre, vsabor, vtamano, venvase, vprecio);

END$$

DELIMITER ;COPIA EL CÓDIGO

```

9) Ejecuta el SP:

```csharp

CALLincluir_producto_parametros('1000800','Sabor del Mar',

'Naranja', '700 ml', 'Botella de Vidrio', 2.25);COPIA EL CÓDIGO

```

10) El comando anterior ingresa los datos que serán incluidos dentro de la tabla a través de los parámetros.

11) Repite la ejecución del mismo comando:

```csharp

CALLincluir_producto_parametros('1000800','Sabor del Mar',

'Naranja', '700 ml', 'Botella de Vidrio', 2.25);COPIA EL CÓDIGO

```

12) Observa que hubo un error de clave primaria porque el producto 1000800 ya existe en la tabla.

13) Podemos tratar este error para que MySQL presente un mensaje amigable cuando ocurra duplicidad en los registros. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `incluir_producto_parametros`(vcodigoVARCHAR(20), vnombreVARCHAR(20),vsaborVARCHAR(20),

vtamanoVARCHAR(20), venvaseVARCHAR(20), vprecioDECIMAL(4,2))

BEGINDECLARE mensajeVARCHAR(40);

DECLARE EXIT HANDLERFOR 1062

BEGINSET mensaje = 'Producto duplicado! ';

SELECT mensaje;

END;

INSERTINTO tabla_de_productos (CODIGO_DEL_PRODUCTO,NOMBRE_DEL_PRODUCTO,SABOR,TAMANO,ENVASE,PRECIO_DE_LISTA)

VALUES (vcodigo, vnombre, vsabor, vtamano, venvase, vprecio);

SET mensaje = 'Producto incluido con éxito!';

SELECT mensaje;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

14) Digita y ejecuta:

```csharp

CALLincluir_producto_parametros('1000801','Sabor del Mar',

'Naranja', '700 ml', 'Botella de Vidrio', 3.25);COPIA EL CÓDIGO

```

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/18.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/18.png)

15) Si repites el mismo comando, entonces, no aparecerá el error en el *output*, pero si obtendrás el siguiente mensaje:

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/19.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/19.png)

16) Vimos que el comando `SET` es utilizado para atribuir valores a las variables. Pero, podemos atribuir valores a las mismas usando el comando `SELECT` a través de la cláusula `SELECT INTO`. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `mostrar_sabor`(vcodigoVARCHAR(15))

BEGINDECLARE vsaborVARCHAR(20);

SELECT SABORINTO vsaborFROM tabla_de_productosWHERE CODIGO_DEL_PRODUCTO = vcodigo;

SELECT vsabor;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

17) Ejecuta:

```csharp

CALLmostrar_sabor('1101035');COPIA EL CÓDIGO

```

18) Aparecerá como respuesta el sabor del producto ingresado como parámetro.

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/20.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/03/20.png)


Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1) Abre un nuevo script MySQL.

2) Digita e ejecuta:

```sql

DELIMITER $$

CREATE ROCEDURE `edad_clientes`(vdniVARCHAR(20))

BEGINDECLARE vresultadoVARCHAR(50);

DECLARE vfechaDATE;

SELECT FECHA_DE_NACIMIENTOINTO vfechaFROM tabla_de_clientesWHERE DNI = vdni;

IF

vfecha < '19950101'

THENSET vresultado = 'Cliente Viejo.';

ELSESET vresultado = 'Cliente Joven.';

END IF;

SELECT vresultado;

END$$

DELIMITER ;

  

CALL edad_clientes('50534475787');COPIA EL CÓDIGO

```


![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/21.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/21.png)


3) El SP anterior hace uso de la estructura IF-THEN-ELSE para clasificar a un cliente como joven o viejo, basado en su edad.

4) Vamos a ver otra estructura de control de flujo. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `precio_producto`(vcodigoVARCHAR(20))

BEGINDECLARE vresultadoVARCHAR(40);

DECLARE vprecioFLOAT;

SELECT PRECIO_DE_LISTAINTO vprecioFROM tabla_de_productos

WHERE CODIGO_DEL_PRODUCTO = vcodigo;

IF vprecio >=12

THENSET vresultado = 'Producto costoso.';

ELSEIF

vprecio >= 7AND vprecio < 12

THENSET vresultado = 'Producto asequible.';

ELSESET vresultado = 'Producto barato.';

END IF;

SELECT vresultado;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

5) Digita y ejecuta:

```csharp

CALLprecio_producto('1000801');COPIA EL CÓDIGO

```

¿Cómo fue clasificado este precio?

6) Observa que la estructura IF-THEN-ELSEIF permite encadenar diversas pruebas.

7) El encadenamiento de condiciones puede ser expresado, también, usando el comando CASE-END-CASE. Para ello, digita e ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `define_sabor`(vcodigoVARCHAR(20))

BEGINDECLARE vsaborVARCHAR(20);

SELECT SABORINTO vsaborFROM tabla_de_productosWHERE CODIGO_DEL_PRODUCTO = vcodigo;

CASE vsabor

WHEN 'Maracuyá'THENSELECT 'Muy Rico';

WHEN 'Limón'THENSELECT 'Muy Rico';

WHEN 'Frutilla'THENSELECT 'Muy Rico';

WHEN 'Uva'THENSELECT 'Muy Rico';

WHEN 'Sandía'THENSELECT 'Normal';

WHEN 'Mango'THENSELECT 'Normal';

ELSESELECT 'Jugos comunes';

ENDCASE;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

8) Digita y ejecuta:

```csharp

CALLdefine_sabor('1096818');COPIA EL CÓDIGO

```

¿Estás de acuerdo con el resultado de este sabor?

9) Una estructura derivada de la anterior es el CASE-NOT-FOUND. Para ello, vamos a crear otro SP que llamaremos `define_sabor_con_error`. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `define_sabor_con_error`(vcodigoVARCHAR(20))

BEGINDECLARE vsaborVARCHAR(20);

SELECT SABORINTO vsaborFROM tabla_de_productosWHERE CODIGO_DEL_PRODUCTO = vcodigo;

CASE vsabor

WHEN 'Maracuyá'THENSELECT 'Muy Rico';

WHEN 'Limón'THENSELECT 'Muy Rico';

WHEN 'Frutilla'THENSELECT 'Muy Rico';

WHEN 'Uva'THENSELECT 'Muy Rico';

WHEN 'Sandía'THENSELECT 'Normal';

WHEN 'Mango'THENSELECT 'Normal';

ENDCASE;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

10) Este SP difiere del anterior tan solo porque fue retirada la siguiente línea de código:

```sql

ELSESELECT 'Jugos comunes';COPIA EL CÓDIGO

```


Estamos forzando una situación donde ninguna de las condiciones pueden ser satisfechas.

11) Ejecuta el SP:

```csharp

CALLdefine_sabor_con_error('1002334');COPIA EL CÓDIGO

```


12) Obtendremos el siguiente error:

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/24.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/24.png)

13) Para evitar que no queden contempladas todas las situaciones al interior del comando CASE, podemos añadir tratamiento de errores. Altera el SP ejecutando:

```sql

DELIMITER $$

CREATEPROCEDURE `define_sabor_con_error`(vcodigoVARCHAR(20))

BEGINDECLARE vsaborVARCHAR(20);

DECLARE mensajeerrorVARCHAR(50);

DECLARE CONTINUE HANDLERFOR 1339

SET mensajeerror = 'Sabor no definido en ningún caso.';

SELECT SABORINTO vsaborFROM tabla_de_productosWHERE CODIGO_DEL_PRODUCTO = vcodigo;

CASE vsabor

WHEN 'Maracuyá'THENSELECT 'Muy Rico';

WHEN 'Limón'THENSELECT 'Muy Rico';

WHEN 'Frutilla'THENSELECT 'Muy Rico';

WHEN 'Uva'THENSELECT 'Muy Rico';

WHEN 'Sandía'THENSELECT 'Normal';

WHEN 'Mango'THENSELECT 'Normal';

ENDCASE;

SELECT mensajeerror;

END$$

DELIMITERCOPIA EL CÓDIGO

```

14) Ejecuta el SP:

```csharp

CALLdefine_sabor_con_error('1000800');COPIA EL CÓDIGO

```

El error ha sido tratado con éxito, y nos muestra un mensaje amigable:

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/25.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/25.png)

15) El CASE Condicional utiliza una estructura de CASE semejante a la empleada cuando ejecutamos un comando SELECT. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `precio_producto_case`(vcodigoVARCHAR(20))

BEGINDECLARE vresultadoVARCHAR(40);

DECLARE vprecioFLOAT;

SELECT PRECIO_DE_LISTAINTO vprecioFROM tabla_de_productos

WHERE CODIGO_DEL_PRODUCTO = vcodigo;

CASEWHEN vprecio >=12THENSET vresultado = 'Producto costoso.';

WHEN vprecio >= 7AND vprecio < 12THENSET vresultado = 'Producto asequible.';

WHEN vprecio < 7THENSET vresultado = 'Producto barato.';

ENDCASE;

SELECT vresultado;

END $$

DELIMITER ;COPIA EL CÓDIGO

```

16) Digita y ejecuta:

```csharp

CALLprecio_producto_case('1000801');COPIA EL CÓDIGO

```

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/26.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/26.png)

17) La estructura de Loop permite repetir un conjunto de comandos hasta que determinada condición se cumpla. Digita y ejecuta:

```sql

CREATETABLE tb_looping (IDINT);COPIA EL CÓDIGO

```

18) Luego de crear la anterior tabla auxiliar, digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `looping`(vinicialINT, vfinalINT)

BEGINDECLARE vcontadorINT;

DELETEFROM tb_looping;

SET vcontador = vinicial;

WHILE vcontador <= vfinal

DO

INSERTINTO tb_loopingVALUES(vcontador);

SET vcontador = vcontador+1;

END WHILE;

SELECT *FROM tb_looping;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

19) Vamos a ejecutar el SP ingresando como parámetros un número inicial y un número final para la creación de una secuencia numérica en la tabla auxiliar. Digita y ejecuta:

```scss

CALL looping(1,10);COPIA EL CÓDIGO

```

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/27.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/04/27.png)

Obtenemos como resultado una secuencia numérica que va de 1 a 10.

Un stored procedure es realmente una subrutina que ejecuta una serie de comandos y listo, siempre está ejecutando unos comandos. Pero la función como tal ella no solamente ejecuta una serie de comandos, sino que siempre ella va a retornar un valor o un resultado, entonces esa es como la principal diferencia entre una subrutina y una función.

La subrutina ejecuta varios comandos. Pero la función los ejecuta y nos devuelve un resultado. Entonces en funciones, vamos a ver aquí rápidamente la sintaxis para yo crear una función utilizando MySQL, entonces uso las siguientes cláusulas: CREATE FUNCTION, el nombre de la función, los parámetros que le vamos a introducir a la función.

Y después vamos a definir o vamos a decir que ella devuelve RETURNS un tipo de datos. Aquí, entonces puede ser un INTEGER, VARCHAR, DATE, lo que queramos, el tipo de datos que queremos que devuelva esta función. Y después viene BEGIN y END, al igual que un stored procedure, como los que ya hemos venido trabajando.

Pero a diferencia del stored procedure, aquí va a tener obligatoriamente la cláusula RETURN que nos va a devolver un valor o un resultado de acuerdo con el datatype, que fue definido. Entonces ahora vamos a, antes de partir a Workbench, hay un parámetro que debemos establecer en 1, que es el log_bing_trust_function_creators.

SET GLOBAL log_bing_trust_function_creators =1;

```sql

SET GLOBAL log_bing_trust_function_creators =1;

```

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1) Cuando el resultado de un `SELECT` posee más de una línea no podemos atribuirlo a una variable usando el `SELECT INTO`. Para ello, tenemos que usar una estructura llamada Cursor para recibir los valores provenientes de una tabla, con una o más columnas.

2) Vamos a crear un SP que utilice un Cursor. Digita y ejecuta:

```sql

DELIMITER $$

CREATEPROCEDURE `cursor_1`()

BEGINDECLARE vnombreVARCHAR(50);

DECLARE cCURSORFORSELECT NOMBREFROM tabla_de_clientes LIMIT 4;

OPEN c;

FETCH cINTO vnombre;

SELECT vnombre;

FETCH cINTO vnombre;

SELECT vnombre;

FETCH cINTO vnombre;

SELECT vnombre;

FETCH cINTO vnombre;

SELECT vnombre;

FETCH cINTO vnombre;

SELECT vnombre;

CLOSE c;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

3) Nota que cada línea de la selección es atribuida a la variable vnombre, una línea a la vez. Ejecuta:

```objectivec

CALL cursor_1;COPIA EL CÓDIGO

```

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/28.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/28.png)

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/29.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/29.png)

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/30.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/30.png)

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/31.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/31.png)

4) En el anterior SP ejecutamos un Cursor controlado donde sabíamos cuántos elementos el mismo contenía. Pero, normalmente, no sabemos esta información. Por ello usamos siempre el Cursor combinado con un Looping. Digita y ejecuta la creación del siguiente SP:

```sql

DELIMITER $$

CREATEPROCEDURE `cursor_looping`()

BEGINDECLARE fin_cINTDEFAULT 0;

DECLARE vnombreVARCHAR(50);

DECLARE cCURSORFORSELECT NOMBREFROM tabla_de_clientes;

DECLARE CONTINUE HANDLERFORNOT FOUND

SET fin_c = 1;

OPEN c;

WHILE fin_c = 0

DO

FETCH cINTO vnombre;

IF fin_c = 0

THENSELECT vnombre;

END IF;

END WHILE;

CLOSE c;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

5) Ejecuta el SP:

```objectivec

CALL cursor_looping;COPIA EL CÓDIGO

```

Obtendremos diversos resultados en diferentes consultas.
  
![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/32.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/32.png)

6) El Cursor puede recibir más de un campo. Crea el siguiente SP:

```sql

DELIMITER $$

CREATEPROCEDURE `cursor_looping_varios_campos`()

BEGINDECLARE fin_cINTDEFAULT 0;

DECLARE vbarrio, vciudad, vestado, vcpVARCHAR(50);

DECLARE vnombre, vdireccionVARCHAR(150);

DECLARE cCURSORFORSELECT NOMBRE, DIRECCION_1, BARRIO, CIUDAD, ESTADO, CPFROM tabla_de_clientes;

DECLARE CONTINUE HANDLERFORNOT FOUND

SET fin_c = 1;

OPEN c;

WHILE fin_c = 0

DO

FETCH cINTO vnombre, vdireccion, vbarrio, vciudad, vestado, vcp;

IF fin_c = 0

THENSELECT CONCAT(vnombre, ' Dirección: ', vdireccion, " - ", vbarrio, ' - ', vciudad, ' - ', vestado, ' - ',vcp)AS RESULTADO;

END IF;

END WHILE;

CLOSE c;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

7) Ejecútalo:  

```objectivec

CALL cursor_looping_varios_campos;COPIA EL CÓDIGO

```

Este SP también devuelve múltiples consultas.

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/33.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/33.png)

8) También, podemos crear una función. La diferencia entre una función y un SP es que la función retorna un valor y puede ser usado dentro de un comando SELECT, INSERT, UPDATE y condiciones de DELETE.

9) Para crear una función con el botón derecho del mouse, haz clic sobre **Function** y selecciona **Create Function**:

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/34.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/34.png)

10) Digita y ejecuta el siguiente código:

```sql

DELIMITER $$

CREATEFUNCTION `f_define_sabor`(vsaborVARCHAR(40))RETURNSvarchar(40) CHARSET utf8mb4

BEGINDECLARE vretornoVARCHAR(40)DEFAULT "";

CASE vsabor

WHEN 'Maracuyá'THENSET vretorno = 'Muy Rico';

WHEN 'Limón'THENSET vretorno = 'Muy Rico';

WHEN 'Frutilla'THENSET vretorno = 'Muy Rico';

WHEN 'Uva'THENSET vretorno = 'Muy Rico';

WHEN 'Sandía'THENSET vretorno = 'Normal';

WHEN 'Mango'THENSET vretorno = 'Normal';

ELSESET vretorno = 'Jugos comunes';

ENDCASE;

RETURN vretorno;

END$$

DELIMITER ;COPIA EL CÓDIGO

```

11) Si te aparece el siguiente error:

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/35.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/35.png)

12) Es porque MySQL no permite la construcción de Funciones por defecto. Para permitir la creación de funciones, ejecuta el siguiente comando:

```sql

SETGLOBAL log_bin_trust_function_creators = 1;COPIA EL CÓDIGO

```

Y nuevamente crea la función.

13) Ejecuta la función:

```csharp

SELECTf_define_sabor('Maracuyá');COPIA EL CÓDIGO

```

![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/36.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/36.png)

14) Podemos usar la función en un comando SELECT (Al igual que con cualquier otra función de MySQL). Digita y ejecuta:

```vbnet

SELECT NOMBRE_DEL_PRODUCTO, SABOR, f_define_sabor(SABOR)AS TIPO

FROM tabla_de_productos;COPIA EL CÓDIGO

```


![https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/37.png](https://caelum-online-public.s3.amazonaws.com/1833-esp-procedures-sql/05/37.png)