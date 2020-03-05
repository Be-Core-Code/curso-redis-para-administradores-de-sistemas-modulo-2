### Conjuntos (Sets)

Lista no ordenada de `STRINGS` que no se repiten dentro del conjunto.

En un conjunto caben 2^23 - 1 elementos.

^^^^^^

#### 💻️ Conjuntos

Para añadir elementos a un conjunto utilizamos el comando [`SADD`](https://redis.io/commands/sadd)

```redis-cli
redis-cli > SADD item:5 "barato" "vegetariano" "comida mediterránea"
(integer) 3 
```

^^^^^^

#### 💻️ Conjuntos

Podemos preguntar si un `STRING` es un elemento de un conjunto con [`SISMEMBER`](https://redis.io/commands/sismember)

```redis-cli
redis-cli > SISMEMBER item:5 barato
(integer) 1 
redis-cli > SISMEMBER item:5 caro
(integer) 0
```

^^^^^^

#### 💻️ Conjuntos

Y quitamos elementos de un conjunto con [`SREM`](https://redis.io/commands/srem)

```redis-cli
redis-cli > SREM item:5 barato
(integer) 1
redis-cli > SADD item:5 caro
(integer) 1
```

^^^^^^

#### 💻️ Conjuntos

Podemos saber cuántos elementos tiene un conjunto usando [`SCARD`](https://redis.io/commands/scard)

```redis-cli
redis-cli > SCARD item:5
(integer) 3 
```

^^^^^^

#### 💻️ Conjuntos

Para ver los elementos de un conjunto tenemos dos opciones.

La primera es [`SMEMBERS`](https://redis.io/commands/smembers)

```redis-cli
redis-cli > SMEMBERS item:5
1) "vegetariano"
2) "caro"
3) "comida mediterr\xc3\xa1nea" 
```

^^^^^^

#### 💻️ Conjuntos

El problema que tiene [`SMEMBERS`](https://redis.io/commands/smembers) es que lista todos los elementos del conjunto.

Si el conjunto tiene muchos elementos **el servidor puede tardar tiempo en obtener la respuesta 
y durante este tiempo no responderá otras peticiones**.
 
^^^^^^

#### 💻️ Conjuntos

Por eso tenemos una segunda opción: [`SSCAN`](https://redis.io/commands/scan)

La familia de comandos [`SCAN`](https://redis.io/commands/scan) iteran sobre una lista utilizando un cursor:

```redis-cli
redis-cli > SSCAN item:5 0 COUNT 2
1) "1"                            <--- Recibimos el siguiente cursor
2) 1) "vegetariano"               <--- Los elementos que hemos pedido (2)
   2) "caro"```
redis-cli > SSCAN item:5 1 COUNT 2
1) "0"
2) 1) "comida mediterr\xc3\xa1nea"
```

^^^^^^

#### 💻️ Conjuntos

Es aconsejable entender el funcionamiento de 
la familia de comandos [`SCAN`](https://redis.io/commands/scan).

Cuando queremos extraer muchos datos de Redis **es la manera de hacerlo sin bloquear el proceso principal**.


^^^^^^

#### Conjuntos: Más información

[Comandos relacionados con conjuntos](https://redis.io/commands#set)

En el ebook de RedisLabs
podéis profundizar un poco más sobre esta estructura de datos.

* [Sección 1.2.3](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/1-2-3-sets-in-redis/)
* [Sección 3.3](https://redislabs.com/ebook/part-2-core-concepts/chapter-3-commands-in-redis/3-3-sets/)

