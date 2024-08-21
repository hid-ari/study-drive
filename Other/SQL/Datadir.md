Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Ahora bien, sobre mecanismos de almacenamiento, durante la creación de la tabla, es posible determinar cuál mecanismo la misma estará utilizando. Crea una tabla, en la base de datos **sakila**, conforme al siguiente comando:

```sql
CREATETABLE df_table (IDINT, NOMBREVARCHAR(100));COPIA EL CÓDIGO
```

2. Si te diriges hacia Tables, en el árbol de objetos de Workbench y haz clic sobre el ícono de información, verás las características de almacenamiento de la tabla que fue creada:
![[study_drive/other/SQL/imgs/Pasted image 20240716164844.png]]  3) Ahora, puedes observar que, por defecto, las tablas son creadas con el mecanismo de almacenamiento InnoDB:
![[study_drive/other/SQL/imgs/Pasted image 20240716164921.png]]4. Es posible alterar la propiedad del mecanismo de almacenamiento de la tabla, con el comando:

```sql
ALTERTABLE DEFAULT_TABLE ENGINE = MyISAM;COPIA EL CÓDIGO
```

5. Adicionalmente, puedes definir el tipo de mecanismo de almacenamiento que será usado en una tabla al momento de su creación. Para ello, digite:

```sql
CREATETABLE df_table1 (IDINT, NOMBREVARCHAR(100)) ENGINE = MEMORY;COPIA EL CÓDIGO
```

6. Cuando crees una tabla por el asistente de Workbench, puedes ver la opción de selección de mecanismos de almacenamiento, siempre presentando a **InnoDB** como estándar: 
![[study_drive/other/SQL/imgs/Pasted image 20240716164944.png]]  7. Los componentes de la base quedan almacenados en una base de datos. Puedes crear una nueva base de datos con el siguiente comando (para este caso, será creada con el nombre de **base**):

```csharp
CREATE DATABASEbase;COPIA EL CÓDIGO
```

8. La base de datos puede ser creada, también, por el asistente de Workbench. Para ello, haz clic con el botón derecho del mouse en un espacio vacío de la lista de componentes, a la izquierda de Workbench, y escoge la opción **Create Schema...**:
 ![[study_drive/other/SQL/imgs/Pasted image 20240716165013.png]] 9) Crea una nueva base llamada base2, pero utilizando el asistente. Para ello, digita su nombre en la opción Name:
 ![[study_drive/other/SQL/imgs/Pasted image 20240716165032.png]]
 1) Cuando fueron creadas estas bases, MySQL escribió en el disco duro los archivos físicos que las representan. Para saber en qué directorio estos archivos fueron creados, puedes consultar el valor de la variable de entorno con Variable_Name:
```
SHOW VARIABLESWHERE Variable_NameLIKE '%dir';COPIA EL CÓDIGO
```
![[study_drive/other/SQL/imgs/Pasted image 20240716165239.png]]Este comando mostrará todas las variables de entorno que acaban con `dir`. La variable que indica el camino hacia el directorio donde están almacenadas las bases de datos es `datadir`.

11. En dicho directorio, encontrarás los siguientes elementos:
![[study_drive/other/SQL/imgs/Pasted image 20240716165250.png]]
Existe un subdirectorio para cada base.

12. La inicialización de esta variable `datadir` está en el archivo `my.ini`:
![[study_drive/other/SQL/imgs/Pasted image 20240716165314.png]]
13. Para eliminar una base, basta ejecutar el comando:

```sql
DROP DATABASE base2;COPIA EL CÓDIGO
```

14. También se puede eliminar a través del asistente de Workbench. Para ello, haz clic con el botón derecho del mouse sobre la base de datos que será excluida y escoge la opción **Drop Schema...**:
 ![[study_drive/other/SQL/imgs/Pasted image 20240716165328.png]]