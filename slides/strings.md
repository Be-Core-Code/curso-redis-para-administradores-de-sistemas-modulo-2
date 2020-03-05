### Strings

Almacena secuencias de bytes.

Se usan para almacenar:

* Caracteres de text
* Enteros
* Números con coma flotante

^^^^^^

#### Strings

El comando que se utiliza para guardar un `STRING` es [`SET`](https://redis.io/commands/set). 

Para recuperar su valor usamos [`GET`](https://redis.io/commands/get).

```redis-cli
redis-cli > SET nombre "Curso de Redis"
OK
redis-cli > GET nombre
"Alfonso" 
```

^^^^^^

#### Strings

Para borrar una clave utilizamos el comando [`DEL`](https://redis.io/commands/del).

```redis-cli
redis-cli > DEL nombre
(integer) 1 
```
^^^^^^

#### Strings

Como os he mencionado en la primera diapositiva de esta sección, el tipo `STRING` también se usa para 
almacenar números:

```redis-cli
redis-cli > set numero_alumnos 10
OK 
redis-cli > GET numero_alumnos
"10" 
```

^^^^^^

#### Strings

Algunas operaciones que podemos hacer sobre números:

* [`INCR`](https://redis.io/commands/incr): incrementa en 1 el valor de la clave
* [`DECR`](https://redis.io/commands/decr): reduce en 1 el valor de la clave

```redis-cli
redis-cli > INCR numero_alumnos 
(integer) 11
redis-cli > DECR numero_alumnos
(integer) 10
```

^^^^^^

#### Strings

* [`INCRBYFLOAT`](https://redis.io/commands/incrbyfloat): incrementa un número en coma flotante en la cantidad indicada

```redis-cli
redis-cli > SET edad_promedio 35.56 
OK
redis-cli > INCRBYFLOAT edad_promedio 0.76
"36.32"
```

^^^^^^

#### Strings

Redis también nos facilita comandos para manipular partes o bytes de los `STRINGS`:

* [`APPEND`](https://redis.io/commands/append) concatena el `STRING` a la clave indicada
* [`GETRANGE`](https://redis.io/commands/getrange) extrae una parte del `STRING`

```redis-cli
redis-cli > SET unvalor 10876
OK 
redis-cli > APPEND unvalor 987
(integer) 8
redis-cli > get unvalor
"10876987"
redis-cli > GETRANGE unvalor 3 6
"7698"
```

^^^^^^

#### Strings

* [`GETBIT`](https://redis.io/commands/getbit) trata el `STRING` como una cadena de bits y devuelve el bit solicitado 
* [`SETBIT`](https://redis.io/commands/setbit) trata el `STRING` como una cadena de bits y fija el bit solicitado al valor indicado
* [`BITOP`](https://redis.io/commands/bitop) realiza operaciones a nivel de bit entre dos claves
* [`BITPOS`](https://redis.io/commands/bitpos) devuelve la posición del primer bit que tiene el valor solicitado 

^^^^^^

#### Strings

En el [ebook de RedisLabs](https://redislabs.com/ebook/part-2-core-concepts/chapter-3-commands-in-redis/3-1-strings/)
podéis profundizar un poco más sobre esta estructura de datos.