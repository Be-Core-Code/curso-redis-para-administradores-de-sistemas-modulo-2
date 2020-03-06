### Listas

Una lista es una secuencia ordenada de `STRINGS`.

^^^^^^

#### 💻️ Listas

Podemos crear listas usando los comandos [`RPUSH`](https://redis.io/commands/rpush) y [`LPUSH`](https://redis.io/commands/lpush):


```redis-cli
redis-cli > RPUSH alumnos "Meganito López" "Fulanito Perez"
(integer) 2   
redis-cli > LPUSH alumnos "Futanito Jimenez" 
(integer) 3   
```

notes:

Estos dos comandos devuelve en número de elementos que tiene la lista después de realizar la operación.

^^^^^^

#### 💻️ Listas

Para ver los elementos de una lista utilizamos el comando [`LRANGE`](https://redis.io/commands/lrange)

```redis-cli
redis-cli > LRANGE alumnos 0 -1
1) "Futanito Jimenez"
2) "Meganito López"
3) "Fulanito Perez" 
```

^^^^^^

#### 💻️ Listas

Con el comando [`LINSERT`](https://redis.io/commands/linsert) podemos añadir un elemento antes o depués de otro:

```redis-cli
redis-cli > LINSERT alumnos AFTER "Futanito Jimenez" "Miguel de Unamuno"
(integer) 4 
redis-cli > LRANGE alumnos 0 -1
1) "Futanito Jimenez"
2) "Miguel de Unamuno"
3) "Meganito López"
4) "Fulanito Perez" 
```

^^^^^^

#### 💻️ Listas

Con el comando [`LINDEX`](https://redis.io/commands/lindex) podemos obtener los elementos de la lista por posición:

```redis-cli
redis-cli > LINDEX alumnos 1
"Miguel de Unamuno"
 
```

^^^^^^

#### 💻️ Listas

Para sacar elementos de una lista podemos utilizar los comandos [`RPOP`](https://redis.io/commands/rpop) y 
[`LPOP`](https://redis.io/commands/lpop)

```redis-cli
redis-cli > LPOP alumnos 
"Futanito Jimenez"
redis-cli > RPOP alumnos 
"Fulanito Perez" 
redis-cli > LRANGE alumnos 0 -1
1) "Miguel de Unamuno"
2) "Meganito López"
```
^^^^^^

#### 💻️ Listas

Podemos usar el comando [`LSET`](https://redis.io/commands/lset) para asignar un valor a un elemento de la lista:

```redis-cli
redis-cli > LSET alumnos 0 "Miguel de Unamuno y Jugo"
OK
redis-cli > LRANGE alumnos 0 -1
1) "Miguel de Unamuno y Jugo"
2) "Meganito López"
```

^^^^^^

#### 💻️ Listas

Los comandos [`BLPOP`](https://redis.io/commands/blpop) y [`BRPOP`](https://redis.io/commands/brpop) son la versión
con bloqueo de los comandos [`LPOP`](https://redis.io/commands/lpop) y [`RPOP`](https://redis.io/commands/rpop).

Son muy útiles para implementar colas de trabajos.

^^^^^^

#### 💻️ Listas: cola de trabajos

Dos clientes que se conectan a redis: `worker-1` y `worker-2`. Estos clientes ejecutan el comando 
[`BLPOP`](https://redis.io/commands/blpop) y quedan en espera, bloqueados hasta que aparezca algún elemento
en la lista `jobs`:

```redis-cli
redis-cli (worker-1) > BRPOP jobs

queda en espera... no ocurre nada
```

```redis-cli
redis-cli (worker-2) > BRPOP jobs

queda en espera... no ocurre nada
```

^^^^^^

#### 💻️ Listas: cola de trabajos

Abrimos ahora una tercera conexión (`client`) que va a añadir tres trabajos a la cola:

```redis-cli
redis-cli (client) > LPUSH jobs job1 job2 job3 
(integer) 3
``` 

^^^^^^

#### 💻️ Listas: cola de trabajos

Al ejecutar este comando, el cliente `worker-1` que estaba bloqueado esperando a que apareciese algún elemento en la lista, 
recibe el primer elemento y lo saca:

```redis-cli
redis-cli (worker-1) >BRPOP jobs 0
1) "jobs"
2) "job1"
(317.41s) 
```

Se ejecuta el comando de `worker-1` antes que el de `worker-2` porque `worker-1` ejecutó [`BRPOP`](https://redis.io/commands/brpop) 
antes que `worker-2`.

^^^^^^

#### 💻️ Listas: cola de trabajos

Después de que el cliente `worker-1` haya ejecutado el pop, el cliente `worker-2` extrae el nuevo primer elemento de la lista (jobs2):

```redis-cli
redis-cli >> BRPOP jobs 0
1) "jobs"
2) "job2"
(51.95s) 
``` 

^^^^^^

#### 💻️ Listas: cola de trabajos

La lista `jobs` todavía tiene un elemento dentro (`job3`) que nadie ha procesado todavía:

```redis-cli
redis-cli > LRANGE jobs 0 -1
1) "job3" 
```

^^^^^^

#### 💻️ Listas: rendimiento

Existen tres implementaciones de las listas en redis:

* **Listas enlazadas**: tienen un coste de almacenamiento de 40+ bytes por entrada.
* **Ziplists**: almacena la lista en un bloque de memoria y utiliza _offsets_ en lugar de punteros
* **quicklist**: es un híbrido de los dos: es una lista enlazada de ziplists
  * `[ziplist 0] <-> [ziplist 1] <-> ... <-> [ziplist N]`

notes:

Si queremos almacenar un millón de enteros en una lista enlazada, ocuparíamos 4 MB con los datos y más de 40 MB
con los punteros. Usando Ziplists el espacio adicional se reduciría a 1 MB.

El problema que tiene un ziplist es que se trata de un bloque de memoria solido; con sólido entendemos que 
los elementos se almancenan uno detrás de otro. El problema de esta estructura es que si queremos añadir o quitar elementos
de la zona central de la lista, las operaciones son delicadas. 


Más información en [este enlace](https://matt.sh/redis-quicklist)

^^^^^^

#### 💻️ Listas: rendimiento

`[ziplist 0] <-> [ziplist 1] <-> ... <-> [ziplist N]`

* `list-max-ziplist-size`: tamaño máximo del `ziplist`. Cuando se rebasa, se crea una 
  nueva `ziplist`
* Este tamaño puede ser un número exacto de elementos o un número negativo que representa el tamaño:

```bash
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
```
   
notes:

Esta información está sacada del fichero [redis.conf](http://download.redis.io/redis-stable/redis.conf).

^^^^^^

#### 💻️ Listas: rendimiento

`[ziplist 0] <-> [ziplist 1] <-> ... <-> [ziplist N]`

* `list-compress-depth`: activa la compresión de los `ziplists` de las listas
* `list-compress-depth = 0`: valor por defecto (sin compresión)

^^^^^^

#### 💻️ Listas: rendimiento
* `list-compress-depth = 1`: se comprimen todos los nodos excepto el primero y el último
* `list-compress-depth = 2`: se comprimen todos los nodos excepto los dos primeros y el los dos últimos
* `list-compress-depth = 3`: se comprimen todos los nodos excepto los tres primeros y el los tres últimos
* 

[Más información](https://docs.redislabs.com/latest/ri/memory-optimizations/enable-compression-for-list/)   

^^^^^^

#### Listas

[Comandos relacionados con listas](https://redis.io/commands#list)

En el ebook de RedisLabs
podéis profundizar un poco más sobre esta estructura de datos.

* [Sección 1.2.2](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/1-2-2-lists-in-redis/)
* [Sección 3.2](https://redislabs.com/ebook/part-2-core-concepts/chapter-3-commands-in-redis/3-2-lists/)

