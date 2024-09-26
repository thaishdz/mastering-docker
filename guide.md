
# Installation Guide 🚀


Instalación de Docker hasta la ejecución de un contenedor PHP.

### Paso 1: Instalar Docker 🐳

#### **En Windows**:
1. Descarga **Docker Desktop** desde [el sitio oficial de Docker](https://www.docker.com/products/docker-desktop).
2. Ejecuta el instalador y sigue las instrucciones.
3. Asegúrate de habilitar **WSL 2** (subsistema de Windows para Linux) si te lo pide durante la instalación.
4. Una vez instalado, inicia Docker Desktop. Se ejecutará en segundo plano, y verás un icono en la bandeja del sistema.

#### **En macOS**:
1. Descarga **Docker Desktop** desde [el sitio oficial de Docker](https://www.docker.com/products/docker-desktop).
2. Ejecuta el archivo `.dmg` y arrastra el icono de Docker a la carpeta de aplicaciones.
3. Abre Docker desde las aplicaciones. Puede pedirte permisos administrativos.
4. Docker se ejecutará en segundo plano, y aparecerá un icono en la barra de menú superior.

#### **En Linux**:
1. Abre una terminal y ejecuta los siguientes comandos para instalar Docker en tu distribución Linux (en este ejemplo es para **Ubuntu**):
   ```bash
   sudo apt update
   sudo apt install docker.io -y
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
2. Agrega tu usuario al grupo `docker` (esto es opcional pero te permitirá ejecutar Docker sin `sudo`):
   ```bash
   sudo usermod -aG docker $USER
   ```
3. Reinicia tu sesión para que los cambios surtan efecto.

Para otras distribuciones Linux, puedes encontrar las instrucciones específicas [en la documentación oficial de Docker](https://docs.docker.com/engine/install/).

### Paso 2: Verificar la instalación de Docker

Abre una terminal o línea de comandos y ejecuta el siguiente comando para asegurarte de que Docker está instalado correctamente:

```bash
docker --version
```

Si ves algo como `Docker version 20.10.x`, entonces Docker está instalado correctamente.

### Paso 3: Crear el entorno PHP (en este caso) con Docker 🐳

Ahora que Docker está instalado, vamos a crear un contenedor para ejecutar PHP. Sigue estos pasos:

#### Crear un directorio para tu proyecto

Primero, crea un directorio en tu máquina donde colocarás los archivos de tu proyecto.

```bash
mkdir php-docker-app
cd php-docker-app
```

#### Crear un `Dockerfile`

El `Dockerfile` define cómo Docker debe configurar el contenedor para ejecutar PHP. Crea un archivo llamado `Dockerfile` con el siguiente contenido:

```Dockerfile
# Usar la imagen oficial de PHP en modo CLI
FROM php:8.1-cli

# Configurar el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar todos los archivos del directorio actual al contenedor
COPY . /app

# Comando por defecto para ejecutar el contenedor
CMD ["php", "./index.php"]
```

#### Crear un archivo PHP

Ahora vamos a crear un simple script PHP que será ejecutado dentro del contenedor. Crea un archivo llamado `index.php` dentro del directorio del proyecto con el siguiente contenido:

```php
<?php
echo "¡Hola, mundo desde Docker!\n";
```

#### Crear un archivo `docker-compose.yml`

Si quieres gestionar tu contenedor más easy, creálo. Esto te permite arrancar el contenedor con un solo comando y mantener un entorno más ordenado.

Aparte de sincronizar los cambios que hagas en el código.

Crea el archivo `docker-compose.yml` en el mismo directorio con el siguiente contenido:

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

Este archivo define un servicio llamado `php`, que usa el `Dockerfile` para construir la imagen y monta el directorio actual dentro del contenedor.

### Paso 4: Construir la imagen Docker

Ahora que tienes todo configurado, es hora de construir la imagen Docker a partir del `Dockerfile`. Ejecuta el siguiente comando en la terminal (desde el directorio donde está tu `Dockerfile`):

```bash
docker build -t php-docker-app .
```

- **`-t php-docker-app`**: Asigna un nombre a la imagen que estás construyendo (en este caso, `php-docker-app`).
- **`.`**: Indica que Docker debe buscar el `Dockerfile` en el directorio actual.

Este comando descargará la imagen base de PHP y construirá tu imagen personalizada.

### Paso 5: Ejecutar el contenedor

Después de construir la imagen, puedes ejecutar el contenedor con el siguiente comando:

```bash
docker run --rm php-docker-app
```

- **`--rm`**: Borra automáticamente el contenedor una vez que se detenga, para no dejar contenedores innecesarios en tu máquina.
- **`php-docker-app`**: El nombre de la imagen que creaste.

Este comando ejecutará el script `index.php` dentro del contenedor y deberías ver la salida:

```
¡Hola, mundo desde Docker!
```

### Paso 6: Ejecutar con Docker Compose

Puedes levantar el contenedor así:

```bash
docker-compose up
```

Este comando construirá y ejecutará el contenedor de acuerdo con las especificaciones del archivo `docker-compose.yml`.

### Paso 7: Verificar contenedores en ejecución

Si deseas ver todos los contenedores en ejecución, puedes usar el siguiente comando:

```bash
docker ps
```

Para ver los contenedores que han sido ejecutados anteriormente (incluidos los detenidos), usa:

```bash
docker ps -a
```

### Paso 8: Detener y limpiar contenedores (Opcional)

Para detener un contenedor que está corriendo, puedes usar el siguiente comando:

```bash
docker stop <container_id>
```

Si ejecutas `docker ps` para ver los contenedores en ejecución, obtendrás un **ID de contenedor** que puedes usar para detenerlo.

Para eliminar contenedores o imágenes innecesarias y liberar espacio, puedes ejecutar:

```bash
docker system prune
```

### Resumen de la guía:
1. **Instalación de Docker**: Instala Docker Desktop en Windows/macOS/Linux.
2. **Verificación de Docker**: Confirma que Docker esté correctamente instalado usando `docker --version`.
3. **Crear un proyecto PHP**: Configura un directorio con un `Dockerfile` y un script PHP simple.
4. **Construir la imagen Docker**: Usa `docker build` para crear una imagen de tu aplicación PHP.
5. **Ejecutar el contenedor**: Corre el contenedor y ejecuta el script PHP con `docker run`.
6. **Docker compose**: Usa `docker-compose` para gestionar el contenedor de manera más sencilla y sincronizar los cambios.
