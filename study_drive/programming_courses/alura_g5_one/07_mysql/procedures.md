# Stored Procedures

- [MySQL](https://dev.mysql.com/doc/refman/8.0/en/create-procedure.html)
- [MariaDB](https://mariadb.com/kb/en/stored-procedures/)
- [MariaDB Create Procedure](https://mariadb.com/kb/en/create-procedure/)
- MariaDB [W3Schools](https://www.w3schools.blog/procedure-mariadb) procedure

```sql
CREATE
    [DEFINER = user]
    PROCEDURE sp_name ([proc_parameter[, ...]])
    [characteristic ...] routine_body

CREATE
    [DEFINER = user]
    FUNCTION sp_name ([func_parameter[, ...]])
    RETURN type
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

func_parameter:
    param_name type

type:
    Any valid MySQL data type

characteristic: {
    COMMENT 'string'
    | LANGUAGE SQL
    | [NOT] DETERMINISTIC
    | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
    | SQL SECURITY { DEFINER | INVOKER }
}

routine_body:
    Valid SQL routine statement
```

```sql
CREATE PROCEDURE
    <nombre_del_procedimiento> (parámetros)
BEGIN
    DECLARE <declaración_de_variables>;
    ...
    <ejecuciones_del_procedimiento>;
    ...
END;
```

### Ejemplo STORE PROCEDURE

```sql
USE jugos_ventas;

DROP PROCEDURE IF EXISTS print_procedure;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE print_procedure ()
BEGIN
    SELECT CONCAT("Procedimiento,", " de ", "ejemplo");
END$$
DELIMITER ;
```

### Stored Procedures Sintaxis

- Puede tener **letras**, **números** y los caracteres `$` y `_`
- Tamaño máximo: 64 caracteres
- Nombre único
- Case Sensitive

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS hola_pianola;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE hola_pianola ()
BEGIN
    SELECT "Hola pianola!";
END$$

DELIMITER ;
```

### CALL Procedure

```sql
CALL print_procedure;
+-----------------------------+
| CONCAT("hola,", " pianola") |
+-----------------------------+
| hola, pianola               |
+-----------------------------+
```


```sql
CALL hola_pianola;

+---------------+
| Hola pianola! |
+---------------+
| Hola pianola! |
+---------------+
```

#### Otros ejemplos

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS calcular;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE calcular ()
BEGIN
    /* Este procedure tiene un comentario */
    -- Suma 6+3 y lo mútliplica por 5
    # Y muestra el "RESULTADO"
    SELECT (6+3)*5 AS RESULTADO;
END$$

DELIMITER ;

CALL calcular;

+-----------+
| RESULTADO |
+-----------+
|        45 |
+-----------+
```

## Eliminar procedure

```sql
DROPR PROCEDURE jugos_ventas.hola_pianola;
```

### Ejemplo de un procedimiento almacenado mas extenso

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS proc_extenso;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE proc_extenso ()
BEGIN
    INSERT INTO tabla_de_productos (
        CODIGO_DEL_PRODUCTO,
        NOMBRE_DEL_PRODUCTO,
        SABOR,
        TAMANO,
        ENVASE,
        PRECIO_DE_LISTA
    ) VALUES
        ('1001001','Sabor Alpino','Mango','700 ml','Botella',7.50),
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

    SELECT * FROM tabla_de_productos
    WHERE NOMBRE_DEL_PRODUCTO LIKE 'Sabor Alp%';

    UPDATE tabla_de_productos
    SET PRECIO_DE_LISTA = 5
    WHERE NOMBRE_DEL_PRODUCTO LIKE 'Sabor Alp%';

    SELECT * FROM tabla_de_productos
    WHERE NOMBRE_DEL_PRODUCTO LIKE 'Sabor Alp%';

    DELETE FROM tabla_de_productos
    WHERE NOMBRE_DEL_PRODUCTO LIKE 'Sabor Alp%';

    SELECT * FROM tabla_de_productos
    WHERE NOMBRE_DEL_PRODUCTO LIKE 'Sabor Alp%';
END$$

DELIMITER ;
```

```sql
CALL proc_extenso;
```

```txt
+---------------------+---------------------+---------+------------+---------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO  | SABOR      | ENVASE  | PRECIO_DE_LISTA |
+---------------------+---------------------+---------+------------+---------+-----------------+
| 1001000             | Sabor Alpino        | 700 ml  | Melón      | Botella |             7.5 |
| 1001001             | Sabor Alpino        | 700 ml  | Mango      | Botella |             7.5 |
| 1001002             | Sabor Alpino        | 700 ml  | Guanábana  | Botella |             7.5 |
| 1001003             | Sabor Alpino        | 700 ml  | Mandarina  | Botella |             7.5 |
| 1001004             | Sabor Alpino        | 700 ml  | Banana     | Botella |             7.5 |
| 1001005             | Sabor Alpino        | 700 ml  | Asaí       | Botella |             7.5 |
| 1001006             | Sabor Alpino        | 1 Litro | Mango      | Botella |             7.5 |
| 1001007             | Sabor Alpino        | 1 Litro | Melón      | Botella |             7.5 |
| 1001008             | Sabor Alpino        | 1 Litro | Guanábana  | Botella |             7.5 |
| 1001009             | Sabor Alpino        | 1 Litro | Mandarina  | Botella |             7.5 |
| 1001010             | Sabor Alpino        | 1 Litro | Banana     | Botella |             7.5 |
| 1001011             | Sabor Alpino        | 1 Litro | Asaí       | Botella |             7.5 |
+---------------------+---------------------+---------+------------+---------+-----------------+
12 rows in set (0.008 sec)

+---------------------+---------------------+---------+------------+---------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO  | SABOR      | ENVASE  | PRECIO_DE_LISTA |
+---------------------+---------------------+---------+------------+---------+-----------------+
| 1001000             | Sabor Alpino        | 700 ml  | Melón      | Botella |               5 |
| 1001001             | Sabor Alpino        | 700 ml  | Mango      | Botella |               5 |
| 1001002             | Sabor Alpino        | 700 ml  | Guanábana  | Botella |               5 |
| 1001003             | Sabor Alpino        | 700 ml  | Mandarina  | Botella |               5 |
| 1001004             | Sabor Alpino        | 700 ml  | Banana     | Botella |               5 |
| 1001005             | Sabor Alpino        | 700 ml  | Asaí       | Botella |               5 |
| 1001006             | Sabor Alpino        | 1 Litro | Mango      | Botella |               5 |
| 1001007             | Sabor Alpino        | 1 Litro | Melón      | Botella |               5 |
| 1001008             | Sabor Alpino        | 1 Litro | Guanábana  | Botella |               5 |
| 1001009             | Sabor Alpino        | 1 Litro | Mandarina  | Botella |               5 |
| 1001010             | Sabor Alpino        | 1 Litro | Banana     | Botella |               5 |
| 1001011             | Sabor Alpino        | 1 Litro | Asaí       | Botella |               5 |
+---------------------+---------------------+---------+------------+---------+-----------------+
12 rows in set (0.011 sec)

Empty set (0.016 sec)

Query OK, 36 rows affected (0.016 sec)
```

## Variables en procedure

```sql
DECLARE <nombre_de_variable> <datatype> DEFAULT <value>;
```

- **Datatype** es olbligatorio
- **DEFAULT** es opcional, si no se establece, default es **NULL**
- Solo **letras**, **números**, `$` y `_`
- Se pueden declarar varias, con la restricción que sean del mismo tipo
- Tamaño máximo de 255 caracteres
- Nombre único, **case sesitive**
- La declaración termina con `;`


```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS mostrar_vars;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE mostrar_vars ()
BEGIN
    DECLARE texto CHAR(20) DEFAULT 'Hola';
    DECLARE numero INTEGER DEFAULT 789;
    DECLARE decimales DECIMAL(5,3) DEFAULT 45.678;
    DECLARE fecha DATE DEFAULT '2023-10-10';
    DECLARE tiempo TIMESTAMP DEFAULT CURRENT_TIMESTAMP();
    SELECT texto, numero, decimales, fecha, tiempo;
    /* Modificando valor */
    SET numero = 1;
    SELECT numero AS numero_modificado;
END$$

DELIMITER ;

CALL mostrar_vars;
```

```txt
+-------+--------+-----------+------------+---------------------+
| texto | numero | decimales | fecha      | tiempo              |
+-------+--------+-----------+------------+---------------------+
| Hola  |    789 |    45.678 | 2023-10-10 | 2023-10-21 13:11:34 |
+-------+--------+-----------+------------+---------------------+
```

***Crear un Stored Procedure que actualice la edad de los clientes.***

```sql
/* Cálculo de edad */
TIMESTAMPDIFF(YEAR, FECHA_DE_NACIMIENTO, CURDATE()) as EDAD
```

```sql
DELIMITER $$
CREATE PROCEDURE `calcula_edad`()
BEGIN
    UPDATE tabla_de_clientes
    SET EDAD =  TIMESTAMPDIFF(YEAR, FECHA_DE_NACIMIENTO, CURDATE());
END $$
```

## Procedure params

### Ejemplo de procedimiento con variables

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS insert_prod;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE insert_prod (
    vcodigo VARCHAR(20),
    vnombre VARCHAR(20),
    vsabor VARCHAR(20),
    vtamano VARCHAR(20),
    venvase VARCHAR(20),
    vprecio DECIMAL(4,2)
)
BEGIN
    /*
        DECLARE vcodigo VARCHAR(20) DEFAULT '30030001';
        DECLARE vnombre VARCHAR(20) DEFAULT 'Sabor Intenso';
        DECLARE vsabor VARCHAR(20) DEFAULT 'Tutti Frutti';
        DECLARE vtamano VARCHAR(20) DEFAULT '700 ml';
        DECLARE venvase VARCHAR(20) DEFAULT 'Botella PET';
        DECLARE vprecio DECIMAL(4,2) DEFAULT '7.25';
    */
    INSERT INTO tabla_de_productos (
        CODIGO_DEL_PRODUCTO, NOMBRE_DEL_PRODUCTO, SABOR,
        TAMANO, ENVASE, PRECIO_DE_LISTA
    ) VALUES
        (vcodigo, vnombre, vtamano, vsabor, venvase, vprecio);
END$$

DELIMITER ;
```

```sql
CALL insert_prod(
    '1000800', 'Sabor del Mar', '700 ml',
    'Naranja', 'Botella de Vidrio', 2.25
);
```

***Crear un Stored Procedure para reajustar la comisión de los vendedores.***
***Usando como parámetro el valor (Ej. 0,90).***

```sql
DELIMITER $$
CREATE PROCEDURE `reajuste_comision`(vporcentaje FLOAT)
BEGIN
    UPDATE tabla_de_vendedores
    SET PORCENTAJE_COMISION = PORCENTAJE_COMISION * (1 + vporcentaje);
END $$
```

#### Ejemplo de procedimiento que establece una variable `a`

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS ejm_params;

DELIMITER //
USE jugos_ventas//

CREATE PROCEDURE ejm_params (OUT param1 INTEGER)
BEGIN
    DECLARE total INTEGER DEFAULT 16;
    SET param1 = total;
END; //

DELIMITER ;

CALL ejm_params(@a);

SELECT @a;
```

## Manejo de errores en procedures

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS insert_prod;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE insert_prod (
    vcodigo VARCHAR(20),
    vnombre VARCHAR(20),
    vsabor VARCHAR(20),
    vtamano VARCHAR(20),
    venvase VARCHAR(20),
    vprecio DECIMAL(4,2)
)
BEGIN
    DECLARE mensaje VARCHAR(40);
    DECLARE EXIT HANDLER FOR 1062
    BEGIN
        SET mensaje = 'PK duplicada!';
        SELECT mensaje AS ERROR_PK;
    END;
    INSERT INTO tabla_de_productos (
        CODIGO_DEL_PRODUCTO, NOMBRE_DEL_PRODUCTO, SABOR,
        TAMANO, ENVASE, PRECIO_DE_LISTA
    ) VALUES
    (vcodigo, vnombre, vtamano, vsabor, venvase, vprecio);
    SET mensaje = "Producto insertado con exito";
    SELECT mensaje AS INFORMACION;
END$$

DELIMITER ;
```

```sql
CALL insert_prod(
    '1000800', 'Sabor del Mar', '700 ml',
    'Naranja', 'Botella de Vidrio', 2.25
);

+---------------+
| ERROR_PK      |
+---------------+
| PK duplicada! |
+---------------+

CALL insert_prod(
    '1000801', 'Sabor del Mar', '900 ml',
    'Naranja', 'Botella de Vidrio', 2.45
);

+------------------------------+
| INFORMACION                  |
+------------------------------+
| Producto insertado con exito |
+------------------------------+
```

### Ejemplo rutine que muestra el sabor del produco insertado

```sql
USE jugos_ventas;
DROP PROCEDURE IF EXISTS mostrar_sabor;

DELIMITER $$
USE jugos_ventas$$
CREATE PROCEDURE mostrar_sabor (
    vcodigo VARCHAR(20) COLLATE 'utf8mb4_uca1400_as_ci'
)
BEGIN
    DECLARE vsabor VARCHAR(20);
    SELECT SABOR INTO vsabor FROM tabla_de_productos
    WHERE CODIGO_DEL_PRODUCTO = `vcodigo`;
    SELECT vsabor AS SABOR;
END$$

DELIMITER ;
```

```sql
CALL mostrar_sabor('1000800');

+---------+
| SABOR   |
+---------+
| Naranja |
+---------+
```

***Crear una variable `N_FACTURAS` y asignar el número de facturas del día***
***`01/01/2017` y mostrar el valor de la variable.***

```sql
DELIMITER $$
CREATE PROCEDURE `cantidad_facturas`()
BEGIN
    DECLARE N_FACTURAS INTEGER;
    SELECT COUNT(*) INTO N_FACTURAS FROM facturas WHERE
    FECHA_VENTA = '2017-01-01';
    SELECT N_FACTURAS;
END $$
```

## IF THEN ELSE

```sql
IF <condition> THEN
    <if_statements>;
ELSE
    <else_statements>;
END IF
```

```sql
DROP PROCEDURE IF EXISTS edad_clientes;
DELIMITER $$
CREATE PROCEDURE edad_clientes(
    vdni VARCHAR(20) COLLATE 'utf8mb4_uca1400_as_ci'
)
BEGIN
    DECLARE vresultado VARCHAR(50);
    DECLARE vfecha DATE;
    SELECT fecha_de_nacimiento INTO vfecha
    FROM tabla_de_clientes WHERE dni=vdni;
    IF
        vfecha < '19950101'
    THEN
        SET vresultado = 'Cliente Antiguo';
    ELSE
        SET vresultado = 'Cliente Normal';
    END IF;
    SELECT vresultado AS TIPO_CLIENTE;
END $$

DELIMITER ;
```

```
CALL edad_clientes('1471156710');
+-----------------+
| TIPO_CLIENTE    |
+-----------------+
| Cliente Antiguo |
+-----------------+

CALL edad_clientes('3623344710');
+----------------+
| TIPO_CLIENTE   |
+----------------+
| Cliente Normal |
+----------------+
```

***Crear un Stored Procedure que, según una fecha dada, calcule el número***
***de facturas. Si el resultado son más de 70 facturas mostar el mensaje:***
***'Muchas facturas'. En otro caso, el mensaje será 'Pocas facturas'.***
***En ambos casos se debe mostrar el número de facturas***

```sql
DROP PROCEDURE IF EXISTS cant_facturas;
DELIMITER $$
CREATE PROCEDURE cant_facturas(vfecha DATE)
BEGIN
    DECLARE vresultado VARCHAR(50);
    DECLARE vfacturas INTEGER;
    SELECT COUNT(matricula) INTO vfacturas
    FROM facturas WHERE fecha_venta=vfecha;
    IF
        vfacturas > 70
    THEN
        SET vresultado = CONCAT('Muchas Facturas: ', vfacturas);
        # SET vresultado = 'Muchas Facturas: ';
    ELSE
        SET vresultado = CONTACT('Pocas Facturas: ', vfacturas);
        # SET vresultado = 'Pocas Facturas: ';
    END IF;
    SELECT vresultado AS FACTURAS_EN_FECHA;
END $$

DELIMITER ;
```

```sql
CALL cant_facturas('2018-01-01');
+---------------------+
| FACTURAS_EN_FECHA   |
+---------------------+
| Muchas Facturas: 87 |
+---------------------+
```

### ELSE IF

```sql
IF <condition> THEN
    <if_statements>;
ELSEIF <condition>
    <elseif_statements>;
    (...)
ELSEIF <condition>
    <elseif_statements>;
ELSE
    <else_statements>;
END IF;
```

```sql
/*
    precio >= 12 producto costoso
    precio >= 12 < 12 producto asequible
    precio < 7 producto barato
*/
DROP PROCEDURE IF EXISTS clasific_precio;
DELIMITER $$
CREATE PROCEDURE clasific_precio(
    vcodigo VARCHAR(20)
    COLLATE 'utf8mb4_uca1400_as_ci'
)
BEGIN
    DECLARE vresultado VARCHAR(40);
    DECLARE vprecio FLOAT;

    SELECT precio_de_lista INTO vprecio
    FROM tabla_de_productos
    WHERE codigo_del_producto = vcodigo;

    IF vprecio >= 12 THEN
        SET vresultado = CONCAT(
            'Producto Costoso: $', vprecio, '.-'
        );
    ELSEIF vprecio >= 7 AND vprecio < 12 THEN
        SET vresultado = CONCAT(
            'Producto Asequible: $', vprecio, '.-'
        );
    ELSE
        SET vresultado = CONCAT(
            'Producto Barato: $', vprecio, '.-'
        );
    END IF;
    SELECT vresultado AS "Clasificación por precio";
END $$

DELIMITER ;
```

```sql
CALL clasific_precio('1000800');
+--------------------------+
| Clasificación por precio |
+--------------------------+
| Producto Barato: $2.25.- |
+--------------------------+

CALL clasific_precio('783663');
+-----------------------------+
| Clasificación por precio    |
+-----------------------------+
| Producto Asequible: $7.71.- |
+-----------------------------+

CALL clasific_precio('1004327');
+----------------------------+
| Clasificación por precio   |
+----------------------------+
| Producto Costoso: $19.51.- |
+----------------------------+
```

***Crear un Stored Procedure `comparacion_ventas` que compare las ventas***
***en entre 2 fechas distintas. Si el porcentaje de variación es mayor al***
***10% debe retornar 'Verde'. Si está entre -10% y 10% debe restornar***
***'Amarillo'. Si es menor al -10% debe retornar 'Rojo'***

```sql
/*
    total > 10% Verde
    precio <= 10% y >= -10% Amarillo
    precio < -10% Rojo
*/
DROP PROCEDURE IF EXISTS comparacion_ventas;
DELIMITER $$
CREATE PROCEDURE comparacion_ventas(
    vfecha1 DATE,
    vfecha2 DATE
)
BEGIN
    DECLARE vresultado VARCHAR(50);
    DECLARE vtotal1 INTEGER;
    DECLARE vtotal2 INTEGER;
    DECLARE vtotal FLOAT;

    SELECT SUM(B.CANTIDAD * B.PRECIO) INTO vtotal1
    FROM facturas A INNER JOIN items_facturas B
    ON A.NUMERO = B.NUMERO
    WHERE A.FECHA_VENTA = vfecha1;

    SELECT SUM(B.CANTIDAD * B.PRECIO) INTO vtotal2
    FROM facturas A INNER JOIN items_facturas B
    ON A.NUMERO = B.NUMERO
    WHERE A.FECHA_VENTA = vfecha2;

    SELECT ROUND(((vtotal2*100)/vtotal1)-100, 2) INTO vtotal;
    
    IF vtotal > 10 THEN
        SET vresultado = CONCAT('Verde: ', vtotal, '%');
    ELSEIF vtotal >= -10 AND vtotal <= 10 THEN
        SET vresultado = CONCAT('Amarillo: ', vtotal, '%');
    ELSE
        SET vresultado = CONCAT('Rojo: ', vtotal, '%');
    END IF;
    SELECT vresultado AS "Porcentaje variación ventas";
END $$

DELIMITER ;
```

```sql
CALL comparacion_ventas('2016-12-31','2017-12-31');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Verde: 28.23%                |
+------------------------------+
```

```sql
CALL comparacion_ventas('2018-03-28','2018-03-27');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Amarillo: -2.2%              |
+------------------------------+
```

```sql
CALL comparacion_ventas('2018-03-28','2015-03-28');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Rojo: -41.86%                |
+------------------------------+
```

## CASE

```sql
CASE <selector>
    WHEN <selector_value_1> THEN <then_statement_1>;
    WHEN <selector_value_2> THEN <then_statement_2>;
    (...)
    WHEN <selector_value_n> THEN <then_statement_n>;
    [ELSE <else_statements>]
END CASE;
```

```sql
/*
Maracuya = Rico
Frutilla = Rico
Uva = Rico
Sandía = Normal
Mango = Normal
Otros = Comunes
*/
DROP PROCEDURE IF EXISTS clasif_sabores;
DELIMITER $$
CREATE PROCEDURE clasif_sabores(
    vcodigo VARCHAR(20)
    COLLATE 'utf8mb4_uca1400_as_ci'
)
BEGIN
    DECLARE msg_error VARCHAR(40);
    DECLARE vresultado VARCHAR(50);
    DECLARE vsabor VARCHAR(50);
    # Se comenta el ELSE para testear HANDLER CASE NOT FOUND
    #DECLARE CONTINUE HANDLER FOR 1339
    DECLARE EXIT HANDLER FOR 1339
    BEGIN
        SET msg_error = "Sabor sin clasificar!";
        SELECT msg_error AS "ERROR CASE NOT FOUND";
    END;

    SELECT sabor INTO vsabor
    FROM tabla_de_productos
    WHERE codigo_del_producto = vcodigo;

    CASE vsabor
        WHEN 'Maracuya' THEN SELECT 'Rico' INTO vresultado;
        WHEN 'Limón' THEN SELECT 'Rico' INTO vresultado;
        WHEN 'Frutilla' THEN SELECT 'Rico' INTO vresultado;
        WHEN 'Uva' THEN SELECT 'Rico' INTO vresultado;
        WHEN 'Sandía' THEN SELECT 'Normal' INTO vresultado;
        WHEN 'Mango' THEN SELECT 'Normal' INTO vresultado;
        #ELSE SELECT 'Común' INTO vresultado;
    END CASE;
    SELECT CONCAT(vsabor, ': ', vresultado) AS "Clasificación de sabor";
END $$

DELIMITER ;
```

```sql
CALL clasif_sabores('1042712');
+-------------------------+
| Clasificación de sabor  |
+-------------------------+
| Limón: Rico             |
+-------------------------+
```

```sql
CALL clasif_sabores('1004327');
+-------------------------+
| Clasificación de sabor  |
+-------------------------+
| Sandía: Normal          |
+-------------------------+
```

```sql
CALL clasif_sabores('394479');
+-------------------------+
| Clasificación de sabor  |
+-------------------------+
| Cereza: Común           |
+-------------------------+
```

### CASE condicional

```sql
DROP PROCEDURE IF EXISTS clasif_precio;
DELIMITER $$
CREATE PROCEDURE clasif_precio(
    vcodigo VARCHAR(20)
    COLLATE 'utf8mb4_uca1400_as_ci'
)
BEGIN
    DECLARE vresultado VARCHAR(40);
    DECLARE vprecio FLOAT;

    SELECT precio_de_lista INTO vprecio
    FROM tabla_de_productos
    WHERE codigo_del_producto = vcodigo;

    CASE
    WHEN vprecio >= 12 THEN
        SET vresultado = CONCAT(
            'Producto Costoso: $', vprecio, '.-'
        );
    WHEN vprecio >= 7 AND vprecio < 12 THEN
        SET vresultado = CONCAT(
            'Producto Asequible: $', vprecio, '.-'
        );
    ELSE
        SET vresultado = CONCAT(
            'Producto Barato: $', vprecio, '.-'
        );
    END CASE;
    SELECT vresultado AS "Clasificación por precio";
END $$

DELIMITER ;
```

```sql
CALL clasif_precio('1013793');
+----------------------------+
| Clasificación por precio   |
+----------------------------+
| Producto Costoso: $24.01.- |
+----------------------------+
```

```sql
CALL clasif_precio('1096818');
+-----------------------------+
| Clasificación por precio    |
+-----------------------------+
| Producto Asequible: $7.71.- |
+-----------------------------+
```

```sql
CALL clasif_precio('1000801');
+---------------------------+
| Clasificación por precio  |
+---------------------------+
| Producto Barato: $2.45.-  |
+---------------------------+
```

***Modificar SP `comparacion_ventas` usando CASE***

```sql
DROP PROCEDURE IF EXISTS compara_ventas;
DELIMITER $$
CREATE PROCEDURE compara_ventas(
    vfecha1 DATE,
    vfecha2 DATE
)
BEGIN
    DECLARE vresultado VARCHAR(50);
    DECLARE vtotal1 INTEGER;
    DECLARE vtotal2 INTEGER;
    DECLARE vtotal FLOAT;

    SELECT SUM(B.CANTIDAD * B.PRECIO) INTO vtotal1
    FROM facturas A INNER JOIN items_facturas B
    ON A.NUMERO = B.NUMERO
    WHERE A.FECHA_VENTA = vfecha1;

    SELECT SUM(B.CANTIDAD * B.PRECIO) INTO vtotal2
    FROM facturas A INNER JOIN items_facturas B
    ON A.NUMERO = B.NUMERO
    WHERE A.FECHA_VENTA = vfecha2;

    SELECT ROUND(((vtotal2*100)/vtotal1)-100, 2) INTO vtotal;
    
    CASE
    WHEN vtotal > 10 THEN
        SET vresultado = CONCAT('Verde: ', vtotal, '%');
    WHEN vtotal >= -10 AND vtotal <= 10 THEN
        SET vresultado = CONCAT('Amarillo: ', vtotal, '%');
    ELSE
        SET vresultado = CONCAT('Rojo: ', vtotal, '%');
    END CASE;
    SELECT vresultado AS "Porcentaje variación ventas";
END $$

DELIMITER ;
```

```sql
CALL comparacion_ventas('2016-12-31','2017-12-31');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Verde: 28.23%                |
+------------------------------+
```

```sql
CALL comparacion_ventas('2018-03-28','2018-03-27');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Amarillo: -2.2%              |
+------------------------------+
```

```sql
CALL comparacion_ventas('2018-03-28','2015-03-28');
+------------------------------+
| Porcentaje variación ventas  |
+------------------------------+
| Rojo: -41.86%                |
+------------------------------+
```

## WHILE

```sql
WHILE <condition>
    DO <statements>;
END WHILE;
```

```sql
DROP PROCEDURE IF EXISTS looping;
DELIMITER $$
CREATE PROCEDURE looping(vinicial INT, vfinal INT)
BEGIN
    DECLARE vcontador INT; 
    SET vcontador = vinicial;
    CREATE TABLE tb_looping (ID INT);
    WHILE vcontador <= vfinal
    DO
        INSERT INTO tb_looping VALUES(vcontador);
        SET vcontador = vcontador+1;
    END WHILE;
    SELECT * FROM tb_looping;
    DROP TABLE tb_looping;
END $$

DELIMITER ;
```

```sql
CALL looping(1,5);
+------+
| ID   |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
+------+
```

***Crear un SP que, a partir del día `01/01/2017`, cuente el número de***
***facturas hasta el día `10/01/2017`. Debe mostrar la fecha y el número***
***de facturas día tras día***

#### Solución propuesta

```sql
DROP PROCEDURE IF EXISTS loop_date;
DELIMITER $$
CREATE PROCEDURE loop_date(vfecha_inicial DATE, vfecha_final DATE)
BEGIN
    CREATE TEMPORARY TABLE informe (Fecha DATE, Facturas_Emitidas INT);
    WHILE vfecha_inicial <= vfecha_final
    DO
        INSERT INTO informe 
        SELECT fecha_venta, COUNT(numero) FROM facturas
        WHERE fecha_venta = vfecha_inicial;
        SET vfecha_inicial = ADDDATE(vfecha_inicial, INTERVAL 1 DAY);
    END WHILE;
    SELECT * FROM informe;
    DROP TABLE informe;
END $$

DELIMITER ;
```

```sql
CALL loop_date('2017-01-01','2017-01-10');
+------------+-------------------+
| Fecha      | Facturas_Emitidas |
+------------+-------------------+
| 2017-01-01 |                74 |
| 2017-01-02 |                77 |
| 2017-01-03 |                81 |
| 2017-01-04 |                71 |
| 2017-01-05 |                65 |
| 2017-01-06 |                75 |
| 2017-01-07 |                82 |
| 2017-01-08 |                80 |
| 2017-01-09 |                85 |
| 2017-01-10 |                72 |
+------------+-------------------
```

#### Solución

```sql
DROP PROCEDURE IF EXISTS suma_dias_facturas;
DELIMITER $$
CREATE PROCEDURE suma_dias_facturas()
BEGIN
    DECLARE fecha_inicial DATE;
    DECLARE fecha_final DATE;
    DECLARE n_facturas INT;
    SET fecha_inicial = '20170101';
    SET fecha_final = '20170110';
    WHILE fecha_inicial <= fecha_final
    DO
        SELECT COUNT(*) INTO n_facturas FROM facturas
        WHERE FECHA_VENTA = fecha_inicial;
        SELECT concat(
            DATE_FORMAT(fecha_inicial, '%d/%m/%Y'),
            '-' , CAST(n_facturas AS CHAR(50))
        ) AS RESULTADO;
        SELECT ADDDATE(fecha_inicial, INTERVAL 1 DAY) INTO fecha_inicial;
    END WHILE;
END $$
DELIMITER ;
```

## Problemas con SELECT INTO

```sql
DROP PROCEDURE IF EXISTS problema_select_into;
DELIMITER $$
CREATE PROCEDURE problema_select_into()
BEGIN
    DECLARE vnombre VARCHAR(50);
    SELECT nombre INTO vnombre FROM tabla_de_clientes;
    SELECT vnombre;
END $$
DELIMITER ;

CALL problema_select_into();
ERROR 1172 (42000): Result consisted of more than one row
```

## CURSOR

Un **CURSOR** es una estructura que permite la interación línea a línea
mediante un orden determinado

#### Fases para utilizar Cursor

- **Declaración:** Definir la consulta que será depositada en el cursor
- **Apertura:** Abrir el cursor para recorrerlo línea a línea
- **Recorrido:** Línea a línea hasta el final
- **Cierre:** Cierre del cursor
- **Limpieza:** Limpiar el cursor de la memoria

#### Tabla_1

| Codigo | Nombre | Valor |
| - | - | - |
| 1 | Juan | 2 |
| 2 | José | 67 |
| 3 | María | 7 |
| 4 | Lucia | 54 |

```sql
DECLARE @nombre VARCHAR(10)
DECLARE CURSOR_1 CURSOR FOR
    SELECT NOMBRE FROM tabla_1;
    OPEN CURSOR_1;
        FETCH CURSOR_1 INTO @nombre;
        FETCH CURSOR_1 INTO @nombre;
        FETCH CURSOR_1 INTO @nombre;
        FETCH CURSOR_1 INTO @nombre;
    CLOSE CURSOR_1;
```

#### Ejemplo CURSOR

```sql
DROP PROCEDURE IF EXISTS ejm_cursor;
DELIMITER $$
CREATE PROCEDURE ejm_cursor()
BEGIN
    DECLARE vnombre VARCHAR(50);
    DECLARE c CURSOR FOR
    SELECT nombre FROM tabla_de_clientes LIMIT 4;
    OPEN c;
        FETCH c INTO vnombre;
        SELECT vnombre;
        FETCH c INTO vnombre;
        SELECT vnombre;
        FETCH c INTO vnombre;
        SELECT vnombre;
        FETCH c INTO vnombre;
        SELECT vnombre;
    CLOSE c;
END $$
DELIMITER ;
```

```sql
CALL ejm_cursor;
+---------------+
| vnombre       |
+---------------+
| Erica Carvajo |
+---------------+

+--------------+
| vnombre      |
+--------------+
| Marcos Rosas |
+--------------+

+--------------+
| vnombre      |
+--------------+
| Jorge Castro |
+--------------+

+-------------+
| vnombre     |
+-------------+
| Abel Pintos |
+-------------+
```

### Iterando el CURSOR

```sql
DROP PROCEDURE IF EXISTS iter_cursor;
DELIMITER $$
CREATE PROCEDURE iter_cursor()
BEGIN
    DECLARE fin_c BIT(1) DEFAULT b'0';
    DECLARE vnombre VARCHAR(50);
    DECLARE c CURSOR FOR
    SELECT nombre FROM tabla_de_clientes;
    DECLARE CONTINUE HANDLER FOR NOT FOUND
    SET fin_c = b'1';
    OPEN c;
        WHILE fin_c = b'0' DO
            FETCH c INTO vnombre;
            IF fin_c = 0 THEN
                SELECT vnombre;
            END IF;
        END WHILE;
    CLOSE c;
END $$
DELIMITER ;
```

```sql
CALL iter_cursor;
+---------------+
| vnombre       |
+---------------+
| Erica Carvajo |
+---------------+

+--------------+
| vnombre      |
+--------------+
| Marcos Rosas |
+--------------+

+--------------+
| vnombre      |
+--------------+
| Jorge Castro |
+--------------+

...
```

***Crear un SP usando un cursor para hallar el valor total de todos los***
***créditos, de todos los clientes***

```sql
DROP PROCEDURE IF EXISTS total_creditos;
DELIMITER $$
CREATE PROCEDURE total_creditos()
BEGIN
    DECLARE fin_c BIT(1) DEFAULT b'0';
    DECLARE vcred_total FLOAT DEFAULT 0;
    DECLARE vcred_temp FLOAT DEFAULT 0;
    DECLARE c CURSOR FOR
    SELECT limite_de_credito FROM tabla_de_clientes;
    DECLARE CONTINUE HANDLER FOR NOT FOUND
    SET fin_c = b'1';
    OPEN c;
        WHILE fin_c = b'0' DO
            FETCH c INTO vcred_temp;
            IF fin_c = 0 THEN
                SET vcred_total = vcred_total + vcred_temp;
            END IF;
        END WHILE;
    CLOSE c;
    SELECT vcred_total AS "Limite de Credito TOTAL";
END $$
DELIMITER ;
```

```sql
CALL total_creditos;
+-------------------------+
| Limite de Credito TOTAL |
+-------------------------+
|                 1780000 |
+-------------------------+
```

### Asignación de multiples campos al CURSOR

```sql
DROP PROCEDURE IF EXISTS cursor_multicampo;
DELIMITER $$
CREATE PROCEDURE cursor_multicampo()
BEGIN
    DECLARE fin_c BIT(1) DEFAULT b'0';
    DECLARE vbarrio, vciudad, vcodp VARCHAR(50);
    DECLARE vnombre, vdireccion VARCHAR(150);
    DECLARE c CURSOR FOR
        SELECT nombre, direccion_1, barrio, ciudad, cp
        FROM tabla_de_clientes;
    DECLARE CONTINUE HANDLER FOR NOT FOUND
        SET fin_c = b'1';
    OPEN c;
        WHILE fin_c = b'0' DO
            FETCH c INTO vnombre, vdireccion, vbarrio, vciudad, vcodp;
            IF fin_c = b'0' THEN
                SELECT CONCAT(vnombre, ', ', vdireccion, ', ', vbarrio, ', ',
                              vciudad, ', ', vcodp) AS Muticampos;
            END IF;
        END WHILE;
    CLOSE c;
END $$
DELIMITER ;
```

```sql
CALL cursor_multicampo;
+------------------------------------------------------------------------------+
| Muticampos                                                                   |
+------------------------------------------------------------------------------+
| Erica Carvajo, Heriberto Frías 1107, Del Valle, Ciudad de México, 80012212   |
+------------------------------------------------------------------------------+

+-----------------------------------------------------------------------+
| Muticampos                                                            |
+-----------------------------------------------------------------------+
| Marcos Rosas, Av. Universidad, Del Valle, Ciudad de México, 22002012  |
+-----------------------------------------------------------------------+

+---------------------------------------------------------------------------------+
| Muticampos                                                                      |
+---------------------------------------------------------------------------------+
| Jorge Castro, Federal México-Toluca 5690, Locaxco, Ciudad de México, 22012002   |
+---------------------------------------------------------------------------------+

...
```

***Crear un SP usando un cursor para hallar el valor total de la***
***facturación para un determinado mes y año.***

#### Solución propuesta

```sql
DROP PROCEDURE IF EXISTS facturacion_mes_ano;
DELIMITER $$
CREATE PROCEDURE facturacion_mes_ano(ano_mes VARCHAR(10))
BEGIN
    DECLARE fin_c BIT(1) DEFAULT b'0';
    DECLARE vcantidad INT;
    DECLARE vprecio FLOAT;
    DECLARE vtotal FLOAT DEFAULT 0;
    DECLARE vfecha DATE;
    DECLARE c CURSOR FOR
        SELECT IFa.cantidad, IFa.precio
        FROM items_facturas IFa
        INNER JOIN facturas F ON F.numero = IFa.numero
        WHERE MONTH(F.fecha_venta) = MONTH(vfecha)
        AND YEAR(F.fecha_venta) = YEAR(vfecha);
    DECLARE CONTINUE HANDLER FOR NOT FOUND
        SET fin_c = b'1';
    SET vfecha = DATE(CONCAT(ano_mes, '-01'));
    OPEN c;
        WHILE fin_c = b'0' DO
            FETCH c INTO vcantidad, vprecio;
            IF fin_c = b'0' THEN
                SET vtotal = vtotal+(vcantidad*vprecio);
            END IF;
        END WHILE;
    CLOSE c;
    SELECT ano_mes AS "Año-Mes",
        CONCAT('$ ', vtotal, '.-') AS "Venta Total";
END $$
DELIMITER ;
```

```sql
CALL facturacion_mes_ano('2017-01');
+----------+-------------+
| Año-Mes  | Venta Total |
+----------+-------------+
| 2017-01  | $3838320.-  |
+----------+-------------+
```

#### Solución

```sql
DROP PROCEDURE IF EXISTS campo_adicional;
DELIMITER $$
CREATE PROCEDURE campo_adicional()
BEGIN
    DECLARE cantidad INT;
    DECLARE precio FLOAT;
    DECLARE facturacion_acumulada FLOAT;
    DECLARE fin_cursor INT;
    DECLARE c CURSOR FOR
        SELECT IFa.CANTIDAD, IFa.PRECIO FROM items_facturas IFa
        INNER JOIN facturas F ON F.NUMERO = IFa.NUMERO
        WHERE MONTH(F.FECHA_VENTA) = 1
        AND YEAR(F.FECHA_VENTA) = 2017;
    DECLARE CONTINUE HANDLER FOR NOT FOUND 
    SET fin_cursor = 1;
    OPEN c;
        SET fin_cursor = 0;
        SET facturacion_acumulada = 0;
        WHILE fin_cursor = 0 DO
            FETCH c INTO cantidad, precio;
            IF fin_cursor = 0 THEN
                SET facturacion_acumulada =
                facturacion_acumulada + (cantidad * precio);
            END IF;
        END WHILE;
    CLOSE c;
    SELECT facturacion_acumulada;
END $$
DELIMITER ;
```

```sql
CALL campo_adicional;
+-----------------------+
| facturacion_acumulada |
+-----------------------+
|               3838320 |
+-----------------------+
```

## FUNCTION

```sql
CREATE FUNCTION function_name (parameters)
RETURNS datatype;
BEGIN
    DECLARE <declaration_statement>;
    (...)
    <excecutable_statement>;
    (...)
    RETURN <statement>
    (...)
END;
```

```sql
DROP FUNCTION IF EXISTS f_clas_sabores;
DELIMITER $$
CREATE FUNCTION f_clas_sabores(vsabor VARCHAR(20))
RETURNS VARCHAR(40) COLLATE 'utf8mb4_uca1400_as_ci'
BEGIN
    DECLARE vretorno VARCHAR(40) DEFAULT '';
    CASE vsabor
        WHEN 'Maracuya' THEN SET vretorno = 'Rico';
        WHEN 'Limón' THEN SET vretorno = 'Rico';
        WHEN 'Frutilla' THEN SET vretorno = 'Rico';
        WHEN 'Uva' THEN SET vretorno = 'Rico';
        WHEN 'Sandía' THEN SET vretorno = 'Normal';
        WHEN 'Mango' THEN SET vretorno = 'Normal';
        ELSE SET vretorno = 'Común';
    END CASE;
    RETURN vretorno;
END $$
DELIMITER ;
```

```sql
SELECT f_clas_sabores('Sandía') AS "Clasificación";
+----------------+
| Clasificación  |
+----------------+
| Normal         |
+----------------+
```

```sql
SELECT nombre_del_producto, sabor, f_clas_sabores(sabor) AS "Clasificación"
FROM tabla_de_productos;
+---------------------+-----------------+----------------+
| nombre_del_producto | sabor           | Clasificación  |
+---------------------+-----------------+----------------+
| Sabor del Mar       | Naranja         | Común          |
| Sabor del Mar       | Naranja         | Común          |
| Sabor da Montaña    | Uva             | Rico           |
...
```

```sql
SELECT nombre_del_producto, sabor
FROM tabla_de_productos 
WHERE f_clas_sabores(SABOR) = 'Normal';
+---------------------+---------+
| nombre_del_producto | sabor   |
+---------------------+---------+
| Vida del Campo      | Sandía  |
| Light               | Sandía  |
| Verano              | Mango   |
| Refrescante         | Mango   |
| Refrescante         | Mango   |
| Verano              | Mango   |
| Vida del Campo      | Sandía  |
| Refrescante         | Mango   |
| Light               | Sandía  |
+---------------------+---------+
```

***Transformar el sgte. SP en una función que recibe como parámetro la***
***fecha y retorna el número de facturas***

```sql
DELIMITER $$
CREATE PROCEDURE `sp_numero_facturas` ()
BEGIN
    DECLARE n_facturas INT;
    SELECT COUNT(*) INTO n_facturas FROM facturas
    WHERE FECHA_VENTA = '20170101';
    SELECT n_facturas;
END $$
```

```sql
DROP FUNCTION IF EXISTS f_cantidad_facturas;
DELIMITER $$
CREATE FUNCTION f_cantidad_facturas(fecha DATE)
RETURNS INT
BEGIN
    DECLARE n_facturas INT;
    SELECT COUNT(*) INTO n_facturas FROM facturas
    WHERE FECHA_VENTA = fecha;
    RETURN n_facturas;
END $$
DELIMITER ;
```

```sql
SELECT f_cantidad_facturas('2018-01-01');
+-----------------------------------+
| f_cantidad_facturas('2018-01-01') |
+-----------------------------------+
|                                87 |
+-----------------------------------+
```
