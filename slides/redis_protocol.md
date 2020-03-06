### Protocolo de Redis

Como vimos en el módulo 1, Redis no es más que una base de datos en memoria que acepta conexiones TCP 
de un cliente, recibe un comando y lo ejecuta.

^^^^^^

#### Protocolo de Redis

El protocolo que utiliza Redis para comunicarse con los clientes se llama _REdis Serialization Protocol (RESP)_.

En esta sección vamos a ver cómo funciona.

^^^^^^

#### 💻️ Protocolo de Redis

Vamos a enviar el comando `PING` a redis utilizando el protocolo de Redis "a pelo":

```bash 
> echo -e "*1\r\n\$4\r\nPING\r\n" | nc  127.0.0.1 6379
+PONG
``` 

^^^^^^

#### 💻️ Protocolo de Redis

Crearemos una cadena con [`SET`](https://redis.io/commands/set) y usaremos [`INCR`](https://redis.io/commands/incr)
para aumentar su valor:

```bash
> echo -e "*3\r\n\$3\r\nset\r\n\$5\r\nmykey\r\n\$1\r\n1\r\n" | nc 127.0.0.1 6379
+OK
> echo -e "*2\r\n\$4\r\nINCR\r\n\$5\r\nmykey\r\n" | nc 127.0.0.1 6379
:2
```

^^^^^^

#### 💻️ Protocolo de Redis

Si enviamos un comando que no existe, recibiremos un error:

```bash
> echo -e "*2\r\n\$3\r\ngot\r\n\$3\r\nfoo\r\n" | nc 127.0.0.1 6379
-ERR unknown command 'got'
```

^^^^^^

#### 💻️ Protocolo de Redis

Podemos enviar varias comandos en una petición:

```bash
> echo -e "*3\r\n\$3\r\nset\r\n\$3\r\nfoo\r\n\$3\r\nbar\r\n*2\r\n\$3\r\nget\r\n\$3\r\nfoo\r\n" | nc 127.0.0.1 6379
+OK 
$3 
bar
```

^^^^^^

#### 💻️ Protocolo de Redis

🤔

^^^^^^

#### 💻️ Protocolo de Redis

Empecemos por el primer comando:

```bash 
*1\r\n\$4\r\nPING\r\n
``` 

^^^^^^

#### 💻️ Protocolo de Redis

```bash 
*1\r\n
\$4\r\n
PING\r\n
``` 

^^^^^^

#### 💻️ Protocolo de Redis

* `*1`: es la longitud del array. En este caso, como sólo enviamos un comando (`PING`) el array tiene longitud 1
* `\r\n`: es el separador que se utiliza en RESP, lo veremos detrás de cada fragmento que compone el protocolo
* `\$4\r\n`: longitud del primer (y único) elemento del array. En este caso le estamos diciendo que se trata de una cadena
  de 4 caracteres. El caracter `\` delante del `$` es para escaparlo y que no lo interprete el comando `echo`
^^^^^^

#### 💻️ Protocolo de Redis
* `PING\r\n`: La cadena en sí (nuestro comando)

^^^^^^

#### 💻️ Protocolo de Redis

Veamos qué ocurre con el segundo comando `SET mykey 1`:

```bash
*3\r\n
\$3\r\n
set\r\n
\$5\r\n
mykey\r\n
\$1\r\n
1\r\n
```   

^^^^^^

#### 💻️ Protocolo de Redis

* `*3\r\n`: le vamos a enviar un array de tres elementos: [`SET`, `mykey`, `1`]
* `\$3\rªn`: el primer elemento es una cadena de longitud 3
* `set\r\n`
* `\$5\r\n`: el siguiente elemento es una cadena de longitud 5
* `mykey\r\n`
* `\$1\r\n`: el siguiente elemento es una cadena de longitud 1
* `1\r\n`

^^^^^^

#### 💻️ Protocolo de Redis: pipelines

Permite enviar varios comandos de Redis en una misma conexión, evitando realizar una conexión por comando.


^^^^^^

#### 💻️ Protocolo de Redis: pipelines

Creamos un fichero `pipeline.txt`:

```bash
HMSET "Sir Lancelot" cualidad valiente color_favorito azul
HMSET "Sir Robin" cualidad Not-Quite-So-Brave-as-Sir-Lancelot color_favorito desconocido
HMSET "Sir Galahad" cualidad castidad color_favorito "not es azul"
HMSET "Sir Arthur" cualidad liderazgo color_favorito desconocido
HMSET "Sir Bedevere" cualidad sabidura color_favorito desconocido
```

^^^^^^

#### 💻️ Protocolo de Redis: pipelines

Convertimos el final de línea de unix a dos:

```bash
> unix2dos pipeline.txt
```

^^^^^^

#### 💻️ Protocolo de Redis: pipelines

```bash
> cat pipeline.txx | redis-cli --pipe 
All data transferred. Waiting for the last reply...
Last reply received from server.
errors: 0, replies: 5
> redis-cli HGETALL "Sir Arthur"
1) "cualidad"
2) "liderazgo"
3) "color_favorito"
4) "desconocido"
```


^^^^^^

#### 💻️ Protocolo de Redis

[Más información](https://redis.io/topics/protocol)