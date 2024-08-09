Uno de los principales deberes o de las principales funciones del DBA es auxiliar al analista digamos, en el mejoramiento, por decirlo así, del desempeño de sus consultas, porque muchas veces cuando las consultas son un poco más rebuscadas o más específicas, entonces puede haber una demora en la ejecución, en el tiempo de ejecución.

EXPLAIN FORMAT=JSON dir /G;

Llegó la hora de que sigas todos los pasos realizados por mí durante esta clase. Si ya lo has hecho ¡Excelente! Si todavía no lo has hecho, es importante que ejecutes lo que fue visto en los vídeos para que puedas continuar con la próxima aula.

1. Selecciona la base **jugos_ventas**, y crea un nuevo script MySQL.
    
2. Digita las siguientes consultas:
```
SELECT A.CODIGO_DEL_PRODUCTOFROM tabla_de_productos A; SELECT A.CODIGO_DEL_PRODUCTO, C.CANTIDADFROM tabla_de_productos A INNERJOIN items_facturas CON A.CODIGO_DEL_PRODUCTO = C.CODIGO_DEL_PRODUCTO; SELECT A.CODIGO_DEL_PRODUCTO,YEAR(B.FECHA_VENTA)AS ANO,C.CANTIDADFROM tabla_de_productos A INNERJOIN items_facturas CON A.CODIGO_DEL_PRODUCTO = C.CODIGO_DEL_PRODUCTO INNERJOIN facturas BON C.NUMERO = B.NUMERO; SELECT A.CODIGO_DEL_PRODUCTO,YEAR(B.FECHA_VENTA)AS ANO, SUM(C.CANTIDAD)AS CANTIDADFROM tabla_de_productos A INNERJOIN items_facturas CON A.CODIGO_DEL_PRODUCTO = C.CODIGO_DEL_PRODUCTO INNERJOIN facturas BON C.NUMERO = B.NUMERO GROUPBY A.CODIGO_DEL_PRODUCTO,YEAR(B.FECHA_VENTA) ORDERBY A.CODIGO_DEL_PRODUCTO,YEAR(B.FECHA_VENTA);COPIA EL CÓDIGO```

