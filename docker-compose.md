
# docker-compose

<p align="center">
  <img src="https://github.com/user-attachments/assets/8113ae18-24fc-4d6f-be9d-2a3ca0ecc22f" height="300" />
</p>

Docker Compose es una herramienta que te **permite definir y ejecutar múltiples contenedores** Docker en un solo archivo de configuración.

```yaml
services:
  php:
    build: .
    volumes:
      - .:/app
    working_dir: /app
    command: php ./index.php
```

### Explicación paso a paso:

```yaml
services:
```
Aquí comienzas a definir los **servicios** que formarán parte de tu aplicación.
- **Servicios**: En Docker Compose, un servicio es un contenedor. Puedes definir uno o varios servicios, y cada uno puede tener configuraciones únicas. En este caso, tienes un único servicio llamado `php`, que representará el contenedor PHP.

---
```yaml
php:
```
Este es el nombre del servicio que estás definiendo. En este caso, se llama `php`, pero podrías usar cualquier nombre que desees. 
- **Importante**: Este nombre es solo a nivel interno de Docker Compose y puede usarse para hacer referencia a otros servicios, como bases de datos o servidores web, en caso de tener múltiples servicios.

---
```yaml
build: .
```
Esta línea indica a Docker Compose que debe **construir una imagen Docker** a partir de un `Dockerfile` ubicado en el directorio actual (`.`).
- **Punto (`.`)**: El punto (`.`) indica que el archivo `Dockerfile` se encuentra en el mismo directorio que el archivo `docker-compose.yml`. Docker buscará el `Dockerfile` en esa ubicación y lo usará para construir la imagen del contenedor PHP.
  
  Si quisieras usar una imagen Docker existente en lugar de construir una desde un `Dockerfile`, podrías reemplazar esta línea por `image: php:8.1-cli`.

### Volúmenes 
```yaml
volumes:
  - .:/app
```
<p align="center">
  <em>El <b>-</b> es sintaxis YAML y significa que quieres crear una lista</em>
</p>

Aquí defines un **volumen**. Un volumen en Docker es un mecanismo para montar un directorio del **host (tu máquina local)** dentro del contenedor.
  
  En este caso:
  - **Punto (`.`)**: Se refiere al **directorio actual en tu máquina local**, es decir, el directorio donde se encuentra tu código y archivos del proyecto.
  - **`:/app`**: Es el **directorio dentro del contenedor** donde se montará ese directorio local.

#### Cambios sincronizados

Cualquier cambio que hagas en los archivos dentro de tu directorio local (host) se reflejará automáticamente dentro del contenedor. Esto es útil durante el desarrollo, ya que puedes modificar tu código sin tener que reconstruir o reiniciar el contenedor cada vez.
  
En este ejemplo, el directorio del host será accesible dentro del contenedor bajo la ruta `/app`.

### Ejemplo de flujo de trabajo:

1. Inicias el contenedor con `docker compose up`.
2. Editas tu archivo `index.php` en tu máquina local, añadiendo una nueva línea:
    ```php
    echo "Nueva línea de código\n";
    ```
3. No necesitas detener el contenedor ni volver a construir la imagen. Simplemente recargas o ejecutas el contenedor, y el código actualizado estará disponible de inmediato dentro del contenedor.

### [CONCEPTO 💡] Bind mount

Un __bind mount__ es un tipo de volumen que te permite montar un directorio o archivo del host (tu máquina local) dentro de un contenedor. Cuando usas un bind mount, cualquier cambio que hagas en los archivos dentro de ese directorio en tu máquina local se reflejará inmediatamente dentro del contenedor, y viceversa.

### ¿Cómo funciona un bind mount?

Con un bind mount, Docker NO controla el contenido del volumen; es tu máquina (host) la que proporciona el directorio. Puedes especificar tanto la ruta en el host como la ruta en el contenedor donde quieres que se monte.

### Ejemplo

Imagina que tienes un archivo PHP en el directorio `/home/thais/proyectoLoco` de tu máquina local. Puedes montar este directorio dentro de un contenedor usando un `bind mount`:

```sh
docker run -v /home/user/proyecto:/var/www/html php:8.1-apache
```

- `/home/user/proyectoLoco`: Es el directorio en tu máquina local (host) donde está tu código PHP.
  
- `/var/www/html`: Es el directorio dentro del contenedor donde el servidor web Apache espera encontrar los archivos PHP.
  
- Cualquier cambio que hagas en el código dentro de `/home/user/proyectoLoco` en tu máquina local se verá automáticamente dentro del contenedor en `/var/www/html`.

### Ventajas de los `bind mounts`

1.	Sincronización en tiempo real: Los cambios en el sistema de archivos del host se reflejan automáticamente dentro del contenedor. Esto es útil para desarrollo.
2.	Control total: Puedes acceder y modificar los archivos del host desde el contenedor y viceversa.
3.	Flexibilidad: Puedes montar cualquier directorio o archivo del host en el contenedor sin tener que hacer cambios en el contenedor.

### Desventajas de los `bind mounts`

1. __Dependencia del sistema de archivos del host__: El contenedor depende del sistema de archivos del host, por lo que la portabilidad puede verse afectada si se ejecuta en diferentes entornos (por ejemplo, en otro servidor que no tenga la misma estructura de directorios).
  
2. __Menos aislado__: Si accidentalmente modificas o eliminas archivos en el host, esos cambios se reflejan en el contenedor y viceversa, lo que puede ser riesgoso en producción.

### Diferencia con volúmenes gestionados por Docker

•	__Bind mounts__: Usan directorios específicos en el sistema de archivos del host que defines manualmente.

•	__Volúmenes de Docker__: Docker administra los volúmenes y guarda los datos en una ubicación interna dentro del sistema Docker, lo que proporciona más independencia y control de los datos entre contenedores y el host.

### Ejemplo con un `bind mount`

```yaml

services:
  php:
    image: php:8.1-cli
    volumes:
      - ./mi-proyecto-loco:/app // el bind mount
    working_dir: /app
    command: php index.php

```

El directorio local `./mi-proyecto-loco` se monta en el contenedor en `/app`. Esto significa que cualquier archivo en __mi-proyecto-loco__ estará disponible en el contenedor, y los cambios serán bidireccionales (entre el host y el contenedor).


## Bind Mount VS Watch

Si usas una configuración de **"watch"** o alguna funcionalidad de sincronización nativa de Docker, sería Docker el encargado de gestionar los archivos y monitorear cambios, **sin depender del sistema de archivos del host directamente** como lo hace un **bind mount**.

1. **Bind Mount** (lo que tienes ahora con `- ./mi-proyecto-loco:/app `):
   - **El sistema de archivos del host gestiona la sincronización**: Los archivos del directorio local en tu máquina se montan directamente dentro del contenedor, lo que significa que el sistema de archivos del host es responsable de gestionar esos archivos.
   
   - **Tiempo real**: Los cambios en el host se reflejan instantáneamente en el contenedor y viceversa.
     
   - **Dependencia del host**: Estás directamente enlazando el sistema de archivos de tu máquina local con el contenedor, lo que puede causar problemas si las rutas de los archivos cambian, o si llevas el contenedor a otro entorno donde la estructura de directorios sea diferente.

2. **Watch (sincronización gestionada por Docker)**:
   - **Docker gestiona la sincronización**: En este caso, Docker supervisa los archivos locales y gestiona la sincronización con los archivos dentro del contenedor. Esto significa que Docker maneja la actualización de los archivos en el contenedor en lugar de depender del sistema de archivos del host.
     
   - **Independencia del host**: Con "watch" o alguna forma de sincronización nativa de Docker, los archivos no están directamente "montados" en el contenedor. Docker monitorea los cambios en los archivos locales y sincroniza los cambios dentro del contenedor, pero de una manera más desacoplada.
     
   - **Más control**: Docker puede ofrecer reglas más sofisticadas de cómo sincronizar los archivos, por ejemplo, qué archivos observar o ignorar.

### ¿Cómo funcionaría con `watch` en Docker Compose?

Si intentas poner un `watch` junto a un `bind mount` Docker te dirá que ya existe este y que sudará la polla de trackearlo, por lo que habrá un ✨conflicto✨. Si quieres que **Docker gestione completamente la sincronización** y no depender del sistema de archivos local, deberías **eliminar el bind mount** y dejar que `watch` haga su trabajo.

### Ejemplo con `watch`:

```yaml

services:
  php:
    build: .
    develop:
      watch:
        - action: sync
          path: .
          target: /app
    working_dir: /app
    command: php ./index.php
```

### En este caso:
- **Elimina el bind mount** (`volumes: - .:/app`) para que no haya conflicto entre el sistema de archivos del host y la gestión de Docker.
  
- **Docker gestionará los archivos** usando la funcionalidad `watch`, supervisando el directorio `.` (tu directorio local) y sincronizando los cambios en `/app` dentro del contenedor.

### Cuando usar uno u otro ☝️

👉 **Usar `bind mounts`**: Es útil en desarrollo cuando necesitas sincronización en tiempo real y un entorno simple donde los archivos del host están directamente accesibles dentro del contenedor. Ideal para máquinas locales donde no necesitas alta portabilidad.
  
👉 **Usar `watch` (sincronización gestionada por Docker)**: Es más adecuado si quieres más independencia entre el contenedor y el host, o si trabajas en entornos que podrían tener problemas con bind mounts, como sistemas Windows o si necesitas reglas avanzadas de sincronización.

**Resumen**: Si configuras `watch`, Docker se encargaría de gestionar la sincronización de archivos, **desacoplándote** del sistema de archivos del host. Esto puede ser más robusto y portable en ciertos casos, especialmente en entornos de producción o desarrollo que requieren mayor flexibilidad. Solo asegúrate de **eliminar el bind mount** para evitar conflictos.

---

### ¿Qué hace Docker un volumen?
Cuando defines un volumen de esta manera:
- Docker monta el directorio local (`.`) en el contenedor, en el directorio `/app`. 
- Si haces cualquier cambio en el código en tu máquina local (el directorio que corresponde a `.`), esos cambios se reflejan automáticamente en el contenedor, en la carpeta `/app`.
- No necesitas volver a construir la imagen o reiniciar el contenedor, ya que siempre estará trabajando con los archivos más actualizados de tu máquina.
  
En resumen, con esta configuración, tu código en el contenedor estará **"sincronizado" en tiempo real** con el de tu máquina local, haciendo mucho más ágil el proceso de desarrollo.

## ¿Quieres más volúmenes?

```yaml
volumes:
  - ./src:/app/src  # Montar el directorio src
  - ./config:/app/config  # Montar el directorio config
```
<p align="center">
  <em>Simplemente añade más elementos</em>
</p>

## Ponerle nombre a un volumen

Si quisieras definir un volumen con un nombre específico (no un “bind mount” como el que estamos usando):

```yaml
volumes:
  my_volume:
    external: false
```

Y luego lo usarías en los servicios:
```yaml
services:
  php:
    volumes:
      - my_volume:/app/data
```
---
```yaml
working_dir: /app
```
Aquí defines el **directorio de trabajo** dentro del contenedor. Es decir, cada vez que se ejecuten comandos dentro del contenedor, lo harán desde este directorio.
  
- **En este caso**: El directorio de trabajo es `/app`, que es donde previamente montaste tu código con el volumen. Ahora, cualquier comando ejecutado dentro del contenedor asumirá que se está ejecutando en `/app`.

  Esto evita que tengas que especificar rutas absolutas para tus archivos cada vez que ejecutes comandos.

---
```yaml
command: php ./index.php
```
Esta línea define el **comando** que Docker ejecutará por defecto cuando el contenedor arranque.
  
- **En este caso**:
  - **`php`**: Es el comando que ejecutará el intérprete de PHP.
  - **`./index.php`**: Es el archivo PHP que se ejecutará al iniciar el contenedor. El `./` indica que el archivo `index.php` está en el directorio de trabajo actual, que es `/app` gracias a la instrucción `working_dir`.

#### ¿Qué sucede aquí?
Cada vez que levantes el contenedor, Docker ejecutará automáticamente el comando `php ./index.php`, lo que hará que tu script PHP `index.php` se ejecute dentro del contenedor.

### Resumen del flujo de trabajo:
1. **Se define la versión de Docker Compose** (en este caso, la versión 3).
2. **Se crea un servicio llamado `php`**.
3. El servicio PHP:
   - **Construye la imagen Docker** usando un `Dockerfile` ubicado en el directorio actual.
   - **Monta el código de tu máquina local (host)** en el directorio `/app` dentro del contenedor.
   - **Define el directorio de trabajo** como `/app`, para que los comandos se ejecuten desde ahí.
   - **Ejecuta automáticamente el comando `php ./index.php`**, lo que hará que tu script PHP se ejecute cuando el contenedor arranque.

### ¿Por qué es útil `docker-compose.yml`?
- **Facilidad de uso**: Te permite ejecutar tu entorno PHP con un solo comando (`docker-compose up`), sin tener que escribir largos comandos `docker run`.
- **Desarrollo en tiempo real**: Con los volúmenes, puedes modificar tus archivos en tu máquina local, y esos cambios se reflejan instantáneamente dentro del contenedor.
- **Portabilidad**: Este archivo puede ser compartido entre miembros de tu equipo o utilizado en distintos entornos sin problemas, asegurando que todos ejecuten la aplicación de la misma manera.

### Ayuditas 🛎️
- [Docker Compose Quickstart⚡📰](https://docs.docker.com/compose/gettingstarted/)
