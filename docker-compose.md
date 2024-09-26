
# docker-compose

<p align="center">
  <img src="https://github.com/user-attachments/assets/8113ae18-24fc-4d6f-be9d-2a3ca0ecc22f" height="300" />
</p>

Docker Compose es una herramienta que te **permite definir y ejecutar múltiples contenedores** Docker en un solo archivo de configuración.

```yaml
version: '3'
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
version: '3'
```
Esta línea define la **versión del formato de archivo de Docker Compose** que estás utilizando.
- **Versión 3**: De las versiones más usadas (a 2024), especialmente para entornos de producción y despliegues.
---
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

#### ¿Qué significa esto en la práctica?
- **Sincronización de archivos**: Cualquier cambio que hagas en los archivos dentro de tu directorio local (host) se reflejará automáticamente dentro del contenedor. Esto es útil durante el desarrollo, ya que puedes modificar tu código sin tener que reconstruir o reiniciar el contenedor cada vez.
  
  En este ejemplo, el directorio del host será accesible dentro del contenedor bajo la ruta `/app`.

### Ejemplo de flujo de trabajo:

1. Inicias el contenedor con `docker-compose up`.
2. Editas tu archivo `index.php` en tu máquina local, añadiendo una nueva línea como:
    ```php
    echo "Nueva línea de código\n";
    ```
3. No necesitas detener el contenedor ni volver a construir la imagen. Simplemente recargas o ejecutas el contenedor, y el código actualizado estará disponible de inmediato dentro del contenedor.

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

### ¿Por qué es útil este archivo `docker-compose.yml`?
- **Facilidad de uso**: Te permite ejecutar tu entorno PHP con un solo comando (`docker-compose up`), sin tener que escribir largos comandos `docker run`.
- **Desarrollo en tiempo real**: Con los volúmenes, puedes modificar tus archivos en tu máquina local, y esos cambios se reflejan instantáneamente dentro del contenedor.
- **Portabilidad**: Este archivo puede ser compartido entre miembros de tu equipo o utilizado en distintos entornos sin problemas, asegurando que todos ejecuten la aplicación de la misma manera.



### ¿Qué significa esta línea?
1. `.`: La parte a la izquierda de los dos puntos (`:`) representa el directorio actual en tu máquina local (el *host*), es decir, donde está tu código fuente.
2. `/app`: La parte a la derecha de los dos puntos es el directorio dentro del contenedor donde queremos montar ese código.

### ¿Qué hace Docker con esto?
Cuando defines un volumen de esta manera:
- Docker monta el directorio local (`.`) en el contenedor, en el directorio `/app`. 
- Si haces cualquier cambio en el código en tu máquina local (el directorio que corresponde a `.`), esos cambios se reflejan automáticamente en el contenedor, en la carpeta `/app`.
- No necesitas volver a construir la imagen o reiniciar el contenedor, ya que siempre estará trabajando con los archivos más actualizados de tu máquina.


### ¿Por qué es útil?
Cuando desarrollas, no quieres estar constantemente reconstruyendo la imagen o volviendo a crear el contenedor cada vez que cambias una línea de código. El uso de volúmenes te permite trabajar de manera fluida y rápida, como si estuvieras ejecutando PHP directamente en tu máquina, pero dentro de un contenedor.
