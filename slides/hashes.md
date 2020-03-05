### Hashes

Estructura que almacena una relación entre campos y valores, de manera a similar a los diccionarios o mapas que existen
en diferentes lenguajes de programación.

Tanto las claves como los valores de un Hash deben ser `STRINGS`.

El número máximo de parejas clave|valor que se pueden almacenar en un Hash es 2^32-1.

^^^^^^
#### 💻️ Hashes

Para crear un nuevo Hash definiendo múltiples parejas clave|valor utilizamos el comando [`HMSET`](https://redis.io/commands/hmset).

```redis-cli
redis-cli > HMSET "Lancelot" mision: "Buscar el santo grial" color_favorito "azul"
OK  
```

^^^^^^
#### 💻️ Hashes

Para crear añadir *una* clave|valor utilizamos el comando [`HSET`](https://redis.io/commands/hset).

```redis-cli
redis-cli > HSET "Lancelot" mision: "Buscar el santo grial"
OK 
redis-cli > HSET color_favorito "azul"
OK
  
```

^^^^^^
#### 💻️ Hashes

Para recuperar múltiples claves de un Hash utilizamos el comando [`HMGET`](https://redis.io/commands/hmget).

```redis-cli
redis-cli > HMGET "Lancelot" color_favorito
"azul"
redis-cli > HMGET "Lancelot" color_favorito mision
"azul"
"Buscar el santo grial"
```

^^^^^^
#### 💻️ Hashes

Para recuperar **una** clave de un Hash utilizamos el comando [`HGET`](https://redis.io/commands/hget).

```redis-cli
redis-cli > HMGET "Lancelot" color_favorito
"azul"
redis-cli > HMGET "Lancelot" color_favorito mision
"azul"
"Buscar el santo grial"
```

^^^^^^
#### 💻️ Hashes

Para recuperar **todas** las claves de un Hash utilizamos el comando [`HGETALL`](https://redis.io/commands/hgetall) .

```redis-cli
redis-cli > HGETALL "Lancelot"
1) "mision:"
2) "Buscar el santo grial"
3) "color_favorito"
4) "azul"
```

^^^^^^
#### 💻️ Hashes

Podemos averiguar si una clave está definida en un Hash usando el comando [`HEXISTS`](https://redis.io/commands/hexists)

```redis-cli
redis-cli > HEXISTS Lancelot color_favorito
(integer) 1
redis-cli > HEXISTS Lancelot fecha_nacimiento
(integer) 0 
```

^^^^^^
#### 💻️ Hashes

Podemos borrar una clave de un hash usando el comando [`HDEL`](https://redis.io/commands/hdel)

```redis-cli
redis-cli > HDEL Lancelot mision
(integer) 1
redis-cli > HGETALL "Lancelot"
1) "color_favorito"
2) "azul"
```

^^^^^^

#### 💻️ Hashes

Al igual que nos ocurría con los Conjuntos, si ejecutamos [`HGETALL`](https://redis.io/commands/hgetall) sobre un Hash
que tiene millones de claves

**el servidor puede tardar tiempo en obtener la respuesta 
y durante este tiempo no responderá otras peticiones**.

^^^^^^

#### 💻️ Hashes

Para evitar este problema utilizamos el comando [`HSCAN`], que nos devuelve un cursor para continuar obteniendo resultados
seguido del primer subconjunto de claves.

```redis-cli
redis-cli >
HSCAN Lancelot 0
1) "0"
2) 1) "color_favorito"
   2) "azul"
   3) "mision"
   4) "Buscar el santo grial" 
```

^^^^^^
#### Hashes

[Comandos relacionados con hashes](https://redis.io/commands#hash)

En el ebook de RedisLabs
podéis profundizar un poco más sobre esta estructura de datos.

* [Sección 1.2.4](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/1-2-4-hashes-in-redis/)
* [Sección 3.4](https://redislabs.com/ebook/part-2-core-concepts/chapter-3-commands-in-redis/3-4-hashes/)
