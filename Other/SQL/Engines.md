![[study_drive/Other/SQL/imgs/Pasted image 20240716162654.png]]![[study_drive/Other/SQL/imgs/Pasted image 20240716162726.png]]Vamos a hablar inicialmente sobre MyISAM. Es un mecanismo de almacenamiento que no es transaccional. Es decir, no está diseñado para que varios usuarios estén realizando diversas operaciones en las tablas simultáneamente. Entonces si yo tengo una tabla que no recibe muchas transacciones, una buena opción sería trabajar con MyISAM.

 Él únicamente permite que yo realice el bloqueo a nivel tabla. Tengo que bloquear toda la tabla para poder trabajar sobre ella, y esto me genera que sea una lectura más rápida, de hecho esto es de lo más adecuado cuando estamos trabajando con Data Warehouses.

También pues se recomienda con las tablas que no están continuamente cambiando, sino más bien con tablas que tienen información, contenido fijo, registros fijos o que no sufren muchas alteraciones. La clave externa no soporta el tipo de full text, el tipo de datos full text. Almacena datos de forma más compacta.

Vamos a optimizar espacio de almacenamiento utilizando este mecanismo llamado MyISAM e implementa índices HASH y BTREE que es una ventaja, lo estaremos viendo más adelante en el desarrollo de nuestro entrenamiento. También tenemos diversas variables de ambiente. Una de ellas es key_buffer_size, que determina el tamaño de cache para almacenar los índices de MyISAM.

 Él determina el tamaño de esta memoria cache para que estos índices sean mucho más rápidos. Varían de 8 megas a 4 gigas de acuerdo con el sistema operativo. Si es de 32 bits o 64 bits. Concurrent_insert es una variable que nos dice el comportamiento de las inserciones concurrentes dentro de la tabla MyISAM. ¿Qué quiere decir? La cantidad de inserciones que se pueden estar realizando en ese momento.

Entonces si yo tengo ese parámetro en 0, dice que no puede hacer inserciones simultáneas, está desactivada. Si lo tengo en 1, me permite inserciones simultáneas sin tener un intervalo entre estas inserciones, que son al mismo tiempo y como no es transaccional podemos tener problemas.

Y con 2, inserciones simultáneas con intervalo de datos, me deja un intervalo aunque sea pequeño, de segundos o milisegundos, para hacer las inserciones de diversos datos. También tengo delay_key_write, que ella genera como un atraso o un delay, como se diría en inglés, entre la actualización de índices y el momento que se crea la tabla.

 No son creados al mismo tiempo los índices con sus respectivos registros, sino que espera que todos los registros sean ingresados para posteriormente actualizar los índices. ¿Qué sucede? Es un proceso más lento pero garantiza la integridad de mis datos. Entonces hay más consistencia a cambio de menos rapidez.

 También tengo max_write_lock_count, que es una variable de ambiente que determina el número de grabaciones en las tablas que tendrán precedencia a las diversas lecturas que se realices, entonces prioriza una cierta cantidad de grabaciones que se van a realizar antes de las diversas lectura en las diferentes conexiones. Para eso sirve max_write_lock_count.

Preload_buffer_size, el tamaño de buffer que utilizaremos antes de cargar los índices de cache de las claves de las tablas, que por defecto viene de 32 KB. El uso de estas variables de ambiente, se llevará en la medida que el DBA conozca su entorno y entienda mejor los mecanismos de MyISAM, o sea que el DBA sepa si verdaderamente su tabla con la que está trabajando, funciona mejor con mecanismo MyISAM, considerando las características que nombramos previamente.

Entonces si no tiene muchas transacciones, si es una tabla que no sufre muchas modificaciones, que es más bien una tabla más fija, si es únicamente para lectura, entonces el DBA dice: "podemos trabajar con MyISAM". Es recomendable trabajar sin embargo con la configuración por defecto al usar el mecanismo MyISAM.

Mantenerlo por defecto. No comenzar a modificar mucho estas variables porque todo esto también viene con la experiencia y con el conocimiento del entorno.

Entonces es importante considerar esto con respecto a MyISAM: También MyISAM cuenta con las diferentes aplicaciones que estaremos mencionando a continuación: myisamchk, que analiza, optimiza y repara las tablas MyISAM. Si alguna viene o se corrompe en algún momento, entonces myisamchk reconstruye la tabla.

También tengo myisampack, que crea tablas MyISAM más compactas y serán utilizadas sólo para lectura. No podremos hacer inserciones a la tabla. Lógicamente su desempeño para leerlas va a ser mucho mejor, va a ser de más rápida lectura, sobre todo cuando se realizan muchas consultas a estas tablas, va a ser mucho más rápida.

También contamos con myisam_ftdump, que exhibe la información más completa cuando estamos hablando de campos de tipo texto. Entonces de modo general, esto es lo que queríamos compartir con ustedes con respecto a mecanismos de almacenamiento y en primer lugar, MyISAM.

 En el próximo video estaremos hablando de InnoDB y también de MEMORY que son otros mecanismo comúnmente utilizados en MySQL. Ya nos vemos

![[study_drive/Other/SQL/imgs/Pasted image 20240716162741.png]]
Hola alumnos y alumnas. Ahora vamos a hablar un poco sobre otro mecanismo de almacenamiento que sería InnoDB. Es el mecanismo de almacenamiento por defecto que se trabaja en MySQL actualmente. Es transaccional, o sea, él sí está diseñado para que diversos usuarios estén realizando operaciones en las tablas simultáneamente.

Él tiene soporte transaccional completo. Tiene soporte a claves externas o claves foráneas. El cache de buffer es configurado de forma separada, tanto para la base de datos como para el índice. Entonces esto es muy importante. El bloqueo de tabla se realiza a nivel de línea, no como en el caso de MyISAM que tengo que bloquear toda la tabla para poder realizar cualquier alteración en ella.

Aquí, el bloque de tabla únicamente bloquea la línea o el registro en el cual se tiene que trabajar. Tiene indexación BTREE, estaremos hablando más adelante sobre ello. El backup de la base de datos puede ser realizado online, o sea con el servidor activo. No hay necesidad de parar el servidor para poder hacer un backup. Eso es una gran ventaja.

 Tenemos también las siguientes variables de ambiente propias de InnoDB. En cuanto a las tablas tenemos una que se llama innodb_data_file_path, que determina el camino y el tamaño máximo del archivo dentro del sistema donde estaremos almacenando la información.

 Entonces ahí, nosotros modificamos y establecemos en cuál directorio va a quedar almacenado y el tamaño máximo de este archivo. El innodb_data_home_dir, entonces el camino común del directorio de todos los archivos innoDB. Cuando lo especificamos graba todo al interior de este directorio.

Si no, por defecto, él lo va a guardar en directorio que se llama mysqldata. E innodb_file_per_table, él separa el almacenamiento de sus datos y de sus índices. Digamos por defecto él puede almacenar sus datos y su respectivos índices de forma compartida, pero al establecer este parámetro al darle un valor, él separa al almacenamiento de los datos y de sus respectivos índices.

Ahora en cuanto a desempeño, tenemos la variable innodb_buffer_pool_size, que determina el tamaño de almacenamiento que InnoDB utilizará para almacenar índices y datos en cache. Innodb_flush_log_at_trx_commit determina la frecuencia con que el buffer de lógica-in es habilitado para el disco. entonces este buffer lo va habilitando.

En la medida que lo utilicemos, después de un lapso él va a ser descargado al disco duro. Entonces es una variable con la cual podemos trabajar para mejorar el desempeño. Tenemos innodb_log_file_size, que es el tamaño en bytes con el cual se crearán archivos lógica-in, entonces por defecto él va creando archivos lógica-in de 5MB.

No quiere decir que para todos los archivos de log in va a ser 5 MG sino que va a creando de 5 en 5 MG cada archivo de lógica-in. Finalmente tenemos

![[study_drive/Other/SQL/imgs/Pasted image 20240716162812.png]]

MEMORY. MEMORY es un mecanismo de almacenamiento que crea tablas en la memoria RAM, no quedan en disco. Entonces eso quiere decir que siempre que se reinicialice, se va a borrar.

Entonces si nosotros queremos trabajar con MEMORY debemos reinicializar las tablas cada vez que estemos trabajando con estos datos. No soporta clave externa. No podemos trabajar con clave externa. En cambio nos da acceso muy rápido a la información. Claro, porque los datos están ahí disponibles en RAM, entonces es muy rápido acceder a esta información.

Ellos como les mencione, necesitan ser reinicializados juntos con el servidor toda la información se pierde cada vez que se para el servidor, entonces ya se sale toda la información de la memoria RAM. Tiene bloqueo únicamente a nivel de tabla, al igual que MyISAM, la única que tiene bloque a nivel de línea es InnoDB, que es el mecanismo de almacenamiento por defecto de MySQL 8.0.

Utiliza índices HASH por defecto y BTREE. El formato de línea es de longitud física. Claro, como estamos trabajando con memoria RAM, no podemos permitir que trabaje con BIG LONG, BLOB o con TEXT. Porque pueden ser muchísimos datos. Entonces lógicamente no trabajaría con este tipo de dato. Ni BLOB ni TEXT.

Eso es a grandes rasgos, lo que atañe a MEMORY, el mecanismo de almacenamiento MEMORY y también al mecanismo de almacenamiento InnoDB. En el próximo video estaremos entonces ya entrando un poco más en este mecanismo. Vamos a trabajar más con Workbench y mostrarles específicamente cómo podemos crear tablas configurando estos parámetros.