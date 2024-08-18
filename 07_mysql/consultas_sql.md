# Consultas SQL
## Cursos
- Curso [Java y JDBC](jdbc.md)
- Curso [JPA Persistence Hibernate](jpa_persistence_hibernate.md)
- Curso [JPA Avanzado](jpa_avanzado.md)
- Curso [Spring Boot 1](spring_boot_1.md)
- Curso [Spring Boot 2](spring_boot_2.md)
- Curso [Spring Boot 3](spring_boot_3.md)
## Lectura
- Lectura [JDBC](https://www.aluracursos.com/blog/conociendo-el-jdbc)
- Lectura [Maven](https://www.aluracursos.com/blog/que-es-maven)
- Lectura [Rest](https://www.aluracursos.com/blog/rest-concepto-y-fundamentos)

Avanzando en SQL con MySQL

### Importar tablas de ejemplo

[alura-repo](https://github.com/alura-es-cursos/1827-consultas-sql-avanzando-en-sql-con-my-sql.git)

> **WARNING:** Cambiar `COLLATE` de `utf8mb4_0900_ai_ci` a `uca1400_as_ci`

```sql
USE jugos_ventas;
```

## Consutas

```sql
DESC tabla_de_clientes;

+---------------------+--------------+------+-----+---------+-------+
| Field               | Type         | Null | Key | Default | Extra |
+---------------------+--------------+------+-----+---------+-------+
| DNI                 | varchar(11)  | NO   | PRI | NULL    |       |
| NOMBRE              | varchar(100) | YES  |     | NULL    |       |
| DIRECCION_1         | varchar(150) | YES  |     | NULL    |       |
| DIRECCION_2         | varchar(150) | YES  |     | NULL    |       |
| BARRIO              | varchar(50)  | YES  |     | NULL    |       |
| CIUDAD              | varchar(50)  | YES  |     | NULL    |       |
| ESTADO              | varchar(2)   | YES  |     | NULL    |       |
| CP                  | varchar(8)   | YES  |     | NULL    |       |
| FECHA_DE_NACIMIENTO | date         | YES  |     | NULL    |       |
| EDAD                | smallint(6)  | YES  |     | NULL    |       |
| SEXO                | varchar(1)   | YES  |     | NULL    |       |
| LIMITE_DE_CREDITO   | float        | YES  |     | NULL    |       |
| VOLUMEN_DE_COMPRA   | float        | YES  |     | NULL    |       |
| PRIMERA_COMPRA      | bit(1)       | YES  |     | NULL    |       |
+---------------------+--------------+------+-----+---------+-------+
14 rows in set (0.005 sec)
```

### Consultar todos los datos

```sql
SELECT dni, nombre, direccion_1, direccion_2, barrio, ciudad, estado, cp,
fecha_de_nacimiento, edad, sexo, limite_de_credito, volumen_de_compra,
primera_compra
```

El equivalente sería `SELECT * FROM tabla_de_clientes;`

### Consultar algunos datos

```sql
SELECT dni, nombre FROM tabla_de_clientes;

SELECT dni AS Identificacion, nombre AS Cliente FROM tabla_de_clientes;

SELECT * FROM tabla_de_productos WHERE SABOR = 'Uva';

SELECT * FROM tabla_de_productos WHERE precio_de_lista < 16;
SELECT * FROM tabla_de_productos WHERE precio_de_lista > 16;
SELECT * FROM tabla_de_productos WHERE precio_de_lista BETWEEN 16 AND 16.02;
```

## Consultas condicionales

[Operadores SQL](https://www.tutorialspoint.com/sql/sql-operators.htm)

### Operadores lógicos

| Operator | Description |
| - | - |
| `ALL` | TRUE if all of a set of comparisons are TRUE |
| `AND` | TRUE if all the conditions separated by AND are TRUE |
| `ANY` | TRUE if any one of a set of comparisons are TRUE |
| `BETWEEN` | TRUE if the operand lies within the range of comparisons |
| `EXISTS` | TRUE if the subquery returns one or more records |
| `IN` | TRUE if the operand is equal to one of a list of expressions |
| `LIKE` | TRUE if the operand matches a pattern specially with wildcard |
| `NOT` | Reverses the value of any other Boolean operator |
| `OR` | TRUE if any of the conditions separated by OR is TRUE
| `IS NULL` | TRUE if the expression value is NULL |
| `SOME` | TRUE if some of a set of comparisons are TRUE |
| `UNIQUE` | The UNIQUE operator searches every row of a specified table for uniqueness (no duplicates) |

- **Operación OR**: El resultado de la operación es verdadero si **alguna** de
sus condiciones es verdadera
- **Operación AND**: El resultado de la operación es verdadero si **todas** sus
condiciones es verdadera
- **Operación NOR(NOT OR)**: Negación de la operación **OR**
- **Operación NAND(NOT AND)**: Negación de la operación **AND**

| Verdadero | Falso |
| :-: | :-: |
| 1 | 0 |

| A | B | OR | AND | NOR | NAND |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | 1 | 1 |
| 0 | 1 | 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 0 | 0 |

### Operadores de comparación

| Operadores | Descripción |
| - | - |
| `=`  | Equal to |
| `!=` | Not equal |
| `<>` | Not equal |
| `>`  | Greater than |
| `<`  | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |
| `!<` | Not less than |
| `!>` | Not greater than |

### Ejemplo

| A | B | A=B | A<=B |
| :-: | :-: | :-: | :-: |
| 3 | 5 | 0 | 1 | 0 | 1 |
| 5 | 23 | 0 | 1 | 0 | 1 |
| 7 | 7 | 1 | 1 | 1 | 1 |
| 2 | 70 | 0 | 1 | 0 | 1 |
| 87 | 85 | 0 | 0 | 0 | 0 |
| 69 | 43 | 0 | 0 | 0 | 0 |
| 21 | 1 | 0 | 0 | 0 | 0 |
| 4 | 2 | 0 | 0 | 0 | 0 |
| 9 | 9 | 1 | 1 | 1 | 1 |

```txt
NOT ((V AND F) OR NOT (F OR F))
NOT ((F) OR NOT (F))
NOT (F OR V)
NOT (V)
F
```

¿La sgte. expresión es Falsa o Verdadera?

```txt
(NOT ((3 > 2) OR (4 >= 5)) AND (5 > 4) ) OR (9 > 0)
```

Respuesta

```txt
(NOT ((3 > 2) OR (4 >= 5)) AND (5 > 4) ) OR (9 > 0)
(NOT ((V) OR (F)) AND (V) ) OR (V)
(NOT V AND V ) OR V
(NOT F) OR V
V OR V
V
```

### Creación de consultas condicionales

```sql
SELECT * FROM tabla_de_productos WHERE sabor='mango' AND tamano='470 ml';
```

```txt
+---------+--------+--------+-------+-------------------+------+
| 1078680 | Verano | 470 ml | Mango | Botella de Vidrio | 5.18 |
+---------+--------+--------+-------+-------------------+------+
```

```sql
SELECT * FROM tabla_de_productos WHERE sabor='mango' OR tamano='470 ml';
```

```txt
+---------------------+---------------------+------------+---------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR   | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+---------+-------------------+-----------------+
| 1051518             | Verano              | 470 ml     | Limón   | Botella de Vidrio |             3.3 |
| 1078680             | Verano              | 470 ml     | Mango   | Botella de Vidrio |            5.18 |
| 1086543             | Refrescante         | 1 Litro    | Mango   | Botella PET       |           11.01 |
...
```

```sql
SELECT * FROM tabla_de_productos WHERE NOT(sabor='mango') OR tamano='470 ml';
```

```txt
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR           | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| 1000889             | Sabor da Montaña    | 700 ml     | Uva             | Botella de Vidrio |            6.31 |
| 1002334             | Línea Citrus        | 1 Litro    | Lima/Limón      | Botella PET       |               7 |
| 1002767             | Vida del Campo      | 700 ml     | Cereza/Manzana  | Botella de Vidrio |            8.41 |
| 1004327             | Vida del Campo      | 1,5 Litros | Sandía          | Botella PET       |           19.51 |
...
```

```sql
SELECT * FROM tabla_de_productos WHERE NOT(sabor='mango' OR tamano='470 ml');
```

```txt
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR           | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| 1000889             | Sabor da Montaña    | 700 ml     | Uva             | Botella de Vidrio |            6.31 |
| 1002334             | Línea Citrus        | 1 Litro    | Lima/Limón      | Botella PET       |               7 |
| 1002767             | Vida del Campo      | 700 ml     | Cereza/Manzana  | Botella de Vidrio |            8.41 |
| 1004327             | Vida del Campo      | 1,5 Litros | Sandía          | Botella PET       |           19.51 |
...
```

```sql
SELECT * FROM tabla_de_productos WHERE NOT(sabor='mango' AND tamano='470 ml');
```

```txt
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR           | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+-----------------+-------------------+-----------------+
| 1000889             | Sabor da Montaña    | 700 ml     | Uva             | Botella de Vidrio |            6.31 |
| 1002334             | Línea Citrus        | 1 Litro    | Lima/Limón      | Botella PET       |               7 |
| 1002767             | Vida del Campo      | 700 ml     | Cereza/Manzana  | Botella de Vidrio |            8.41 |
| 1004327             | Vida del Campo      | 1,5 Litros | Sandía          | Botella PET       |           19.51 |
| 1013793             | Vida del Campo      | 2 Litros   | Cereza/Manzana  | Botella PET       |           24.01 |
...
```


```sql
SELECT * FROM tabla_de_productos WHERE sabor='mango' AND NOT (tamano='470 ml');
```

```txt
+---------------------+---------------------+------------+-------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+-------+-------------------+-----------------+
| 1086543             | Refrescante         | 1 Litro    | Mango | Botella PET       |           11.01 |
| 1096818             | Refrescante         | 700 ml     | Mango | Botella de Vidrio |            7.71 |
| 235653              | Verano              | 350 ml     | Mango | Lata              |            3.86 |
| 326779              | Refrescante         | 1,5 Litros | Mango | Botella PET       |           16.51 |
+---------------------+---------------------+------------+-------+-------------------+-----------------+
```

### IN

```sql
SELECT * FROM tabla_de_productos WHERE sabor IN ('mango', 'uva');
```

```
+---------------------+---------------------+------------+-------+-------------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR | ENVASE            | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+-------+-------------------+-----------------+
| 1000889             | Sabor da Montaña    | 700 ml     | Uva   | Botella de Vidrio |            6.31 |
| 1078680             | Verano              | 470 ml     | Mango | Botella de Vidrio |            5.18 |
| 1086543             | Refrescante         | 1 Litro    | Mango | Botella PET       |           11.01 |
| 1096818             | Refrescante         | 700 ml     | Mango | Botella de Vidrio |            7.71 |
| 235653              | Verano              | 350 ml     | Mango | Lata              |            3.86 |
| 326779              | Refrescante         | 1,5 Litros | Mango | Botella PET       |           16.51 |
+---------------------+---------------------+------------+-------+-------------------+-----------------+
```

```sql
SELECT nombre, direccion_1, ciudad, fecha_de_nacimiento as nacimiento, edad
    FROM tabla_de_clientes 
    WHERE ciudad IN ('ciudad de México', 'Guadalajara')
    AND (edad BETWEEN 20 AND 25);
```

```txt
+----------------+-----------------------------+-------------------+------------+------+
| NOMBRE         | DIRECCION_1                 | CIUDAD            | NACIMIENTO | EDAD |
+----------------+-----------------------------+-------------------+------------+------+
| Abel Pintos    | Carr. México-Toluca 1499    | Ciudad de México  | 1995-06-11 |   25 |
| Joana Olivera  | Pachuca 75                  | Ciudad de México  | 1995-02-14 |   25 |
| Luis Silva     | Prol. 16 de Septiembre 1156 | Ciudad de México  | 1995-04-07 |   25 |
| Edson Calisaya | Sta Úrsula Xitla            | Ciudad de México  | 1995-01-07 |   25 |
+----------------+-----------------------------+-------------------+------------+------+
```

## LIKE

```sql
SELECT * FROM tb WHERE campo LIKE ´%<condición´;
```

- `<condición>` el texto utilizado
- `%` Representa cualquier registro genérico antes de la condición (comodín)

### Ejemplo

| NOMBRE |
| :- |
| Miguel Suárez Diaz |
| Raul José Suárez |
| Manuela Diaz Avendaño |
| Mario García Rojas |
| Carlos Santiago Pérez |
| Daniela Suárez |
| Pedro González |
| Pablo Restrepo Villa |
| José Manuel Sánchez |

```sql
SELECT * FROM tb WHERE campo LIKE ´%suárez%´;
```

Retornaría: `Miguel Suárez Diaz`, `Raul José Suárez` y `Daniela Suárez`

```sql
SELECT * FROM tb WHERE campo LIKE ´%suárez´;
```

Retornaría: `Raul José Suárez` y `Daniela Suárez`

```sql
SELECT * FROM tabla_de_productos WHERE sabor LIKE '%manzana%' AND envase = 'botella pet';
```

```sql
+---------------------+---------------------+------------+----------------+-------------+-----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR          | ENVASE      | PRECIO_DE_LISTA |
+---------------------+---------------------+------------+----------------+-------------+-----------------+
| 1013793             | Vida del Campo      | 2 Litros   | Cereza/Manzana | Botella PET |           24.01 |
| 520380              | Pedazos de Frutas   | 1 Litro    | Manzana        | Botella PET |           12.01 |
| 788975              | Pedazos de Frutas   | 1,5 Litros | Manzana        | Botella PET |           18.01 |
+---------------------+---------------------+------------+----------------+-------------+-----------------+
```

## DISTINCT

Devuelve solo registros con valores diferentes

| CAMPO_1 | CAMPO_2 |
| :-: | :-: |
| A | B |
| Z | C |
| Z | Q |
| A | B |
| E | R |
| T | E |
| Z | C |
| Z | Q |
| T | E |


```sql
SELECT DISTINCT * FROM tb;
```

Retorna:

| CAMPO_1 | CAMPO_2 |
| :-: | :-: |
| A | B |
| Z | C |
| Z | Q |
| E | R |
| T | E |

```sql
SELECT DISTINCT envase, tamano FROM tabla_de_productos;
```

```txt
+-------------------+------------+
| envase            | tamano     |
+-------------------+------------+
| Botella de Vidrio | 700 ml     |
| Botella PET       | 1 Litro    |
| Botella PET       | 1,5 Litros |
| Botella PET       | 2 Litros   |
| Lata              | 350 ml     |
| Botella de Vidrio | 470 ml     |
+-------------------+------------+
```

```sql
SELECT DISTINCT envase, tamano, sabor FROM tabla_de_productos
    -> WHERE sabor = 'naranja';
```

```txt
+-------------------+------------+---------+
| envase            | tamano     | sabor   |
+-------------------+------------+---------+
| Botella PET       | 2 Litros   | Naranja |
| Botella de Vidrio | 470 ml     | Naranja |
| Botella PET       | 1 Litro    | Naranja |
| Lata              | 350 ml     | Naranja |
| Botella PET       | 1,5 Litros | Naranja |
+-------------------+------------+---------+
```

## LIMIT

Limita el númeo de registros devueltos

Ejm. limitar a 3 resultados

```sql
SELECT * FROM tb LIMIT 3;
```

Ejm. limitar a 3 resultados, a partir de indice 2

```sql
SELECT * FROM tb LIMIT 2,2;
```

```sql
SELECT codigo_del_producto, nombre_del_producto, tamano, sabor 
    FROM tabla_de_productos
    LIMIT 5;
```

```txt
+---------------------+---------------------+------------+----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR          |
+---------------------+---------------------+------------+----------------+
| 1000889             | Sabor da Montaña    | 700 ml     | Uva            |
| 1002334             | Línea Citrus        | 1 Litro    | Lima/Limón     |
| 1002767             | Vida del Campo      | 700 ml     | Cereza/Manzana |
| 1004327             | Vida del Campo      | 1,5 Litros | Sandía         |
| 1013793             | Vida del Campo      | 2 Litros   | Cereza/Manzana |
+---------------------+---------------------+------------+----------------+
```


```sql
SELECT codigo_del_producto, nombre_del_producto, tamano, sabor 
    FROM tabla_de_productos
    LIMIT 2,5;
```

```txt
+---------------------+---------------------+------------+----------------+
| CODIGO_DEL_PRODUCTO | NOMBRE_DEL_PRODUCTO | TAMANO     | SABOR          |
+---------------------+---------------------+------------+----------------+
| 1002767             | Vida del Campo      | 700 ml     | Cereza/Manzana |
| 1004327             | Vida del Campo      | 1,5 Litros | Sandía         |
| 1013793             | Vida del Campo      | 2 Litros   | Cereza/Manzana |
| 1022450             | Festival de Sabores | 2 Litros   | Asái           |
| 1037797             | Clean               | 2 Litros   | Naranja        |
+---------------------+---------------------+------------+----------------+
```

```sql
SELECT * FROM facturas WHERE fecha_venta = '2017/01/01' LIMIT 10;
```

```txt
+-------------+-----------+-------------+--------+----------+
| DNI         | MATRICULA | FECHA_VENTA | NUMERO | IMPUESTO |
+-------------+-----------+-------------+--------+----------+
| 9283760794  | 00235     | 2017-01-01  |  54476 |     0.12 |
| 50534475787 | 00237     | 2017-01-01  |  54477 |     0.12 |
| 492472718   | 00235     | 2017-01-01  |  54478 |     0.12 |
| 3623344710  | 00235     | 2017-01-01  |  54479 |     0.12 |
| 94387575700 | 00236     | 2017-01-01  |  54480 |      0.1 |
| 94387575700 | 00235     | 2017-01-01  |  54481 |     0.12 |
| 9283760794  | 00237     | 2017-01-01  |  54482 |      0.1 |
| 2600586709  | 00235     | 2017-01-01  |  54483 |      0.1 |
| 9283760794  | 00235     | 2017-01-01  |  54484 |      0.1 |
| 5576228758  | 00237     | 2017-01-01  |  54485 |     0.12 |
+-------------+-----------+-------------+--------+----------+
```

## ORDER BY

```sql
SELECT codigo_del_producto AS codigo,
       nombre_del_producto AS nombre,
       precio_de_lista AS precio
    FROM tabla_de_productos
    ORDER BY precio_de_lista DESC
    DESC LIMIT 10;
```

```txt
+---------+---------------------+--------+
| codigo  | nombre              | precio |
+---------+---------------------+--------+
| 1022450 | Festival de Sabores |  38.01 |
| 695594  | Festival de Sabores |  28.51 |
| 1013793 | Vida del Campo      |  24.01 |
| 746596  | Light               |  19.51 |
| 1004327 | Vida del Campo      |  19.51 |
| 788975  | Pedazos de Frutas   |  18.01 |
| 326779  | Refrescante         |  16.51 |
| 1037797 | Clean               |  16.01 |
| 231776  | Festival de Sabores |  13.31 |
| 838819  | Clean               |  12.01 |
+---------+---------------------+--------+
```

```sql
SELECT codigo_del_producto AS codigo,
       nombre_del_producto AS nombre,
       precio_de_lista AS precio
    FROM tabla_de_productos
    ORDER BY precio_de_lista ASC,
             nombre_del_producto DESC
    DESC LIMIT 10;
```

```txt
+---------+-------------------+--------+
| codigo  | nombre            | precio |
+---------+-------------------+--------+
| 544931  | Verano            |   2.46 |
| 812829  | Clean             |   2.81 |
| 1051518 | Verano            |    3.3 |
| 479745  | Clean             |   3.77 |
| 235653  | Verano            |   3.86 |
| 229900  | Pedazos de Frutas |   4.21 |
| 290478  | Vida del Campo    |   4.56 |
| 1040107 | Light             |   4.56 |
| 1041119 | Línea Citrus      |    4.9 |
| 1042712 | Línea Citrus      |    4.9 |
+---------+-------------------+--------+
```

***¿Cuál (o cuáles) fue (fueron) la(s) mayor(es) venta(s) del producto
“Refrescante, 1 Litro, Frutilla/Limón”, en cantidad?***

```sql
SELECT numero, codigo_del_producto, cantidad, precio, cantidad*precio as total
    FROM items_facturas WHERE codigo_del_producto IN (
        SELECT codigo_del_producto
            FROM tabla_de_productos 
            WHERE nombre_del_producto = 'refrescante'
                AND tamano = '1 litro'
                AND sabor = 'frutilla/limón'
    )
ORDER BY total DESC LIMIT 10;
```

```txt
+--------+---------------------+----------+---------+--------------------+
| numero | codigo_del_producto | cantidad | precio  | total              |
+--------+---------------------+----------+---------+--------------------+
|  83710 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  85301 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  87424 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  84674 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  85905 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  83818 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  84389 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  87209 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  81740 | 1101035             |       99 | 10.6324 | 1052.6075563430786 |
|  81765 | 1101035             |       98 | 10.6324 | 1041.9751567840576 |
+--------+---------------------+----------+---------+--------------------+
```

Analisis de la query

```sql
ANALYZE SELECT numero, codigo_del_producto, cantidad, precio, cantidad*precio as total
    FROM items_facturas WHERE codigo_del_producto IN (
        SELECT codigo_del_producto
            FROM tabla_de_productos 
            WHERE nombre_del_producto = 'refrescante'
                AND tamano = '1 litro'
                AND sabor = 'frutilla/limón'
    )
ORDER BY total DESC LIMIT 10;

+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+---------+----------+------------+----------------------------------------------+
| id   | select_type | table              | type | possible_keys       | key                 | key_len | ref                                                 | rows | r_rows  | filtered | r_filtered | Extra                                        |
+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+---------+----------+------------+----------------------------------------------+
|    1 | PRIMARY     | tabla_de_productos | ALL  | PRIMARY             | NULL                | NULL    | NULL                                                | 35   | 35.00   |   100.00 |       2.86 | Using where; Using temporary; Using filesort |
|    1 | PRIMARY     | items_facturas     | ref  | CODIGO_DEL_PRODUCTO | CODIGO_DEL_PRODUCTO | 42      | jugos_ventas.tabla_de_productos.CODIGO_DEL_PRODUCTO | 2743 | 7103.00 |   100.00 |     100.00 |                                              |
+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+---------+----------+------------+----------------------------------------------+
```

```sql
DESCRIBE SELECT numero, codigo_del_producto, cantidad, precio, cantidad*precio as total
    FROM items_facturas WHERE codigo_del_producto IN (
        SELECT codigo_del_producto
            FROM tabla_de_productos 
            WHERE nombre_del_producto = 'refrescante'
                AND tamano = '1 litro'
                AND sabor = 'frutilla/limón'
    )
ORDER BY total DESC LIMIT 10;
+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+----------------------------------------------+
| id   | select_type | table              | type | possible_keys       | key                 | key_len | ref                                                 | rows | Extra                                        |
+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+----------------------------------------------+
|    1 | PRIMARY     | tabla_de_productos | ALL  | PRIMARY             | NULL                | NULL    | NULL                                                | 35   | Using where; Using temporary; Using filesort |
|    1 | PRIMARY     | items_facturas     | ref  | CODIGO_DEL_PRODUCTO | CODIGO_DEL_PRODUCTO | 42      | jugos_ventas.tabla_de_productos.CODIGO_DEL_PRODUCTO | 2743 |                                              |
+------+-------------+--------------------+------+---------------------+---------------------+---------+-----------------------------------------------------+------+----------------------------------------------+
```

## GROUP BY

| X | Y |
| :-: | :-: |
| A | 3 |
| Z | 5 |
| Z | 1 |
| A | 1 |
| E | 4 |
| T | 3 |
| Z | 8 |
| Z | 2 |
| T | 1 |

```sql
SELECT x, SUM(y) FROM tb GROUP BY X;
```

| X | Y |
| :-: | :-: |
| A | 4 |
| E | 4 |
| T | 4 |
| Z | 16 |

```sql
SELECT x, SUM(y) FROM tb;
28
```

### Funciones

Omitiendo el campo de agregación la operación se realiza sobre toda la tabla

| Funcion | Operación |
| - | - |
| `SUM()` | Suma |
| `MAX()` | Máximo |
| `MIN()` | Mínimo |
| `AVG()` | Promedio |
| `COUNT()` | Contador |

#### MAX

```sql
SELECT x, MAX(y) FROM tb GROUP BY X;
```

| X | Y |
| :-: | :-: |
| A | 3 |
| E | 4 |
| T | 3 |
| Z | 8 |

```sql
SELECT x, MAX(y) FROM tb;
8
```

#### MIN

```sql
SELECT x, MIN(y) FROM tb GROUP BY X;
```

| X | Y |
| :-: | :-: |
| A | 1 |
| E | 4 |
| T | 1 |
| Z | 1 |

```sql
SELECT x, MIN(y) FROM tb;
1
```

#### AVG

```sql
SELECT x, AVG(y) FROM tb GROUP BY X;
```

| X | Y |
| :-: | :-: |
| A | 2 |
| E | 4 |
| T | 2 |
| Z | 4 |

```sql
SELECT x, AVG(y) FROM tb GROUP BY X;
3.111
```

#### COUNT

```sql
SELECT x, COUNT(y) FROM tb GROUP BY X;
```

| X | Y |
| :-: | :-: |
| A | 2 |
| E | 1 |
| T | 2 |
| Z | 4 |

```sql
SELECT x, COUNT(y) FROM tb;
9
```

### Pruebas en la BD

```sql
SELECT estado, SUM(limite_de_credito) AS limite_total
    FROM tabla_de_clientes
    GROUP BY estado;
```

```txt
+--------+--------------+
| estado | limite_total |
+--------+--------------+
| EM     |      1495000 |
| JC     |       285000 |
+--------+--------------+
```

```sql
SELECT envase, MAX(precio_de_lista) AS precio_mayor
    FROM tabla_de_productos
    GROUP BY envase;
```

```txt
+-------------------+--------------+
| envase            | precio_mayor |
+-------------------+--------------+
| Botella de Vidrio |        13.31 |
| Botella PET       |        38.01 |
| Lata              |         4.56 |
+-------------------+--------------+
```

```sql
SELECT envase, COUNT(*)
    FROM tabla_de_productos
    GROUP BY envase;
```

```txt
+-------------------+----------+
| envase            | COUNT(*) |
+-------------------+----------+
| Botella de Vidrio |       13 |
| Botella PET       |       16 |
| Lata              |        6 |
+-------------------+----------+
```

```sql
SELECT barrio, SUM(limite_de_credito) AS limite
    FROM tabla_de_clientes
    WHERE ciudad = 'ciudad de méxico'
    GROUP BY barrio;
```

```txt
+-------------------------+--------+
| barrio                  | limite |
+-------------------------+--------+
| Barrio del Niño Jesús   | 150000 |
| Carola                  | 120000 |
| Condesa                 |  70000 |
| Contadero               | 110000 |
| Cuajimalpa              | 170000 |
| Del Valle               | 420000 |
| Ex Hacienda Coapa       |  60000 |
| Floresta Coyoacán       | 200000 |
| Héroes de Padierna      | 120000 |
| Locaxco                 |  75000 |
+-------------------------+--------+
```

```sql
SELECT estado, barrio, MAX(limite_de_credito) AS limite, edad
    FROM tabla_de_clientes
    WHERE edad >= 20
    GROUP BY estado, barrio
    ORDER BY edad;
```

```txt
+--------+-------------------------+--------+------+
| estado | barrio                  | limite | edad |
+--------+-------------------------+--------+------+
| EM     | Contadero               | 110000 |   25 |
| EM     | Condesa                 |  70000 |   25 |
| EM     | Cuajimalpa              | 170000 |   25 |
| EM     | Barrio del Niño Jesús   | 150000 |   25 |
| JC     | Barragán Hernández      | 120000 |   26 |
| EM     | Locaxco                 |  75000 |   26 |
| JC     | Oblatos                 |  75000 |   26 |
| EM     | Carola                  | 120000 |   29 |
| EM     | Héroes de Padierna      | 120000 |   29 |
| EM     | Del Valle               | 170000 |   30 |
| EM     | Ex Hacienda Coapa       |  60000 |   31 |
| EM     | Floresta Coyoacán       | 200000 |   37 |
+--------+-------------------------+--------+------+
```

***¿ Cuantas facturas existen que tengan la mayor cantidad del producto '1101035' ?***

```sql
SELECT COUNT(*) FROM items_facturas
    WHERE cantidad=(
        SELECT MAX(CANTIDAD)
        FROM items_facturas
        WHERE codigo_del_producto = '1101035'
    )
    and codigo_del_producto='1101035';
```

```txt
+----------+
| COUNT(*) |
+----------+
|       79 |
+----------+
```

## HAVING

Filtro que se aplica sobre el resultado de una agregación

| X | Y |
| :-: | :-: |
| A | 3 |
| Z | 5 |
| Z | 1 |
| A | 1 |
| E | 4 |
| T | 3 |
| Z | 8 |
| Z | 2 |
| T | 1 |

```sql
SELECT x, SUM(y) FROM tb
    GROUP BY x
    HAVING SUM(y)>4;
```

| X | Y |
| :-: | :-: |
| Z | 16 |


```sql
SELECT estado, SUM(limite_de_credito) AS limite_total
    FROM tabla_de_clientes
    GROUP BY estado
    HAVING limite_total>500000;
```

```txt
+--------+--------------+
| estado | limite_total |
+--------+--------------+
| EM     |      1495000 |
+--------+--------------+
```

```sql
SELECT envase, MAX(precio_de_lista) AS precio_mayor,
MIN(precio_de_lista) AS precio_menor
    FROM tabla_de_productos
    GROUP BY envase
    HAVING SUM(precio_de_lista)>80;
```

```txt
+-------------------+--------------+--------------+
| envase            | precio_mayor | precio_menor |
+-------------------+--------------+--------------+
| Botella de Vidrio |        13.31 |          3.3 |
| Botella PET       |        38.01 |            7 |
+-------------------+--------------+--------------+
```

```sql
SELECT envase, MAX(precio_de_lista) AS precio_mayor,
MIN(precio_de_lista) AS precio_menor,
ROUND(SUM(precio_de_lista), 2)
    FROM tabla_de_productos
    GROUP BY envase
    HAVING SUM(precio_de_lista)>80;
```

```txt
+-------------------+--------------+--------------+-------------+
| envase            | precio_mayor | precio_menor | suma_precio |
+-------------------+--------------+--------------+-------------+
| Botella de Vidrio |        13.31 |          3.3 |       85.13 |
| Botella PET       |        38.01 |            7 |      256.64 |
+-------------------+--------------+--------------+-------------+
```

```sql
SELECT envase, MAX(precio_de_lista) AS precio_mayor,
MIN(precio_de_lista) AS precio_menor,
ROUND(SUM(precio_de_lista), 2)
    FROM tabla_de_productos
    GROUP BY envase
    HAVING SUM(precio_de_lista)>=80
        AND precio_mayor >=38;
```

```txt
+-------------+--------------+--------------+-------------+
| envase      | precio_mayor | precio_menor | suma_precio |
+-------------+--------------+--------------+-------------+
| Botella PET |        38.01 |            7 |      256.64 |
+-------------+--------------+--------------+-------------+
```

***¿ Cuales fueron los clientes que hicieron mas de 2000 compras en el 2016 ?***

```sql
SELECT dni, COUNT(*) FROM facturas
    WHERE YEAR(fecha_venta) = 2016
    GROUP BY dni
    HAVING COUNT(*)>2000;
```

```txt
+-------------+----------+
| dni         | COUNT(*) |
+-------------+----------+
| 3623344710  |     2012 |
| 492472718   |     2008 |
| 50534475787 |     2037 |
+-------------+----------+
```

## CASE

```sql
CASE
    WHEN <condicion_1> THEN <valor_1>
    WHEN <condicion_2> THEN <valor_2>
    ...
    WHEN <condicion_n> THEN <valor_n>
    ELSE <valor_por_defecto>
```

ej. Tabla `tb`

| X | Y |
| :-: | :-: |
| Cliente_1 | 8 |
| Cliente_2 | 6 |
| Cliente_3 | 3 |
| Cliente_4 | 10 |
| Cliente_5 | 5 |
| Cliente_6 | 7 |
| Cliente_7 | 1 |
| Cliente_8 | 2 |
| Cliente_9 | 1 |

```sql
CASE
    WHEN y>=8 AND Y<=10 THEN 'Muy bueno'
    WHEN y>=7 AND Y<8 THEN 'Bueno'
    WHEN y>=5 AND Y<7 THEN 'Regular'
    ELSE 'Inferior'
END
FROM tb;
```

Resultado:

| X | Y |
| :-: | :-: |
| Cliente_1 | Muy bueno |
| Cliente_2 | Regular |
| Cliente_3 | Inferior |
| Cliente_4 | Muy bueno |
| Cliente_5 | Regular |
| Cliente_6 | Bueno |
| Cliente_7 | Inferior  |
| Cliente_8 | Inferior  |
| Cliente_9 | Inferior  |


```sql
SELECT nombre_del_producto, precio_de_lista,
CASE
    WHEN precio_de_lista >= 12 THEN 'Caro'
    WHEN precio_de_lista >= 5 AND precio_de_lista < 12 THEN 'Asequible'
    ELSE 'Barato'
END as precio
    FROM tabla_de_productos;
```

```txt
+---------------------+-----------------+-----------+
| nombre_del_producto | precio_de_lista | precio    |
+---------------------+-----------------+-----------+
| Sabor da Montaña    |            6.31 | Asequible |
| Línea Citrus        |               7 | Asequible |
| Vida del Campo      |            8.41 | Asequible |
| Vida del Campo      |           19.51 | Caro      |
| Vida del Campo      |           24.01 | Caro      |
| Festival de Sabores |           38.01 | Caro      |
| Clean               |           16.01 | Caro      |
| Light               |            4.56 | Barato    |
| Línea Citrus        |             4.9 | Barato    |
...
```

```sql
SELECT envase, sabor,
CASE
    WHEN precio_de_lista >= 12 THEN 'Caro'
    WHEN precio_de_lista >= 5 AND precio_de_lista < 12 THEN 'Asequible'
    ELSE 'Barato'
END as precio, MIN(precio_de_lista) AS precio_minimo
FROM tabla_de_productos
    WHERE tamano = '700 ml'
        GROUP BY envase,
        CASE
            WHEN precio_de_lista >= 12 THEN 'Caro'
            WHEN precio_de_lista >= 5 AND precio_de_lista < 12 THEN 'Asequible'
            ELSE 'Barato'
        END
        ORDER BY envase;
```

```txt
+-------------------+-------------+-----------+---------------+
| envase            | sabor       | precio    | precio_minimo |
+-------------------+-------------+-----------+---------------+
| Botella de Vidrio | Uva         | Asequible |          6.31 |
| Botella de Vidrio | Lima/Limón  | Barato    |           4.9 |
| Botella de Vidrio | Asaí        | Caro      |         13.31 |
+-------------------+-------------+-----------+---------------+
```

***Registrar el año de nacimiento de los clientes y clasifícar según:***

- Nacidos antes de 1990 = Mayores
- Nacidos entre 1990 y 1995= Adultos
- Nacidos después de 1995 = Jovenes

***Listar el nombre del cliente y la clasificación***

```sql
SELECT nombre,
  CASE
    WHEN YEAR(fecha_de_nacimiento) < 1990 THEN 'Mayor'
    WHEN YEAR(fecha_de_nacimiento) >= 1990
        AND YEAR(fecha_de_nacimiento) <= 1995 THEN 'Adulto'
    ELSE 'Joven'
  END AS rango_etario
  FROM tabla_de_clientes;
```

```txt
+--------------------+--------------+
| nombre             | rango_etario |
+--------------------+--------------+
| Erica Carvajo      | Adulto       |
| Marcos Rosas       | Adulto       |
| Jorge Castro       | Adulto       |
| Abel Pintos        | Adulto       |
| Joana Olivera      | Adulto       |
| Paolo Mendez       | Adulto       |
| Gabriel Roca       | Mayor        |
| Marcelo Perez      | Adulto       |
| Luis Silva         | Adulto       |
| Carlos Santivañez  | Mayor        |
| Alberto Rodriguez  | Adulto       |
| Edson Calisaya     | Adulto       |
| María Jimenez      | Adulto       |
| Walter Soruco      | Mayor        |
| Ximena Gómez       | Adulto       |
+--------------------+--------------+
```

## JOIN

Permite unir dos o más tablas a través de un campo en común

### Tabla A

| Nombre | Id |
| :-: | :-: |
| Alejandro | 2 |
| Zaida | 7 |
| Ximena | 8 |
| Elías | 10 |
| Tatiana | 15 |
| Penélope | 9 |

### Tabla B

| Id | Hobby |
| :-: | :-: |
| 4 | Lectura |
| 5 | Futbol |
| 6 | Tenis |
| 7 | Alpinismo |
| 8 | Fotografía |
| 9 | Hípica |

### INNER JOIN

Devuelve únicamente los registros con llaves correspondientes

```sql
SELECT A.nombre, B.hobby FROM tabla_izq A
INNER JOIN
    tabla_der B
    ON A.id = B.id
```

| Nombre | Hobby |
| :-: | :-: |
| Zaida | Alpinismo |
| Ximena | Fotografía |
| Penélope | Hípica |

### LEFT JOIN

Maniene todos los registros de la tabla izquierda **A** y devuelve únicamente
los correspondientes con la tabla de la derecha **B**

```sql
SELECT A.nombre, B.hobby FROM tabla_izq A
LEFT JOIN
    tabla_der B
    ON A.id = B.id
```

| Nombre | Hobby |
| :-: | :-: |
| Alejandro | `NULL` |
| Zaida | Alpinismo |
| Ximena | Fotografía |
| Elías | `NULL` |
| Tatiana | `NULL` |
| Penélope | Hípica |

### RIGHT JOIN

Maniene todos los registros de la tabla derecha **B** y devuelve únicamente los
correspondientes con la tabla de la izquierda **A**

```sql
SELECT A.nombre, B.hobby FROM tabla_izq A
RIGHT JOIN
    tabla_der B
    ON A.id = B.id
```

| Nombre | Hobby |
| :-: | :-: |
| `NULL`| Lectura |
| `NULL` | Futbol |
| `NULL` | Tenis |
| Zaida | Alpinismo |
| Ximena | Fotografía |
| Penélope | Hípica |

### FULL JOIN

Maniene todos los registros de las tablas

```sql
SELECT A.nombre, B.hobby FROM tabla_izq A
FULL JOIN
    tabla_der B
    ON A.id = B.id
```

| Nombre | Hobby |
| :-: | :-: |
| `NULL`| Lectura |
| `NULL` | Futbol |
| `NULL` | Tenis |
| Zaida | Alpinismo |
| Ximena | Fotografía |
| Penélope | Hípica |
| Alejandro | `NULL` |
| Elías | `NULL` |
| Tatiana | `NULL` |

### CROSS JOIN

Devuelve el prodcuto cartesiano de los registros de las tablas

```sql
SELECT A.nombre, B.hobby FROM tabla_izq A, tabla_der B
```

Devuelve 36 registros con todas las combinaciónes de todos los hobbies y nombres


| Nombre | Hobby |
| :-: | :-: |
| Alejandro | Lectura |
| Zaida | Lectura |
| Ximena | Lectura |
| Elías | Lectura |
| Tatiana | Lectura |
| Penélope | Lectura |
| Alejandro | Futbol |
| Zaida | Futbol |
| Ximena | Futbol |
| ... | ... |

### Prácticas JOIN

#### Práctica INNER JOIN

```sql
SELECT A.nombre, B.matricula, COUNT(*)
FROM tabla_de_vendedores A
INNER JOIN
    facturas B
    ON A.matricula = B.matricula
    GROUP BY A.nombre, B.matricula;
```

```txt
+----------------------+-----------+----------+
| nombre               | matricula | COUNT(*) |
+----------------------+-----------+----------+
| Claudia Morales      | 00236     |    29375 |
| Concepción Martinez  | 00237     |    29113 |
| Miguel Pavón Silva   | 00235     |    29389 |
+----------------------+-----------+----------+
```

***Obtén la facturación anual de la empresa. Ten en cuenta que el valor
financiero de las ventas consiste en multiplicar la cantidad por el precio.***

Tablas y campos de interes

- `facturas`: `fecha_venta` y `numero`
- `items_facturas`: `numero`, `cantidad` y `precio`

```sql
SELECT YEAR(fecha_venta) as Periodo,
ROUND(SUM(cantidad*precio), 3) AS Facturacion
FROM facturas F
    INNER JOIN
        items_facturas IFa
        ON F.numero = IFa.numero
        GROUP BY Periodo
    ORDER BY Periodo DESC;
```

```txt
+---------+--------------+
| Periodo | Facturacion  |
+---------+--------------+
|    2018 | 11022282.826 |
|    2017 | 44359013.133 |
|    2016 | 42362119.436 |
|    2015 | 39848262.063 |
+---------+--------------+
```


#### Práctica LEFT y RIGHT JOIN

Clientes con compras con INNER JOIN

```sql
SELECT DISTINCT A.dni, A.nombre, B.dni
FROM tabla_de_clientes A
    INNER JOIN
    facturas B
    ON A.dni = B.dni;
```

```txt
+-------------+--------------------+-------------+
| dni         | nombre             | dni         |
+-------------+--------------------+-------------+
| 1471156710  | Erica Carvajo      | 1471156710  |
| 3623344710  | Marcos Rosas       | 3623344710  |
| 492472718   | Jorge Castro       | 492472718   |
| 50534475787 | Abel Pintos        | 50534475787 |
| 5576228758  | Joana Olivera      | 5576228758  |
| 5648641702  | Paolo Mendez       | 5648641702  |
| 5840119709  | Gabriel Roca       | 5840119709  |
| 7771579779  | Marcelo Perez      | 7771579779  |
| 8502682733  | Luis Silva         | 8502682733  |
| 8719655770  | Carlos Santivañez  | 8719655770  |
| 9283760794  | Edson Calisaya     | 9283760794  |
| 94387575700 | María Jimenez      | 94387575700 |
+-------------+--------------------+-------------+
```

Clientes con compras con **LEFT JOIN** y condición para encontrar clientes sin
compras

```sql
SELECT DISTINCT A.dni, A.nombre, B.dni
FROM tabla_de_clientes A
    LEFT JOIN
    facturas B
    ON A.dni = B.dni;
    WHERE B.dni IS NULL;
```

```txt
+-------------+-------------------+------+
| dni         | nombre            | dni  |
+-------------+-------------------+------+
| 9275760794  | Alberto Rodriguez | NULL |
| 94387591700 | Walter Soruco     | NULL |
| 95939180787 | Ximena Gómez      | NULL |
+-------------+-------------------+------+
```

Clientes con compras con **RIGHT JOIN** y condición para encontrar clientes sin
compras

```sql
SELECT DISTINCT A.dni, B.dni, B.nombre
FROM facturas A
    RIGHT JOIN tabla_de_clientes B
    ON A.dni = B.dni
    WHERE A.dni IS NULL;
```

```txt
+------+-------------+-------------------+
| dni  | dni         | nombre            |
+------+-------------+-------------------+
| NULL | 9275760794  | Alberto Rodriguez |
| NULL | 94387591700 | Walter Soruco     |
| NULL | 95939180787 | Ximena Gómez      |
+------+-------------+-------------------+
```

#### Práctica CROSS e INNER JOIN

```sql
SELECT A.nombre, B.matricula, COUNT(*)
FROM tabla_de_vendedores A, facturas B
    WHERE A.matricula = B.matricula
    GROUP BY A.nombre, B.matricula;
```

```txt
+----------------------+-----------+----------+
| nombre               | matricula | COUNT(*) |
+----------------------+-----------+----------+
| Claudia Morales      | 00236     |    29375 |
| Concepción Martinez  | 00237     |    29113 |
| Miguel Pavón Silva   | 00235     |    29389 |
+----------------------+-----------+----------+
```

```sql
SELECT tabla_de_clientes.nombre AS nombre_cliente,
tabla_de_clientes.barrio as barrio_cliente,
tabla_de_vendedores.nombre as nombre_vendedor,
tabla_de_vendedores.barrio as barrio_vendedor
FROM tabla_de_clientes
    INNER JOIN
    tabla_de_vendedores
    ON tabla_de_clientes.barrio = tabla_de_vendedores.barrio;
```

```txt
+-------------------+----------------+---------------------+-----------------+
| nombre_cliente    | barrio_cliente | nombre_vendedor     | barrio_vendedor |
+-------------------+----------------+---------------------+-----------------+
| Erica Carvajo     | Del Valle      | Claudia Morales     | Del Valle       |
| Marcos Rosas      | Del Valle      | Claudia Morales     | Del Valle       |
| Joana Olivera     | Condesa        | Miguel Pavón Silva  | Condesa         |
| Gabriel Roca      | Del Valle      | Claudia Morales     | Del Valle       |
| Luis Silva        | Contadero      | Concepción Martinez | Contadero       |
| Alberto Rodriguez | Oblatos        | Patricia Sánchez    | Oblatos         |
+-------------------+----------------+---------------------+-----------------+
```

**CROSS JOIN** utilizando `+0` para representar los **BIT**

```sql
SELECT tabla_de_clientes.nombre, tabla_de_clientes.ciudad, tabla_de_clientes.barrio,
tabla_de_vendedores.nombre, tabla_de_vendedores.vacaciones+0 AS vacaciones
FROM tabla_de_clientes, tabla_de_vendedores
    WHERE tabla_de_clientes.barrio = tabla_de_vendedores.barrio;
```

```txt
+-------------------+------------------+-----------+---------------------+------------+
| nombre            | ciudad           | barrio    | nombre              | vacaciones |
+-------------------+------------------+-----------+---------------------+------------+
| Erica Carvajo     | Ciudad de México | Del Valle | Claudia Morales     |          1 |
| Marcos Rosas      | Ciudad de México | Del Valle | Claudia Morales     |          1 |
| Joana Olivera     | Ciudad de México | Condesa   | Miguel Pavón Silva  |          0 |
| Gabriel Roca      | Ciudad de México | Del Valle | Claudia Morales     |          1 |
| Luis Silva        | Ciudad de México | Contadero | Concepción Martinez |          1 |
| Alberto Rodriguez | Guadalajara      | Oblatos   | Patricia Sánchez    |          0 |
+-------------------+------------------+-----------+---------------------+------------+
```

## UNION

Permite unir dos o más tablas (implícitamente ejecuta DISTINCT)

El número de campos en las tabls de ser iguales (mismos campos y tipos)

ej.

| Id | Hobby |
| :-: | :-: |
| 4 | Lectura |
| 5 | Futbol |
| 6 | Tenis |
| 7 | Alpinismo |

| Id | Hobby |
| :-: | :-: |
| 8 | Fotografía |
| 9 | Hípica |
| 5 | Futbol |
| 11 | Trote |

### Estructura UNION

```sql
<consulta_1>
UNION
<consulta_2>;
```

Retorna:

| Id | Hobby |
| :-: | :-: |
| 4 | Lectura |
| 5 | Futbol |
| 6 | Tenis |
| 7 | Alpinismo |
| 8 | Fotografía |
| 9 | Hípica |
| 11 | Trote |

### Estructura UNION ALL

```sql
<consulta_1>
UNION ALL
<consulta_2>;
```

Retorna:

| Id | Hobby |
| :-: | :-: |
| 4 | Lectura |
| 5 | Futbol |
| 6 | Tenis |
| 7 | Alpinismo |
| 8 | Fotografía |
| 9 | Hípica |
| 5 | Futbol |
| 11 | Trote |

### Práctica UNION

```sql
SELECT BARRIO FROM tabla_de_clientes
UNION
SELECT BARRIO FROM tabla_de_vendedores;
```

```txt
+-------------------------+
| BARRIO                  |
+-------------------------+
| Del Valle               |
| Locaxco                 |
| Cuajimalpa              |
| Condesa                 |
| Héroes de Padierna      |
| Carola                  |
| Contadero               |
| Floresta Coyoacán       |
| Oblatos                 |
| Barrio del Niño Jesús   |
| Barragán Hernández      |
| Ex Hacienda Coapa       |
| Alcalde Barranquitas    |
+-------------------------+
```

```sql
SELECT barrio FROM tabla_de_clientes
UNION ALL
SELECT barrio FROM tabla_de_vendedores;
```

```txt
+-------------------------+
| BARRIO                  |
+-------------------------+
| Del Valle               |
| Del Valle               |
| Locaxco                 |
| Cuajimalpa              |
| Condesa                 |
| Héroes de Padierna      |
| Del Valle               |
| Carola                  |
| Contadero               |
| Floresta Coyoacán       |
| Oblatos                 |
| Barrio del Niño Jesús   |
| Barragán Hernández      |
| Ex Hacienda Coapa       |
| Alcalde Barranquitas    |
| Condesa                 |
| Del Valle               |
| Contadero               |
| Oblatos                 |
+-------------------------+
```

```sql
SELECT barrio, nombre, 'Cliente' AS tipo, dni FROM tabla_de_clientes
UNION ALL
SELECT barrio, nombre, 'Vendedor' AS tipo, matricula FROM tabla_de_vendedores;
```

```txt
+-------------------------+----------------------+----------+-------------+
| barrio                  | nombre               | tipo     | dni         |
+-------------------------+----------------------+----------+-------------+
| Del Valle               | Erica Carvajo        | Cliente  | 1471156710  |
| Del Valle               | Marcos Rosas         | Cliente  | 3623344710  |
| Locaxco                 | Jorge Castro         | Cliente  | 492472718   |
| Cuajimalpa              | Abel Pintos          | Cliente  | 50534475787 |
| Condesa                 | Joana Olivera        | Cliente  | 5576228758  |
| Héroes de Padierna      | Paolo Mendez         | Cliente  | 5648641702  |
| Del Valle               | Gabriel Roca         | Cliente  | 5840119709  |
| Carola                  | Marcelo Perez        | Cliente  | 7771579779  |
| Contadero               | Luis Silva           | Cliente  | 8502682733  |
| Floresta Coyoacán       | Carlos Santivañez    | Cliente  | 8719655770  |
| Oblatos                 | Alberto Rodriguez    | Cliente  | 9275760794  |
| Barrio del Niño Jesús   | Edson Calisaya       | Cliente  | 9283760794  |
| Barragán Hernández      | María Jimenez        | Cliente  | 94387575700 |
| Ex Hacienda Coapa       | Walter Soruco        | Cliente  | 94387591700 |
| Alcalde Barranquitas    | Ximena Gómez         | Cliente  | 95939180787 |
| Condesa                 | Miguel Pavón Silva   | Vendedor | 00235       |
| Del Valle               | Claudia Morales      | Vendedor | 00236       |
| Contadero               | Concepción Martinez  | Vendedor | 00237       |
| Oblatos                 | Patricia Sánchez     | Vendedor | 00238       |
+-------------------------+----------------------+----------+-------------+
```

```sql
SELECT tabla_de_clientes.nombre, tabla_de_clientes.ciudad, tabla_de_clientes.barrio,
tabla_de_vendedores.nombre, vacaciones
FROM tabla_de_clientes
    LEFT JOIN
        tabla_de_vendedores
        ON tabla_de_clientes.barrio = tabla_de_vendedores.barrio
UNION
SELECT tabla_de_clientes.nombre, tabla_de_clientes.ciudad, tabla_de_clientes.barrio,
tabla_de_vendedores.nombre, tabla_de_vendedores.vacaciones+0 AS vacaciones
FROM tabla_de_clientes
    RIGHT JOIN
        tabla_de_vendedores
        ON tabla_de_clientes.barrio = tabla_de_vendedores.barrio;
```

```txt
+--------------------+-------------------+-------------------------+----------------------+------------+
| nombre             | ciudad            | barrio                  | nombre               | vacaciones |
+--------------------+-------------------+-------------------------+----------------------+------------+
| Joana Olivera      | Ciudad de México  | Condesa                 | Miguel Pavón Silva   | 0          |
| Erica Carvajo      | Ciudad de México  | Del Valle               | Claudia Morales      | 1          |
| Marcos Rosas       | Ciudad de México  | Del Valle               | Claudia Morales      | 1          |
| Gabriel Roca       | Ciudad de México  | Del Valle               | Claudia Morales      | 1          |
| Luis Silva         | Ciudad de México  | Contadero               | Concepción Martinez  | 1          |
| Alberto Rodriguez  | Guadalajara       | Oblatos                 | Patricia Sánchez     | 0          |
| Jorge Castro       | Ciudad de México  | Locaxco                 | NULL                 | NULL       |
| Abel Pintos        | Ciudad de México  | Cuajimalpa              | NULL                 | NULL       |
| Paolo Mendez       | Ciudad de México  | Héroes de Padierna      | NULL                 | NULL       |
| Marcelo Perez      | Ciudad de México  | Carola                  | NULL                 | NULL       |
| Carlos Santivañez  | Ciudad de México  | Floresta Coyoacán       | NULL                 | NULL       |
| Edson Calisaya     | Ciudad de México  | Barrio del Niño Jesús   | NULL                 | NULL       |
| María Jimenez      | Guadalajara       | Barragán Hernández      | NULL                 | NULL       |
| Walter Soruco      | Ciudad de México  | Ex Hacienda Coapa       | NULL                 | NULL       |
| Ximena Gómez       | Guadalajara       | Alcalde Barranquitas    | NULL                 | NULL       |
+--------------------+-------------------+-------------------------+----------------------+------------+
```

## Subconsultas

Realizar una consulta al interior de otra

| X | Y |
| :-: | :-: |
| A | 3 |
| Z | 5 |
| Z | 1 |
| A | 1 |
| E | 4 |
| T | 3 |
| Z | 8 |
| Z | 2 |
| T | 1 |

| Y |
| :-: | :-: |
| 1 |
| 2 |

```sql
SELECT x, y FROM tb1
WHERE y IN (
    SELECT Y FROM tb2
)
```

| X | Y |
| :-: | :-: |
| A | 3 |
| Z | 1 |
| A | 1 |
| T | 3 |
| Z | 2 |
| T | 1 |

```sql
SELECT x, SUM(y) AS new_y
FROM tb1 GROUP BY X
```

| X | NEW_Y |
| :-: | :-: |
| A | 4 |
| E | 4 |
| T | 4 |
| Z | 16 |

```sql
SELECT z.x, z.new_y FROM
    (SELECT x SUM(y) AS new_y
    FROM tb1 GROUP BY x) z
    WHERE z.new_y = 4
```

| X | NEW_Y |
| :-: | :-: |
| A | 4 |
| E | 4 |
| T | 4 |

### Práctica Subconsultas

```sql
SELECT *, primera_compra+0 FROM tabla_de_clientes
WHERE barrio IN(
    SELECT DISTINCT barrio FROM tabla_de_vendedores
); 
```

```txt
+------------+-------------------+---------------------------------+-----------+-------------------+--------+----------+---------------------+------+------+-------------------+-------------------+------------------+
| DNI        | NOMBRE            | DIRECCION_1                     | BARRIO    | CIUDAD            | ESTADO | CP       | FECHA_DE_NACIMIENTO | EDAD | SEXO | LIMITE_DE_CREDITO | VOLUMEN_DE_COMPRA | primera_compra+0 |
+------------+-------------------+---------------------------------+-----------+-------------------+--------+----------+---------------------+------+------+-------------------+-------------------+------------------+
| 1471156710 | Erica Carvajo     | Heriberto Frías 1107            | Del Valle | Ciudad de México  | EM     | 80012212 | 1990-03-01          |   30 | F    |            170000 |            245000 |                1 |
| 3623344710 | Marcos Rosas      | Av. Universidad                 | Del Valle | Ciudad de México  | EM     | 22002012 | 1995-05-13          |   26 | M    |            110000 |            220000 |                0 |
| 5576228758 | Joana Olivera     | Pachuca 75                      | Condesa   | Ciudad de México  | EM     | 88192029 | 1995-02-14          |   25 | F    |             70000 |            160000 |                0 |
| 5840119709 | Gabriel Roca      | Eje Central Lázaro Cárdenas 911 | Del Valle | Ciudad de México  | EM     | 80010221 | 1985-06-16          |   36 | M    |            140000 |            210000 |                0 |
| 8502682733 | Luis Silva        | Prol. 16 de Septiembre 1156     | Contadero | Ciudad de México  | EM     | 82122020 | 1995-04-07          |   25 | M    |            110000 |            190000 |                1 |
| 9275760794 | Alberto Rodriguez | Circunvalación Oblatos 690      | Oblatos   | Guadalajara       | JC     | 44700000 | 1991-12-28          |   26 | M    |             75000 |             95000 |                1 |
+------------+-------------------+---------------------------------+-----------+-------------------+--------+----------+---------------------+------+------+-------------------+-------------------+------------------+
```

```sql
SELECT x.envase, x.precio_maximo
FROM (
        SELECT envase, MAX(precio_de_lista) AS precio_maximo
        FROM tabla_de_productos
        GROUP BY envase
     ) x
     WHERE x.precio_maximo >=10;
```

```txt
+-------------------+---------------+
| envase            | precio_maximo |
+-------------------+---------------+
| Botella de Vidrio |         13.31 |
| Botella PET       |         38.01 |
+-------------------+---------------+
```

***¿ Como hacer esta query usando subconsultas ?***

```sql
SELECT DNI, COUNT(*) FROM facturas
WHERE YEAR(FECHA_VENTA) = 2016
GROUP BY DNI
HAVING COUNT(*) > 2000;
```

```txt
+-------------+----------+
| DNI         | COUNT(*) |
+-------------+----------+
| 3623344710  |     2012 |
| 492472718   |     2008 |
| 50534475787 |     2037 |
+-------------+----------+
```

```sql
SELECT X.DNI, X.CONTADOR
FROM (
    SELECT DNI, COUNT(*) AS CONTADOR FROM facturas
    WHERE YEAR(FECHA_VENTA) = 2016
    GROUP BY DNI
    ) X WHERE X.CONTADOR > 2000;
```

## VIEWS

Una ***View*** es una tabla lógica que resulta de una consulta que puede ser
usada posteriormente en cualquier otra consulta.

La vista tiene un costo de procesamiento, cada vez que es invocada se ejecuta
su consulta.

| X | Y |
| :-: | :-: |
| A | 3 |
| Z | 5 |
| Z | 1 |
| A | 1 |
| E | 4 |
| T | 3 |
| Z | 8 |
| Z | 2 |
| T | 1 |

```sql
CREATE VIEW VW_VIEW AS
SELECT x, SUM(Y) AS new_Y
FROM tb1 GROUP BY x
```

VW_VIEW

| X | Y |
| :-: | :-: |
| A | 4 |
| E | 4 |
| T | 4 |
| Z | 16 |

Al almacenar una consulta, se crea una **VIEW**, en este caso llamada
**VW_VIEW**

tb3

| W | Y |
| :-: | :-: |
| F | 4 |
| H | 4 |
| H | 5 |
| G | 6 |
| F | 5 |
| P | 16 |
| L | 7 |
| M | 15 |
| N | 6 |

```sql
SELECT VW_VIEW.x, tb3.w FROM VW_VIEW
    INNER JOIN
        tb3 ON VW_VIEW.new_y = tb3.y
        WHERE tb3.y = 16;
```

| X | W |
| :-: | :-: |
| Z | P |

### Prácticas VIEW

Creando la vista

```sql
CREATE VIEW vw_envases_grandes AS
    SELECT envase, MAX(precio_de_lista) AS precio_maximo
    FROM tabla_de_productos
    GROUP BY envase;

SELECT * FROM vw_envases_grandes;
```

```txt
+-------------------+---------------+
| envase            | precio_maximo |
+-------------------+---------------+
| Botella de Vidrio |         13.31 |
| Botella PET       |         38.01 |
| Lata              |          4.56 |
+-------------------+---------------+
```

Usando la vista

```sql
SELECT x.envase, x.precio_maximo
FROM vw_envases_grandes x
WHERE precio_maximo >=10;
```

```txt
+-------------------+---------------+
| envase            | precio_maximo |
+-------------------+---------------+
| Botella de Vidrio |         13.31 |
| Botella PET       |         38.01 |
+-------------------+---------------+
```

```sql
SELECT a.nombre_del_producto, a.envase, a.precio_de_lista,
b.precio_maximo
FROM tabla_de_productos a
INNER JOIN
    vw_envases_grandes b
    ON a.envase = b.envase;
```

```txt
+---------------------+-------------------+-----------------+---------------+
| nombre_del_producto | envase            | precio_de_lista | precio_maximo |
+---------------------+-------------------+-----------------+---------------+
| Sabor da Montaña    | Botella de Vidrio |            6.31 |         13.31 |
| Línea Citrus        | Botella PET       |               7 |         38.01 |
| Vida del Campo      | Botella de Vidrio |            8.41 |         13.31 |
| Vida del Campo      | Botella PET       |           19.51 |         38.01 |
| Vida del Campo      | Botella PET       |           24.01 |         38.01 |
| Festival de Sabores | Botella PET       |           38.01 |         38.01 |
| Clean               | Botella PET       |           16.01 |         38.01 |
...
```

```sql
SELECT a.nombre_del_producto, a.envase,
((a.precio_de_lista/b.precio_maximo)-1)*100 AS variacion_percent
FROM tabla_de_productos a
INNER JOIN
    vw_envases_grandes b
    ON a.envase = b.envase;
```

```txt
+---------------------+-------------------+---------------------+
| nombre_del_producto | envase            | variacion_percent   |
+---------------------+-------------------+---------------------+
| Sabor da Montaña    | Botella de Vidrio |  -52.59203798761971 |
| Línea Citrus        | Botella PET       |  -81.58379292525673 |
| Vida del Campo      | Botella de Vidrio |  -36.81442838260782 |
...
```

## FUNCIONES

- MySQL Functions & Operators
[Doc](https://dev.mysql.com/doc/refman/8.0/en/built-in-function-reference.html)
- MySQL Functions en
[W3Schools](https://www.w3schools.com/mysql/mysql_ref_functions.asp)
- MariaDB [Functions](https://mariadb.com/kb/en/built-in-functions/) List
- MariaDB Functions & Operators
[Doc](https://mariadb.com/kb/en/function-and-operator-reference/)
- Diferencias entre funciones MySQL y
[MariaDB](https://mariadb.com/kb/en/function-differences-between-mariadb-11-1-and-mysql-8-0/)

### LTRIM

```sql
SELECT LTRIM("    TRIM IZQUIERDO     ") AS LTRIM;
SELECT RTRIM("    TRIM DERECHO     ") AS RTRIM;
SELECT TRIM("    TRIM GENERAL     ") AS TRIM;
```

```txt
+---------------------+
| LTRIM               |
+---------------------+
| TRIM IZQUIERDO      |
+---------------------+

+------------------+
| RTRIM            |
+------------------+
|     TRIM DERECHO |
+------------------+

+--------------+
| TRIM         |
+--------------+
| TRIM GENERAL |
+--------------+
```

### CONCAT

```sql
SELECT CONCAT("MySQL es entretenido,", " no lo crees?") AS CONCAT;
```

```txt
+------------------------------------+
| CONCAT                             |
+------------------------------------+
| MySQL es entretenido, no lo crees? |
+------------------------------------+
```

### UPPER, LOWER

```sql
SELECT UPPER("MySQL es entretenido") AS UPPER;
SELECT LOWER("MYSQL ES ENTRETENIDO") AS LOWER;
```

```txt
+----------------------+
| UPPER                |
+----------------------+
| MYSQL ES ENTRETENIDO |
+----------------------+

+----------------------+
| LOWER                |
+----------------------+
| mysql es entretenido |
+----------------------+
```

### SUBSTRING

```sql
SELECT SUBSTRING("MySQL es entretenido e interesante, bastante que aprender", 24, 11)
    AS SUBSTRING;
```

```txt
+-------------+
| SUBSTRING   |
+-------------+
| interesante |
+-------------+
```

### Práctica funciones

```sql
SELECT CONCAT(nombre, " ", dni) AS nombre_dni
FROM tabla_de_clientes;
```

```txt
+-------------------------------+
| nombre_dni                    |
+-------------------------------+
| Erica Carvajo 1471156710      |
| Marcos Rosas 3623344710       |
| Jorge Castro 492472718        |
| Abel Pintos 50534475787       |
| Joana Olivera 5576228758      |
...
```

***Crear una consulta listando nombre y dirección de cliente, con el detalle***
***de calle, barrio, ciudad y estado***

```sql
SELECT nombre, CONCAT(direccion_1, " ", barrio, " ", ciudad, " ", estado)
AS direccion
FROM tabla_de_clientes;
```

```txt
+---------------+--------------------------------------------------------+
| nombre        | direccion                                              |
+---------------+--------------------------------------------------------+
| Erica Carvajo | Heriberto Frías 1107 Del Valle Ciudad de México EM     |
| Marcos Rosas  | Av. Universidad Del Valle Ciudad de México EM          |
| Jorge Castro  | Federal México-Toluca 5690 Locaxco Ciudad de México EM |
...
```

## DATE FUNCTIONS

### ADDDATE, CURDATE

```sql
SELECT ADDDATE(CURDATE(), INTERVAL 5 YEAR);
```

```txt
+-------------------------------------+
| ADDDATE(CURDATE(), INTERVAL 5 YEAR) |
+-------------------------------------+
| 2028-10-16                          |
+-------------------------------------+
```

### CURRENT_TIMESTAMP

```sql
SELECT CURRENT_TIMESTAMP();
```

```txt
+---------------------+
| CURRENT_TIMESTAMP() |
+---------------------+
| 2023-10-16 14:47:58 |
+---------------------+
```

### DATE DATEDIFF

```sql
SELECT DATE("2023-10-16");
SELECT DATEDIFF("2025-12-02", CURDATE());
```

```txt
+--------------------+
| DATE("2023-10-16") |
+--------------------+
| 2023-10-16         |
+--------------------+

+-----------------------------------+
| DATEDIFF("2025-12-02", CURDATE()) |
+-----------------------------------+
|                               778 |
+-----------------------------------+
```

### DATES NAMES

```sql
SELECT MONTHNAME(CURDATE()), DAYNAME(CURDATE()), TIME_TO_SEC(CURRENT_TIME());
```

```txt
+----------------------+--------------------+-----------------------------+
| MONTHNAME(CURDATE()) | DAYNAME(CURDATE()) | TIME_TO_SEC(CURRENT_TIME()) |
+----------------------+--------------------+-----------------------------+
| October              | Monday             |                       53946 |
+----------------------+--------------------+-----------------------------+
```

```sql
SELECT CONCAT("Fecha y hora actual: ",
DATE_FORMAT(CURRENT_TIMESTAMP(), "%W, %d/%m/%Y, %T")) AS FECHA;
```

```txt
+---------------------------------------------------+
| FECHA                                             |
+---------------------------------------------------+
| Fecha y hora actual: Monday, 16/10/2023, 15:49:02 |
+---------------------------------------------------+
```

### Práctica DATE FUNCTIONS

```sql
SELECT DISTINCT fecha_venta,
DAYNAME(fecha_venta) AS DIA,
MONTHNAME(fecha_venta) AS MES,
YEAR(fecha_venta) AS AÑO
FROM facturas;
```

```txt
+-------------+-----------+---------+------+
| fecha_venta | DIA       | MES     | AÑO  |
+-------------+-----------+---------+------+
| 2015-01-01  | Thursday  | January | 2015 |
| 2015-01-02  | Friday    | January | 2015 |
| 2015-01-03  | Saturday  | January | 2015 |
...
```

***Crear una consulta que muestre el nombre y la edad actual del cliente***

```sql
SELECT nombre, TIMESTAMPDIFF(YEAR, fecha_de_nacimiento, CURDATE()) AS edad
FROM tabla_de_clientes;
```

```txt
+--------------------+------+
| nombre             | edad |
+--------------------+------+
| Erica Carvajo      |   33 |
| Marcos Rosas       |   28 |
| Jorge Castro       |   28 |
...
```

## Funciones Matemáticas

### CEIL, FLOOR, ROUND

```sql
SELECT CEIL(25.01), FLOOR(25.99), ROUND(25.5), ROUND(25.456789, 2);
```

```txt
+-------------+--------------+-------------+---------------------+
| CEIL(25.01) | FLOOR(25.99) | ROUND(25.5) | ROUND(25.456789, 2) |
+-------------+--------------+-------------+---------------------+
|          26 |           25 |          26 |               25.46 |
+-------------+--------------+-------------+---------------------+
```

***Calcular el valor del impuesto pago en el año de 2016 redondeando al menor entero.***
Considerar:

- tabla de facturas -> impuesto
- tabla items_facturas -> cantidad y facturación

```sql
SELECT YEAR(fecha_venta) AS AÑO,
FLOOR(SUM(impuesto * (cantidad * precio))) AS TOTAL_IMPUESTOS
FROM facturas F
    INNER JOIN items_facturas IFa ON F.numero = IFa.numero
    WHERE YEAR(fecha_venta) = 2016
    GROUP BY AÑO;
```

```txt
+------+-----------------+
| AÑO  | TOTAL_IMPUESTOS |
+------+-----------------+
| 2016 |         4656937 |
+------+-----------------+
```

## CONVERT

- [MariaDB](https://mariadb.com/kb/en/convert/)
- [MySQL](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_convert)

```sql
SELECT SUBSTRING(CONVERT(23.45, CHAR),3,1) AS RESULTADO;
```

```txt
+-----------+
| RESULTADO |
+-----------+
| .         |
+-----------+
```

***Crear consulta que retorne el sgte. resultado por cliente***

`El cliente <nombre> compró <cantidad> en el año 2016`

```sql
SELECT CONCAT('El cliente ', TC.NOMBRE, ' compró $', 
CONVERT(SUM(IFa.CANTIDAD * IFa.precio), CHAR(20))
 , '.- el año ', CONVERT(YEAR(F.FECHA_VENTA), CHAR(20))) AS FRASE
FROM facturas F
    INNER JOIN items_facturas IFa ON F.NUMERO = IFa.NUMERO
        INNER JOIN tabla_de_clientes TC ON F.DNI = TC.DNI
        WHERE YEAR(FECHA_VENTA) = 2016
    GROUP BY TC.NOMBRE, YEAR(FECHA_VENTA);
```

```txt
+--------------------------------------------------------------------------+
| FRASE                                                                    |
+--------------------------------------------------------------------------+
| El cliente Abel Pintos compró $3111017.9194583893.- el año 2016          |
| El cliente Carlos Santivañez compró $2827179.4774594307.- el año 2016    |
| El cliente Edson Calisaya compró $3076894.2775964737.- el año 2016       |
...
```

## Informes

### Compras por cliente, detalle DNI, MES-AÑO y COMPRAS

```sql
SELECT F.dni,
       DATE_FORMAT(F.fecha_venta, "%m-%Y") AS mes_año,
       I.cantidad
FROM facturas F
    INNER JOIN
        items_facturas I
    ON F.numero = I.numero
    ORDER BY cantidad;
```

```txt
+-------------+----------+----------+
| dni         | mes_año  | cantidad |
+-------------+----------+----------+
| 7771579779  | 01-2015  |       63 |
| 7771579779  | 01-2015  |       26 |
| 7771579779  | 01-2015  |       67 |
...
```

### Compras mensuales por cliente

```sql
SELECT F.dni,
       DATE_FORMAT(F.fecha_venta, "%m-%Y") AS mes_año,
       SUM(I.cantidad) AS VENTAS
FROM facturas F
    INNER JOIN
        items_facturas I
    ON F.numero = I.numero
    GROUP BY F.dni mes_año
    ORDER BY mes_año;
```

```txt
+-------------+----------+--------+
| dni         | mes_año  | VENTAS |
+-------------+----------+--------+
| 50534475787 | 01-2015  |  23176 |
| 1471156710  | 01-2015  |  24316 |
| 5576228758  | 01-2015  |  21563 |
...
```

***Listar clientes con ventas inválidas y calcular diferencia entre el límite
de venta máximo y la cantidad vendida en porcentaje.***

```sql
SELECT
    A.DNI, A.NOMBRE, A.MES_AÑO,
    A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
   WHEN  (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta Válida'
   ELSE 'Venta Inválida'
END
    AS STATUS_VENTA,
    ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) AS PORCENTAJE
    FROM(
        SELECT F.DNI,
        TC.NOMBRE,
        DATE_FORMAT(F.FECHA_VENTA, "%m - %Y") AS MES_AÑO, 
        SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA, 
        MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA  
        FROM facturas F 
            INNER JOIN 
                items_facturas IFa
            ON F.NUMERO = IFa.NUMERO
            INNER JOIN 
                tabla_de_clientes TC
            ON TC.DNI = F.DNI
    GROUP BY
    F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y"))A
    WHERE (A.CANTIDAD_MAXIMA - A.CANTIDAD_VENDIDA) < 0;
```

```txt
+-------------+--------------------+-----------+------------+-----------------+------------+
| DNI         | NOMBRE             | MES_AÑO   | DIFERENCIA | STATUS_VENTA    | PORCENTAJE |
+-------------+--------------------+-----------+------------+-----------------+------------+
| 1471156710  | Erica Carvajo      | 05 - 2015 |       1885 | Venta Inválida  |       7.14 |
| 1471156710  | Erica Carvajo      | 06 - 2016 |        542 | Venta Inválida  |       2.16 |
| 1471156710  | Erica Carvajo      | 07 - 2017 |       1715 | Venta Inválida  |       6.54 |
| 1471156710  | Erica Carvajo      | 08 - 2015 |        426 | Venta Inválida  |       1.71 |
| 3623344710  | Marcos Rosas       | 01 - 2016 |       2876 | Venta Inválida  |      11.56 |
...
```

***Listar solo clientes con ventas inválidas en el año 2018 excediendo más
del 50% de su límite permitido por mes.
Calcular el porcentaje de diferencia entre el límite de venta máximo y la
cantidad vendida.***

```sql
SELECT A.DNI, A.NOMBRE, A.MES_AÑO, 
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
    WHEN  (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta Válida'
    ELSE 'Venta Inválida'
END AS
    STATUS_VENTA,
    ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) AS PORCENTAJE
    FROM(
        SELECT F.DNI,
        TC.NOMBRE,
        DATE_FORMAT(F.FECHA_VENTA, "%m - %Y") AS MES_AÑO, 
        SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA, 
        MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA  
        FROM facturas F 
        INNER JOIN 
            items_facturas IFa
        ON F.NUMERO = IFa.NUMERO
        INNER JOIN 
            tabla_de_clientes TC
        ON TC.DNI = F.DNI
    GROUP BY
        F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")) A
    WHERE (A.CANTIDAD_MAXIMA - A.CANTIDAD_VENDIDA) < 0
        AND ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) > 50
        AND A.MES_AÑO LIKE "%2018";
```

```txt
+-----------+--------------+-----------+------------+-----------------+------------+
| DNI       | NOMBRE       | MES_AÑO   | DIFERENCIA | STATUS_VENTA    | PORCENTAJE |
+-----------+--------------+-----------+------------+-----------------+------------+
| 492472718 | Jorge Castro | 02 - 2018 |      11219 | Venta Inválida  |      54.15 |
| 492472718 | Jorge Castro | 03 - 2018 |      10789 | Venta Inválida  |      53.18 |
+-----------+--------------+-----------+------------+-----------------+------------+
```

### Informe de ventas por sabor en el 2016

```sql
SELECT P.SABOR, SUM(I.CANTIDAD) AS TOTAL, YEAR(F.FECHA_VENTA) AS AÑO
FROM tabla_de_productos P
INNER JOIN items_facturas I
ON P.codigo_del_producto = I.codigo_del_producto
INNER JOIN facturas F
ON F.NUMERO = I.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY P.SABOR, YEAR(F.FECHA_VENTA)
ORDER BY SUM(I.CANTIDAD) DESC;
```

```txt
+-----------------+--------+------+
| SABOR           | TOTAL  | AÑO  |
+-----------------+--------+------+
| Mango           | 613309 | 2016 |
| Sandía          | 487625 | 2016 |
| Naranja         | 483663 | 2016 |
| Manzana         | 363166 | 2016 |
| Maracuyá        | 245456 | 2016 |
| Lima/Limón      | 239634 | 2016 |
| Frutilla/Limón  | 238118 | 2016 |
| Cereza/Manzana  | 236535 | 2016 |
| Asaí            | 235660 | 2016 |
| Asái            | 121615 | 2016 |
| Uva             | 120597 | 2016 |
| Cereza          | 120478 | 2016 |
| Frutilla        | 120384 | 2016 |
+-----------------+--------+------+
```

```sql
SELECT VENTAS_SABOR.SABOR,
    VENTAS_SABOR.AÑO,
    VENTAS_SABOR.CANTIDAD_TOTAL,
    ROUND((VENTAS_SABOR.CANTIDAD_TOTAL/VENTA_TOTAL.CANTIDAD_TOTAL)*100,2) 
    AS PORCENTAJE
FROM (
    SELECT P.SABOR,
        SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
        YEAR(F.FECHA_VENTA) AS AÑO
    FROM tabla_de_productos P
    INNER JOIN items_facturas IFa
    ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
        INNER JOIN facturas F
        ON F.NUMERO = IFa.NUMERO
    WHERE YEAR(F.FECHA_VENTA) = 2016
    GROUP BY P.SABOR, YEAR(F.FECHA_VENTA)
    ORDER BY SUM(IFa.CANTIDAD) DESC
) VENTAS_SABOR
    INNER JOIN (
        SELECT SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
        YEAR(F.FECHA_VENTA) AS AÑO
        FROM tabla_de_productos P
            INNER JOIN items_facturas IFa
            ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
                INNER JOIN facturas F
                ON F.NUMERO = IFa.NUMERO
        WHERE YEAR(F.FECHA_VENTA) = 2016
        GROUP BY YEAR(F.FECHA_VENTA)
    ) VENTA_TOTAL
ON VENTA_TOTAL.AÑO = VENTAS_SABOR.AÑO
ORDER BY VENTAS_SABOR.CANTIDAD_TOTAL DESC;
```

```txt
+-----------------+------+----------------+------------+
| SABOR           | AÑO  | CANTIDAD_TOTAL | PORCENTAJE |
+-----------------+------+----------------+------------+
| Mango           | 2016 |         613309 |      16.91 |
| Sandía          | 2016 |         487625 |      13.45 |
| Naranja         | 2016 |         483663 |      13.34 |
| Manzana         | 2016 |         363166 |      10.01 |
| Maracuyá        | 2016 |         245456 |       6.77 |
| Lima/Limón      | 2016 |         239634 |       6.61 |
| Frutilla/Limón  | 2016 |         238118 |       6.57 |
| Cereza/Manzana  | 2016 |         236535 |       6.52 |
| Asaí            | 2016 |         235660 |       6.50 |
| Asái            | 2016 |         121615 |       3.35 |
| Uva             | 2016 |         120597 |       3.33 |
| Cereza          | 2016 |         120478 |       3.32 |
| Frutilla        | 2016 |         120384 |       3.32 |
+-----------------+------+----------------+------------+
```

El mismo ranking pero por tamaño

```sql
SELECT 
    VENTAS_TAMANO.TAMANO,
    VENTAS_TAMANO.AÑO,
    VENTAS_TAMANO.CANTIDAD_TOTAL,
    ROUND((VENTAS_TAMANO.CANTIDAD_TOTAL/VENTA_TOTAL.CANTIDAD_TOTAL)*100,2) 
    AS PORCENTAJE
FROM (
    SELECT P.TAMANO,
    SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
    YEAR(F.FECHA_VENTA) AS AÑO
    FROM tabla_de_productos P
    INNER JOIN items_facturas IFa
        ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
        INNER JOIN facturas F
            ON F.NUMERO = IFa.NUMERO
    WHERE YEAR(F.FECHA_VENTA) = 2016
    GROUP BY P.TAMANO, YEAR(F.FECHA_VENTA)
    ORDER BY SUM(IFa.CANTIDAD) DESC
) VENTAS_TAMANO
    INNER JOIN (
        SELECT
        SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
        YEAR(F.FECHA_VENTA) AS AÑO
        FROM tabla_de_productos P
            INNER JOIN items_facturas IFa
            ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
                INNER JOIN facturas F
                ON F.NUMERO = IFa.NUMERO
        WHERE YEAR(F.FECHA_VENTA) = 2016
        GROUP BY YEAR(F.FECHA_VENTA)
    ) VENTA_TOTAL
    ON VENTA_TOTAL.AÑO = VENTAS_TAMANO.AÑO
ORDER BY VENTAS_TAMANO.CANTIDAD_TOTAL DESC;
```

```txt
+------------+------+----------------+------------+
| TAMANO     | AÑO  | CANTIDAD_TOTAL | PORCENTAJE |
+------------+------+----------------+------------+
| 700 ml     | 2016 |        1072577 |      29.58 |
| 1,5 Litros | 2016 |         728225 |      20.08 |
| 350 ml     | 2016 |         615021 |      16.96 |
| 1 Litro    | 2016 |         605779 |      16.71 |
| 2 Litros   | 2016 |         360030 |       9.93 |
| 470 ml     | 2016 |         244608 |       6.75 |
+------------+------+----------------+------------+
```

