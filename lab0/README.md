# Laboratorio 0 - Redes y sistemas distribuidos

**Santiago Denis - 2026**  

## Respuestas

Habra un encabezado h3 por cada una de las 5 preguntas obligatorias, con uno extra para la tarea estrella.

---

> ### Pregunta 1: ¿Cómo sabe el cliente que terminó de recibir la respuesta?

 Sabemos que, como usamos para este lab HTTP/1.0, el servidor cierra la conexion cuando termina la respuesta. En las filminas se explica que ``recv()`` devuelve un string vacio cuando no hay mas datos.  Como el cliente llama a ``recv()`` para recibir los datos, cuando este devuelve un string vacio va a saber que ya termino de recibir la respuesta.

---

> ### Pregunta 2: ¿Qué parte del comportamiento depende de TCP y cuál de HTTP?

 TCP se encarga de todo lo relacionado a la conexion y transporte de datos entre el cliente y el servidor. No solo establece la network por donde se conectan, sino que transporta la informacion de una parte a la otra, siempre asegurando que el pasaje de imformacion es seguro. Ademas, detecta tambien el cierre de conexion.
 Por otro lado, HTTP se encarga de establecer la forma en la que ese mensaje es mostrado; es decir, el contenido del mensaje. Utilizando ciertas reglas de formato (header/body), metodos (GET/POST y demas verbos), status (200, 404, 301, etc).

---

> ### Pregunta 3: ¿Qué pasaría si el servidor no cerrara la conexión después de enviar la respuesta?

 Si el servidor no cerrara la conexion, el ``recv()`` jamas devolveria el string vacio. Por lo tanto, el cliente no sabria cuando termina la respuesta, entonces se quedaria bloqueado esperando mas datos.

---

> ### Pregunta 4:  ¿Por qué la IP obtenida para el mismo hostname podría ser distinta entre ejecuciones (o entre máquinas)?
  
 Un mismo dominio puede apuntar a varios servidores, que pueden cambiar segun cercania o red. Por lo tanto, los servidores DNS pueden devolver distintas IP para dirigir al cliente al servidor mas cercano.

---

> ### Pregunta 5: ¿Por qué DNS suele usar UDP y HTTP usa TCP? ¿Qué pasaría si usáramos TCP para las consultas DNS en este cliente?

 DNS requiere UDP porque no importa establecer una conexion entre el cliente y el servidor, ya que el intercambio es minimo (de una query a una respuesta). Simplemente, el cliente va a pedir una ip, el DNS la va a buscar y se la va a dar.
 En cambio, HTTP usa TCP porque justamente necesita establecer una conexion entre el cliente/servidor. Necesita un orden en los datos y tambien necesita transmitir mensajes que pueden ser grandes. Como hay un va y viene de informacion entre el cliente y el servidor, TCP es necesario.
 Se puede usar TCP en lugar de UDP para el DNS, pero va a ser mas lento y costoso para un intercambio de datos que no necesita los beneficios del TCP; seria innecesario, pero se puede.

---

> ### Tarea Estrella

  Los dominios como ``http://.tw/`` o ``https://.la^2`` no deberian funcionar, pues DNS solo acepta caracteres ASCII. Pero funcionan, porque todos los dominios que tienen caracteres especiales (caracteres bajo el estandar Unicode) son transformados a ASCII antes de ser enviados a un servidor de DNS. Para hacer esta transformacion, se utiliza el esquema Punycode.
  Los navegadores realizan esta conversion automaticamente antes de enviar alguna query al DNS. En python, el proceso de transformacion se puede realizar usando el encoding idna. [Ver documentacion de idna](https://docs.python.org/2.4/lib/module-encodings.idna.html)
