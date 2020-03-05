### Conjuntos ordenados

Es un conjunto en el que los elementos del mismo tiene un peso asociado. 

Los elementos del conjunto pueden ordenarse por el valor del peso.

^^^^^^
#### 💻️ Conjuntos ordenados

Para crear un conjunto ordenado utilizamos el comando  [`ZADD`](https://redis.io/commands/zadd)

```redis-cli
redis-cli > ZADD caballeros 0 Arturo 0 Lancelot 0 Galahad 0 Bedevere
(integer) 4
```

^^^^^^
#### 💻️ Conjuntos ordenados

Hemos creado todos los elementos del conjunto con el mismo peso: 0

Podemos usar el comando [`ZINCRBY`](https://redis.io/commands/zincrby) para incrementar el valor del contador:

```redis-cli
redis-cli > ZINCRBY caballeros 5 Arturo
"5"
redis-cli > ZINCRBY caballeros -1 Bedevere
"5" 
```

notes:

El incremento puede ser un número decimal, por ejemplo:

```redis-cli
redis-cli > ZINCRBY caballeros 0.5 Lancelot
"5"
```


^^^^^^
#### 💻️ Conjuntos ordenados

Para listar los elementos del conjunto utilizamos el comando [`ZRANGE`](https://redis.io/commands/zrange) o 
[`ZREVRANGE`](https://redis.io/commands/zrevrange) para mostrar los elementos:

```redis-cli
redis-cli > ZRANGE caballeros 0 -1
1) "Bedevere"
2) "Galahad"
3) "Lancelot"
5) "Arturo"
redis-cli > ZREVRANGE caballeros 0 -1
1) "Arturo"
2) "Lancelot"
3) "Galahad"
4) "Bedevere" 
```

^^^^^^
#### 💻️ Conjuntos ordenados

Utilizando la opción WITHSCORES podemos ver los valores de los pesos:

```redis-cli
redis-cli > ZRANGE caballeros 0 -1 WITHSCORES
1) "Bedevere"
2) "-1"
3) "Galahad"
4) "0"
5) "Lancelot"
6) "0"
7) "Arturo"
8) "5"
```

^^^^^^
#### 💻️ Conjuntos ordenados


```bash
redis-cli > ZREVRANGE caballeros 0 -1 WITHSCORES
1) "Arturo"
2) "5"
3) "Lancelot"
4) "0"
5) "Galahad"
6) "0"
7) "Bedevere"
8) "-1" 
```

^^^^^^
#### 💻️ Conjuntos ordenados

Podemos eliminar elementos del conjunto con el comando [`ZREM`](https://redis.io/commands/zrem)

```redis-cli
redis-cli > ZREM caballeros Bedevere
(integer) 1 
```

^^^^^^
#### 💻️ Conjuntos ordenados

Los comandos [`ZRANK`](https://redis.io/commands/zrank) y [`ZREVRANK`](https://redis.io/commands/zrevrank)
nos indican la posición en el ranking de un elemento del conjunto:

```redis-cli
redis-cli > ZRANK caballeros Lancelot
(integer) 2
redis-cli > ZREVRANK caballeros Lancelot
(integer) 1 
```

^^^^^^
#### 💻️ Conjuntos ordenados

El comando [`ZSCORE`](https://redis.io/commands/zscore) nos devuelve el peso de un elemento del conjunto

```redis-cli
redis-cli > ZSCORE caballeros Arturo
"5" 
```
^^^^^^

#### Conjuntos: Más información

[Comandos relacionados con conjuntos ordenados](https://redis.io/commands#sorted_set)

En el ebook de RedisLabs
podéis profundizar un poco más sobre esta estructura de datos.

* [Sección 1.2.5](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/1-2-5-sorted-sets-in-redis/)
* [Sección 3.5](https://redislabs.com/ebook/part-2-core-concepts/chapter-3-commands-in-redis/3-5-sorted-sets/)

