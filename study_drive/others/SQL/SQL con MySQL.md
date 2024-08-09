Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Si estás usando un nuevo equipo, debes instalar MySQL. Por ello, te invito a seguir las siguientes instrucciones:

**Instalación en WINDOWS**

- Accede a través de tu navegador y busca: MySQL Downloads. Allí, accederás al link: [http://www.mysql.com/downloads](http://www.mysql.com/downloads).
- Busca el link que dice: **MySQL Community Edition (GPL) / Community (GPL) downloads**.
- Dirígete hacia: **MySQL on Windows (Installer & Tools) / Downloads**.
- Haz clic en: **MySQL Installer**.
- Haz clic en el botón de download al lado de la opción: **Windows (x86-32 Bits), MSI Installer (mysql-installer-web-community-8.0.15.0.msi)**.
- Haz login en la página de Oracle. Si no tienes login regístrate.
- Luego de hacer Login haz clic en **Download Now**.
- Ejecuta el programa que fue descargado.
- Haz clic en: **I Accept the license terms** y luego en **Next**.
- Escoge la instalación: **Developer Default**. Haz clic en **Next** dos veces.
- Haz clic sobre **Execute** para hacer el download y la instalación de la base y sus componentes.
- Haz clic en **Next** dos veces.
- Mantén la opción **StandAlone MySQL Server / Classic MySQL Replication**.
- Mantén las propiedades por defecto del servicio y del gateway. Clic en **Next**.
- Mantén la opción **Use Strong Encryption for Authentication...** Clic en **Next**.
- Incluye la contraseña del usuario root dos veces. Clic en **Next**.
- Mantén las propiedades por defecto. Clic en **Next**.
- Clic en **Execute** para iniciar la instalación.

Siempre selecciona **Next** y **Finish** a medida que otros cuadros de diálogo sean exhibidos. Si se solicita la contraseña del usuario root, digita la que configuraste anteriormente durante la instalación.

Automáticamente, Workbench se abrirá. Haz clic en la conexión que fue configurada. ¡Felicitaciones, tu ambiente de trabajo está al aire, y listo para comenzar!

**Instalación en UBUNTU**

Comandos para verificar si MySQL está instalado:

```
dpkg -l mysql-server
mysql -VCOPIA EL CÓDIGO
```

Comandos para desinstalar MySQL:

```csharp
sudo apt-getremove --purge mysql*
sudo apt-get purge mysql*
sudo apt-get autoremove
sudo apt-get autocleanCOPIA EL CÓDIGO
```

Comandos para instalar MySQL:

```csharp
sudo apt-get install mysql-server
sudo apt-get install mysql-workbenchCOPIA EL CÓDIGO
```

Configurando MySQL:

```
sudo mysql_secure_installationCOPIA EL CÓDIGO
```

Se quieres hacer login como root a través de programas externos:

```sql
sudo mysql -u rootALTERUSER 'root'@'localhost' IDENTIFIEDWITHCOPIA EL CÓDIGO
```

2. Si estás usando un nuevo equipo, o no participaste de los cursos anteriores, debes recuperar la base de datos que usaremos en este curso. Para ello, realiza los siguientes pasos:

- Abre MySQL Workbench. Usa la conexión LOCALHOST.
- Botão derecho del mouse sobre la lista de las bases y escoge **Create Schema...**
- Digita el nombre jugos_ventas. Clic en **Apply** dos veces.
- Descarga y descompacta el archivo RecuperacionAmbiente.rar.(Este archivo está disponible en la sección Preparando el Ambiente de esta Aula).
- Selecciona, en el área Navigator, la pestaña **Administration**.
- Selecciona el link **Data Import/Restore**.
- En la opción **Import From Dump Project Folder** escoge el directorio: `/DumpJugosVentas`.
- Selecciona Start Import.
- Verifica en la base jugos_ventas si las tablas fueron creadas.

3. Debemos crear la base de datos **empresa**, para ello, debemos seguir el siguiente procedimiento:

- Haz clic derecho en el área **Navigator**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/1.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/1.png)

- Luego digita el nombre de la base de datos y presiona el botón **Apply**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/2.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/2.png)

- Ahora se abrirá un asistente con los comandos usados en MySQL para crear una base de datos; haz clic en **Apply**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/3.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/3.png)

- Haz clic en **Finish** y tu base de datos estará creada:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/4.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/4.png)

4. Vamos a crear las tablas de clientes y productos empleando los siguientes comandos en un nuevo script de Workbench (no olvides especificar los campos que serán claves primarias):

```scss
CREATETABLE clientes (
DNI VARCHAR(11) NOT NULL,
NOMBRE VARCHAR(100) NULL,
DIRECCION VARCHAR(150),
BARRIO VARCHAR(50),
CIUDAD VARCHAR(50),
ESTADO VARCHAR(10),
CP VARCHAR(10),
FECHA_NACIMIENTO DATE,
EDAD SMALLINT,
SEXO VARCHAR(1),
LIMITE_CREDITO FLOAT,
VOLUMEN_COMPRA FLOAT,
PRIMERA_COMPRA BIT,
PRIMARY KEY (DNI)
);

CREATETABLE productos (
CODIGO VARCHAR(10) NOT NULL,
DESCRIPCION VARCHAR(100),
SABOR VARCHAR(50),
TAMANO VARCHAR(50),
ENVASE VARCHAR(50),
PRECIO FLOAT,
PRIMARY KEY (CODIGO)
);COPIA EL CÓDIGO
```

5. La tabla de vendedores la crearemos utilizando el asistente:

- Haz clic derecho sobre la opción **Tables** en la base de datos **empresa**, y selecciona **Create Table...**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/5.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/5.png)

- Aparecerá un asistente. Digita los campos como se muestra a continuación y haz click en **Apply**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/6.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/6.png)

- Aparecerán los comandos empleados en MySQL para la creación de la tabla:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/7.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/7.png)

- Haz clic en **Finish** y tu tabla será creada:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/8.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/8.png)

6. Ahora crearemos las tablas de facturas e items utilizando los siguientes comandos en el script de Workbench (No olvides especificar las claves primarias y foráneas):

```sql
CREATETABLE facturas(
NUMEROVARCHAR(5)NOTNULL,
FECHADATE,
DNIVARCHAR(11)NOTNULL,
MATRICULAVARCHAR(5)NOTNULL,
IMPUESTOFLOAT,
PRIMARY KEY (NUMERO),
FOREIGN KEY (DNI)REFERENCES clientes(DNI),
FOREIGN KEY (MATRICULA)REFERENCES vendedores(MATRICULA)
);
CREATETABLE items(
NUMEROVARCHAR(5)NOTNULL,
CODIGOVARCHAR(10)NOTNULL,
CANTIDADINT,
PRECIOFLOAT,
PRIMARY KEY (NUMERO, CODIGO),
FOREIGN KEY (NUMERO)REFERENCES facturas(NUMERO),
FOREIGN KEY (CODIGO)REFERENCES productos(CODIGO)
);COPIA EL CÓDIGO
```

7. Ahora, procederemos a insertar datos en nuestras tablas. Abriremos con nuestro procesador de texto el archivo anexo a este entrenamiento llamado `Carga_Tablas_Registros.sql` y seleccionaremos la línea que contiene los valores de cliente cuyo nombre es Marcos Rosas, y la copiaremos utilizando la sintaxis para comentarios de MySQL:

```bash
/*('3623344710', 'Marcos Rosas', 'Av. Universidad', 'Del Valle',
'Ciudad de México', 'EM', '22002012', '1995-01-13', 26, 'M',
110000, 220000, 1); */COPIA EL CÓDIGO
```

8. Vamos a insertar este cliente a la tabla **clientes** digitando la siguiente línea de comando:

```sql
INSERTINTO clientesVALUES('3623344710', 'Marcos Rosas', 'Av. Universidad',
'Del Valle', 'Ciudad de México', 'EM', '22002012', '1995-01-13', 26, 'M',
110000, 220000, 1);COPIA EL CÓDIGO
```

9. Al ejecutar:

```sql
SELECT *FROM clientes;COPIA EL CÓDIGO
```

¿Qué aparece en el _output_?

10. Ahora seleccionaremos y copiaremos los demás clientes que se encuentran en el archivo `Carga_Tablas_Registros.sql` y los pegaremos en el script de Workbench asegurándonos de no repetir al cliente Marcos Rosas. Ejecutaremos el script:

```objectivec
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('2600586709', 'Raúl Meneses', 'Estudiantes 89', 'Centro', 'Ciudad de México', 'EM', '22002012', '1999-08-13', 21, 'M', 120000, 210000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('19290992743', 'Rodrigo Villa', 'Libertadores 65', 'Héroes', 'Ciudad de México', 'EM', '21002020', '1998-05-30', 22, 'M', 120000, 220000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('9283760794', 'Edson Calisaya', 'Sta Úrsula Xitla', 'Barrio del Niño Jesús', 'Ciudad de México', 'EM', '22002002', '1995-01-07', 25, 'M', 150000, 250000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('7771579779', 'Marcelo Perez', 'F.C. de Cuernavaca 13', 'Carola', 'Ciudad de México', 'EM', '88202912', '1992-01-25', 29, 'M', 120000, 200000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('5576228758', 'Patricia Olivera', 'Pachuca 75', 'Condesa', 'Ciudad de México', 'EM', '88192029', '1995-01-14', 25, 'F', 70000, 160000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('8502682733', 'Luis Silva', 'Prol. 16 de Septiembre 1156', 'Contadero', 'Ciudad de México', 'EM', '82122020', '1995-01-07', 25, 'M', 110000, 190000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('1471156710', 'Erica Carvajo', 'Heriberto Frías 1107', 'Del Valle', 'Ciudad de México', 'EM', '80012212', '1990-01-01', 30, 'F', 170000, 245000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('50534475787', 'Abel Pintos', 'Carr. México-Toluca 1499', 'Cuajimalpa', 'Ciudad de México', 'EM', '22000212', '1995-01-11', 25, 'M', 170000, 260000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('5840119709', 'Gabriel Roca', 'Eje Central Lázaro Cárdenas 911', 'Del Valle', 'Ciudad de México', 'EM', '80010221', '1985-01-16', 36, 'M', 140000, 210000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('8719655770', 'Carlos Santivañez', 'Calz. del Hueso 363', 'Floresta Coyoacán', 'Ciudad de México', 'EM', '81192002', '1983-01-20', 37, 'M', 200000, 240000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('5648641702', 'Paolo Mendez', 'Martires de Tacubaya 65', 'Héroes de Padierna', 'Ciudad de México', 'EM', '21002020', '1991-01-30', 29, 'M', 120000, 220000, 0);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('492472718', 'Jorge Castro', 'Federal México-Toluca 5690', 'Locaxco', 'Ciudad de México', 'EM', '22012002', '1994-01-19', 26, 'M', 75000, 95000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('9275760794', 'Alberto Rodriguez', 'Circunvalación Oblatos 690', 'Oblatos', 'Guadalajara', 'JC', '44700000', '1991-12-28', 26, 'M', 75000, 95000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('94387575700', 'María Jimenez', 'Av. Libertadores 457', 'Barragán Hernández', 'Guadalajara', 'JC', '44469000', '1995-05-13', 26, 'F', 120000, 300000, 1);
INSERT INTO CLIENTES (DNI, NOMBRE, DIRECCION, BARRIO, CIUDAD, ESTADO, CP, FECHA_NACIMIENTO, EDAD, SEXO, LIMITE_CREDITO, VOLUMEN_COMPRA, PRIMERA_COMPRA) VALUES ('95939180787', 'Ximena Gómez', 'Jaguares 822', 'Alcalde Barranquitas', 'Guadalajara', 'JC',        '44270000', '1992-01-05', 16, 'F', 90000, 18000, 0);COPIA EL CÓDIGO
```

Así, habremos insertado con éxito todos nuestros clientes en la tabla **clientes**.

11. Repetiremos el procedimiento anterior con los comandos referentes a productos y a vendedores para llenar nuestras tablas. Ejecutamos el script:

```csharp
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('773912', 'Clean', '1 Litro', 'Naranja', 'Botella PET', 8.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('838819', 'Clean', '1,5 Litros', 'Naranja', 'Botella PET', 12.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1037797', 'Clean', '2 Litros', 'Naranja', 'Botella PET', 16.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('812829', 'Clean', '350 ml', 'Naranja', 'Lata', 2.81);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('479745', 'Clean', '470 ml', 'Naranja', 'Botella de Vidrio', 3.77);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('695594', 'Festival de Sabores', '1,5 Litros', 'Asaí', 'Botella PET', 28.51);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('243083', 'Festival de Sabores', '1,5 Litros', 'Maracuyá', 'Botella PET', 10.51);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1022450', 'Festival de Sabores', '2 Litros', 'Asái', 'Botella PET', 38.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('231776', 'Festival de Sabores', '700 ml', 'Asaí', 'Botella de Vidrio', 13.31);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('723457', 'Festival de Sabores', '700 ml', 'Maracuyá', 'Botella de Vidrio', 4.91);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('746596', 'Light', '1,5 Litros', 'Sandía', 'Botella PET', 19.51);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1040107', 'Light', '350 ml', 'Sandía', 'Lata', 4.56);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1002334', 'Línea Citrus', '1 Litro', 'Lima/Limón', 'Botella PET', 7);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1088126', 'Línea Citrus', '1 Litro', 'Limón', 'Botella PET', 7);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1041119', 'Línea Citrus', '700 ml', 'Lima/Limón', 'Botella de Vidrio', 4.9);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1042712', 'Línea Citrus', '700 ml', 'Limón', 'Botella de Vidrio', 4.9);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('520380', 'Pedazos de Frutas', '1 Litro', 'Manzana', 'Botella PET', 12.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('788975', 'Pedazos de Frutas', '1,5 Litros', 'Manzana', 'Botella PET', 18.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('229900', 'Pedazos de Frutas', '350 ml', 'Manzana', 'Lata', 4.21);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1101035', 'Refrescante', '1 Litro', 'Frutilla/Limón', 'Botella PET', 9.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1086543', 'Refrescante', '1 Litro', 'Mango', 'Botella PET', 11.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('326779', 'Refrescante', '1,5 Litros', 'Mango', 'Botella PET', 16.51);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('826490', 'Refrescante', '700 ml', 'Frutilla/Limón', 'Botella de Vidrio', 6.31);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1096818', 'Refrescante', '700 ml', 'Mango', 'Botella de Vidrio', 7.71);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('394479', 'Sabor da Montaña', '700 ml', 'Cereza', 'Botella de Vidrio', 8.41);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('783663', 'Sabor da Montaña', '700 ml', 'Frutilla', 'Botella de Vidrio', 7.71);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1000889', 'Sabor da Montaña', '700 ml', 'Uva', 'Botella de Vidrio', 6.31);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('544931', 'Verano', '350 ml', 'Limón', 'Lata', 2.46);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('235653', 'Verano', '350 ml', 'Mango', 'Lata', 3.86);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1051518', 'Verano', '470 ml', 'Limón', 'Botella de Vidrio', 3.3);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1078680', 'Verano', '470 ml', 'Mango', 'Botella de Vidrio', 5.18);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1004327', 'Vida del Campo', '1,5 Litros', 'Sandía', 'Botella PET', 19.51);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1013793', 'Vida del Campo', '2 Litros', 'Cereza/Manzana', 'Botella PET', 24.01);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('290478', 'Vida del Campo', '350 ml', 'Sandía', 'Lata', 4.56);
INSERT INTOPRODUCTOS (CODIGO, DESCRIPCION, TAMANO, SABOR, ENVASE, PRECIO)VALUES ('1002767', 'Vida del Campo', '700 ml', 'Cereza/Manzana', 'Botella de Vidrio', 8.41);
INSERTVENDEDORES (MATRICULA, NOMBRE, COMISION, FECHA_ADMISION, VACACIONES, BARRIO)VALUES ('00235','Miguel Pavón Silva',0.08,'2014-08-15', 0,'Condesa');
INSERTVENDEDORES (MATRICULA, NOMBRE, COMISION, FECHA_ADMISION, VACACIONES, BARRIO)VALUES ('00236', 'Claudia Morales',0.08,'2013-09-17', 1,'Del Valle');
INSERTVENDEDORES (MATRICULA, NOMBRE, COMISION, FECHA_ADMISION, VACACIONES, BARRIO)VALUES ('00237', 'Concepción Martinez',0.11,'2017-03-18', 1,'Contadero');
INSERTVENDEDORES (MATRICULA, NOMBRE, COMISION, FECHA_ADMISION, VACACIONES, BARRIO)VALUES ('00238', 'Patricia Sánchez',0.11,'2016-08-21', 0,'Oblatos');COPIA EL CÓDIGO
```

12. Ahora, importaremos los registros referentes a las facturas y a los items de las mismas, desde la base de datos **jugos_ventas** empleando los siguientes comandos:

```sql
INSERTINTO items
SELECT NUMERO, CODIGO_DEL_PRODUCTOAS CODIGO, CANTIDAD, PRECIO
FROM jugos_ventas.items_facturas;

INSERTINTO facturas
SELECT NUMERO, FECHA_VENTAAS FECHA, DNI, MATRICULA, IMPUESTO
FROM jugos_ventas.facturas;COPIA EL CÓDIGO
```

13. Si digitas y ejecutas los siguientes comandos al mismo tiempo:

```sql
SELECT *FROM clientes;
SELECT *FROM productos;
SELECT *FROM vendedores;
SELECT *FROM facturas F
INNERJOIN
items I
ON F.NUMERO = I.NUMERO;COPIA EL CÓDIGO
```

¿Qué observas?

14. ¡Excelente! Has creado tus tablas de forma exitosa.

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Crea un nuevo script en Workbench.
    
2. Vamos a ejecutar el siguiente comando:
    

```csharp
SELECTRAND();COPIA EL CÓDIGO
```

¿Cuál es el _output_?

El comando `RAND()` en MySQL devuelve un número aleatorio entre 0 y 1.

3. Ahora, vamos a aprender a generar un número aleatorio que sea entero dentro de un rango definido. Digita y ejecuta:

```sql
-- MIN = 20 Y MAX= 250
-- (RAND() * (MAX-MIN+1))+MIN

SELECT (RAND() * (250-20+1))+20AS ALEATORIO;
SELECT FLOOR((RAND() * (250-20+1))+20)AS ALEATORIO;COPIA EL CÓDIGO
```

4. Crearemos una función que genere números enteros de forma aleatoria pero, primero, debemos especificar el parámetro `log_bin_trust_function_creators = 1` para poderlo hacer:

```sql
SETGLOBAL log_bin_trust_function_creators = 1;COPIA EL CÓDIGO
```

- En la base de datos **empresa** haz clic derecho en la opción **Functions** y selecciona **Create Function...**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/9.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/9.png)

- Digita los comandos para la creación de la función llamada `f_aleatorio` como se muestra a continuación y haz clic en **Apply**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/10.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/10.png)

- Aparecerá un asistente con los comandos que ejecuta Workbench para la creación de funciones. Haz clic en **Apply** nuevamente:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/11.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/11.png)

- Presiona **Finish** y la función `f_aleatorio()` habrá sido creada.

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Vamos a crear una función que genere un número aleatorio y que este número aleatorio nos sirva como índice para localizar un registro específico en la tabla de clientes.

- En la base de datos **empresa** haz clic derecho en la opción **Functions** y selecciona **Create Function...**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/12.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/12.png)

- Digita los comandos para la creación de la función llamada `f_cliente_aleatorio` como se muestra a continuación (Aún no presiones **Apply**):

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/13.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/13.png)

2. Vamos a recordar cómo se utiliza la cláusula `LIMIT`. Como su nombre lo indica, esta cláusula limita el _output_ al número de registros especificado. Pero, si se le añade una `,` y un número después, ella va a contar el número de registros que anteceden a la `,` y a partir de allí seleccionará el número de registros especificado después de la `,` que puede ser 1, 2, o _n_ registros. Veamos un ejemplo práctico. Digita y ejecuta al mismo tiempo los siguiente comandos:

```sql
SELECT *FROM clientes;
SELECT *FROM clientes LIMIT 5;
SELECT *FROM clientes LIMIT 5,1;COPIA EL CÓDIGO
```

El _output_ será respectivamente:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/14.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/14.png)

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/15.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/15.png)

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/16.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/16.png)

3. Ahora, debemos especificar un registro específico basados en el resultado del número aleatorio obtenido con la función `f_aleatorio`. para ello, nos apoyaremos en la cláusula LIMIT. Digita los comandos como se muestra a continuación y presiona **Apply**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/17.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/17.png)

- Aparecerá un asistente con los comandos que ejecuta Workbench para la creación de funciones. Haz clic en **Apply** nuevamente:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/18.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/18.png)

- Presiona **Finish** y la función `f_cliente_aleatorio()` habrá sido creada.

4. Digita y ejecuta la siguiente línea de comando:

```csharp
SELECTf_cliente_aleatorio() AS CLIENTE;COPIA EL CÓDIGO
```

¿Cuál número de DNI te devolvió la función?

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Crea un nuevo script en Workbench.
    
2. Digita los siguientes comandos y ejecútalos varias veces:
    

```scss
SELECT f_cliente_aleatorio() AS CLIENTE,
f_producto_aleatorio() AS PRODUCTO,
f_vendedor_aleatorio() AS VENDEDOR;COPIA EL CÓDIGO
```

¿Qué observas en cada ejecución? Los clientes, productos y vendedores son generados de forma aleatoria. Estamos listos para generar una venta ficticia.

3. Ahora, crearemos un **Stored Procedure** llamado **sp_venta** en el cual estableceremos los parámetros: Fecha, número máximo de items, y cantidad máxima de cada producto. De esta manera, vamos a generar una venta utilizando las funciones creadas previamente para crear un nuevo registro en la tabla de facturas con sus respectivos productos y cantidades en la tabla de items.

Para crear este **sp**, vamos a hacer lo siguiente:

- En la base de datos **empresa** haz clic derecho en la opción **Stored Procedures** y selecciona **Create Stored Procedure...**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/19.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/19.png)

- Digita los siguientes comandos al interior del **Stored Procedure** que llamaremos `sp_ventas`. Estableceremos como parámetros: `(fecha DATE, maxitems INT, maxcantidad INT)`. Digitaremos y ejecutaremos los siguientes comandos entre las cláusulas `BEGIN` y `END` del **Stored Procedure**:

```sql
DECLARE vclienteVARCHAR(11);
DECLARE vproductoVARCHAR(10);
DECLARE vvendedorVARCHAR(5);
DECLARE vcantidadINT;
DECLARE vprecioFLOAT;
DECLARE vitensINT;
DECLARE vnfacturaINT;
DECLARE vcontadorINTDEFAULT 1;
SELECT MAX(NUMERO) + 1INTO vnfacturaFROM facturas;
SET vcliente = f_cliente_aleatorio();
SET vvendedor = f_vendedor_aleatorio();
INSERTINTO facturas (NUMERO, FECHA, DNI, MATRICULA, IMPUESTO)VALUES (vnfactura, fecha, vcliente, vvendedor, 0.16);
SET vitens = f_aleatorio(1,  maxitems);
WHILE vcontador <= vitens
DO
SET vproducto = f_producto_aleatorio();
SET vcantidad = f_aleatorio(1, maxcantidad);
SELECT PRECIOINTO vprecioFROM productosWHERE CODIGO = vproducto;
INSERTINTO items(NUMERO, CODIGO, CANTIDAD, PRECIO)VALUES(vnfactura, vproducto, vcantidad, vprecio);
SET vcontador = vcontador+1;
END WHILE;COPIA EL CÓDIGO
```

- Nuestro **sp** quedará así:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/20.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/20.png)

- Haz clic en **Apply** 2 veces y al final en **Finish**. Nuestro **Stored Procedure habrá sido creado**

4. Vamos a probar el **sp**. Ejecutemos el siguiente comando:

```csharp
CALLsp_venta('20210619', 15, 100);COPIA EL CÓDIGO
```

¿Qué sucedió?

5. Se presenta un error de duplicidad en la clave primaria porque el campo `NUMERO` en las tablas de `factura` y de `items` fue definido como VARCHAR y cuando aplicamos la función MAX() al campo `NUMERO` MySQL lo evalúa como texto y no como número. Por ello, debemos eliminar nuestras tablas de `factura` y de `items` y recrearlas utilizando el _datatype_ INT para el campo `NUMERO` en ambas tablas. Digita y ejecuta los siguientes comandos en el script de Workbench:

```sql
DROPTABLE facturas;
CREATETABLE facturas(
NUMEROINTNOTNULL,
FECHADATE,
DNIVARCHAR(11)NOTNULL,
MATRICULAVARCHAR(5)NOTNULL,
IMPUESTOFLOAT,
PRIMARY KEY (NUMERO),
FOREIGN KEY (DNI)REFERENCES clientes(DNI),
FOREIGN KEY (MATRICULA)REFERENCES vendedores(MATRICULA)
);

DROPTABLE items;
CREATETABLE items(
NUMEROINTNOTNULL,
CODIGOVARCHAR(10)NOTNULL,
CANTIDADINT,
PRECIOFLOAT,
PRIMARY KEY (NUMERO, CODIGO),
FOREIGN KEY (NUMERO)REFERENCES facturas(NUMERO),
FOREIGN KEY (CODIGO)REFERENCES productos(CODIGO)
);

INSERTINTO items
SELECT NUMERO, CODIGO_DEL_PRODUCTOAS CODIGO, CANTIDAD, PRECIO
FROM jugos_ventas.items_facturas;

INSERTINTO facturas
SELECT NUMERO, FECHA_VENTAAS FECHA, DNI, MATRICULA, IMPUESTO
FROM jugos_ventas.facturas;COPIA EL CÓDIGO
```

6. Vamos a probar el **sp** nuevamente. Ejecutemos el siguiente comando:

```csharp
CALLsp_venta('20210619', 3, 100);COPIA EL CÓDIGO
```

El comando funcionó correctamente. Sin embargo, en la próxima aula, realizaremos nuevas pruebas al **Stored Procedure** para validarlo.

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Crea un nuevo script en Workbench.
    
2. Para validar el **Stored Procedure** realizaremos una consulta para obtener el cálculo de la facturación y ver si efectivamente la rutina `sp_ventas` está funcionando como debería:
    

```vbnet
SELECT A.FECHA, SUM(B.CANTIDAD*B.PRECIO)AS FACTURACION
FROM facturas A
INNERJOIN
items B
ON A.NUMERO = B.NUMERO
WHERE A.FECHA = '20210619'
GROUPBY A.FECHA;COPIA EL CÓDIGO
```

3. Digita y ejecuta el siguiente comando:

```csharp
CALLsp_venta('20210619', 20, 100);COPIA EL CÓDIGO
```

¿Qué ha sucedido?

4. Se ha presentado otro error de duplicidad en la clave primaria, esta vez en la tabla de `items` porque los campos `NUMERO` y `CODIGO` son claves primarias, y al generar 20 productos de forma aleatoria de una tabla que contiene 35 productos en total, la probabilidad de que obtengamos el mismo producto más de una vez es alta, y como la venta relaciona un mismo número de factura a los diversos productos, si se repite algún código de producto, habrá duplicidad en la tabla de `items` por tratarse del mismo número de factura. Dicho de otra forma, no podemos tener un mismo número de factura y un mismo código de producto en más de un registro en la misma tabla.

Para solucionar este inconveniente, debemos crear un nuevo contador que verifique que los productos en la tabla de `items` no se repitan. Después de hacer esta verificación, el **sp** podrá insertar datos a la tabla de `items`. Esto lo solucionaremos estableciendo una condición con el comando `IF`. Así, lo que debemos hacer es:

- Haz clic derecho sobre `sp_ventas` y selecciona la opción **Alter Stored Procedure**:

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/21.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/21.png)

- Realiza los siguientes ajustes como se muestra en la imagen (Observa atentamente los comentarios en las líneas de comando):

![https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/22.png](https://caelum-online-public.s3.amazonaws.com/ESP-1834-proyecto-final-sql-con-mysql/22.png)

- Haz clic en **Apply** 2 veces y al final en **Finish**. Nuestro **Stored Procedure habrá sido modificado**

5. Digita y Ejecuta los siguientes comandos:

```vbnet
CALL sp_venta('20210619', 20, 100);
SELECT A.FECHA, SUM(B.CANTIDAD*B.PRECIO)AS FACTURACION
FROM facturas A
INNERJOIN
items B
ON A.NUMERO = B.NUMERO
WHERE A.FECHA = '20210619'
GROUPBY A.FECHA;COPIA EL CÓDIGO
```

Vuelve y ejecuta los comandos anteriores, pero esta vez, colocaremos el parámetro `maxitems = 100`:

```csharp
CALLsp_venta('20210619', 100, 100);COPIA EL CÓDIGO
```

¿Qué sucede con la facturación?

La facturación aumenta, y comprobamos que efectivamente nuestro **sp** está funcionando como esperado.

6. Ahora, tomaremos los TRIGGERS que utilizamos en el curso de Manipulación de datos y los mejoraremos a través de **sp**. Digita y ejecuta los siguientes comandos:

```sql
CREATETABLE facturacion(
FECHADATENULL,
VENTA_TOTALFLOAT
);

DELIMITER //
CREATETRIGGER TG_FACTURACION_INSERT
AFTERINSERTON items
FOREACHROWBEGINDELETEFROM facturacion;
INSERTINTO facturacion
SELECT A.FECHA, SUM(B.CANTIDAD * B.PRECIO)AS VENTA_TOTAL
FROM facturas A
INNERJOIN
  items B
ON A.NUMERO = B.NUMERO
GROUPBY A.FECHA;
END //

DELIMITER //
CREATETRIGGER TG_FACTURACION_DELETE
AFTERDELETEON items
FOREACHROWBEGINDELETEFROM facturacion;
INSERTINTO facturacion
SELECT A.FECHA, SUM(B.CANTIDAD * B.PRECIO)AS VENTA_TOTAL
FROM facturas A
INNERJOIN
  items B
ON A.NUMERO = B.NUMERO
GROUPBY A.FECHA;
END //

DELIMITER //
CREATETRIGGER TG_FACTURACION_UPDATE
AFTERUPDATEON items
FOREACHROWBEGINDELETEFROM facturacion;
INSERTINTO facturacion
SELECT A.FECHA, SUM(B.CANTIDAD * B.PRECIO)AS VENTA_TOTAL
FROM facturas A
INNERJOIN
  items B
ON A.NUMERO = B.NUMERO
GROUPBY A.FECHA;
END //

SELECT *FROM facturacionWHERE FECHA = '20210622';

CALL sp_venta('20210622', 15, 100);COPIA EL CÓDIGO
```

7. Si en algún momento cambian las reglas de negocio, tendríamos que modificar cada TRIGGER por separado, pero si almacenamos los comandos recurrentes en un **sp**, entonces el TRIGGER siempre va a llamar al **sp** y en caso de cambios en las reglas de negocio, únicamente habría que modificar el **sp**. Vamos a crear un **sp** llamado `sp_triggers` y en el amacenaremos los comandos que se repiten en cada TRIGGER:

```sql
DELIMITER //
CREATEPROCEDURE `sp_triggers`()
BEGINDELETEFROM facturacion;
INSERTINTO facturacion
SELECT A.FECHA, SUM(B.CANTIDAD * B.PRECIO)AS VENTA_TOTAL
FROM facturas A
INNERJOIN
  items B
ON A.NUMERO = B.NUMERO
GROUPBY A.FECHA;
END //COPIA EL CÓDIGO
```

8. Finalmente, eliminaremos los TRIGGERS y los recrearemos utilizando la rutina `sp_triggers` como se muestra a continuación:

```sql
DROPTRIGGER TG_FACTURACION_DELETE;
DROPTRIGGER TG_FACTURACION_UPDATE;
DROPTRIGGER TG_FACTURACION_INSERT;

DELIMITER //
CREATETRIGGER TG_FACTURACION_INSERT
AFTERINSERTON items
FOREACHROWBEGINCALL sp_triggers;
END //

DELIMITER //
CREATETRIGGER TG_FACTURACION_DELETE
AFTERDELETEON items
FOREACHROWBEGINCALL sp_triggers;
END //

DELIMITER //
CREATETRIGGER TG_FACTURACION_UPDATE
AFTERUPDATEON items
FOREACHROWBEGINCALL sp_triggers;
END //
```

Aquí puedes descargar los archivos del proyecto completo.

[Descargue los archivos en Github](https://github.com/alura-es-cursos/1834-proyecto-final-sql-con-mysql/tree/proyecto-final) o haga clic [aquí](https://github.com/alura-es-cursos/1834-proyecto-final-sql-con-mysql/archive/refs/heads/proyecto-final.zip) para descargarlos directamente.