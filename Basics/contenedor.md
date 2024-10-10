
# ¿Qué es un contenedor?

Cada contenedor tiene su propio sistema de archivos, procesos, variables de entorno, etc., lo que significa que:

- __Aislamiento de procesos__: Los procesos que corren dentro de un contenedor no pueden ver o interactuar con los procesos de otro contenedor.
- __Aislamiento de archivos__: Cada contenedor tiene su propio sistema de archivos independiente. Un contenedor no puede acceder directamente al sistema de archivos de otro contenedor a menos que los contenedores compartan un volumen.
- __Aislamiento de red__: Cada contenedor tiene su propio stack de red, es decir, su propia interfaz de red y puertos.


## Conexión entre Contenedores mediante una Red de `Docker Compose`

Docker Compose, además de gestionar los contenedores, también se encarga de crear una red interna que conecta los contenedores entre sí. Esta red permite que los contenedores se comuniquen utilizando nombres de servicio definidos en el archivo docker-compose.yml.

### Red Automática en Docker Compose

Cuando corres `docker compose up`, Docker Compose crea automáticamente una red (por defecto, llamada algo como `mi_humilde_proyecto_default`, donde `mi_humilde_proyecto` es el nombre de tu proyecto o directorio). Todos los servicios definidos en docker-compose.yml se conectan a esta red, lo que les permite comunicarse entre ellos.


Por ejemplo, si tienes los siguientes servicios:

```yml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/mydatabase
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydatabase
```

## ¿Cómo se Comunican los Contenedores?

- El contenedor llamado `web` puede conectarse al contenedor `db` usando el nombre de servicio `db` como hostname. Por ejemplo, en tu configuración de Flask, puedes usar:

```sh
DATABASE_URL=postgresql://user:password@db:5432/mydatabase
```

Aquí, `db` es el nombre del servicio en Docker Compose, y actúa como el hostname del contenedor de la base de datos PostgreSQL. Docker Compose resuelve este nombre usando la red interna.

> [!NOTE] 
> No necesitas preocuparte por las direcciones IP de los contenedores, ya que Docker Compose se encarga de resolver los nombres de servicio.

## Volúmenes y Persistencia de Datos

- Si bien los contenedores están aislados, también pueden compartir datos a través de __volúmenes__. 
- Un volumen es un almacenamiento persistente que Docker monta dentro de los contenedores. Por ejemplo, en tu configuración:

```yml
volumes:
  postgres_data:/var/lib/postgresql/data
```

- __Este volumen hace que los datos de la base de datos sean persistentes__, incluso si el contenedor de PostgreSQL es eliminado y vuelto a crear. 
- Los volúmenes te permiten compartir datos entre el host y los contenedores o entre diferentes contenedores.

## Puertos y Acceso desde el Exterior

Si bien los contenedores están aislados, puedes exponer servicios hacia el exterior mediante el mapeo de puertos. Esto es lo que permite que puedas acceder a servicios desde tu máquina local o cualquier otro dispositivo:

```yml
ports:
  - "5000:5000"
```

Este mapeo significa que el puerto `5000` en tu máquina host está conectado ↔️ al puerto `5000` en el contenedor web. De esta forma, puedes acceder a la aplicación Flask desde tu navegador en http://localhost:5000.
