
# Installation Guide 🚀


Instalación de Docker hasta la ejecución de un contenedor PHP.


- [Instalar Docker](https://github.com/thaishdz/mastering-docker/blob/main/guide.md#paso-1-instalar-docker-)
- [Crear el entorno de desarrollo](https://github.com/thaishdz/mastering-docker/blob/main/guide.md#paso-2-crear-el-entorno-de-desarrollo)
- [Construir la imagen](https://github.com/thaishdz/mastering-docker/blob/main/guide.md#paso-3-construir-la-imagen)
- [Ejecutar el contenedor](https://github.com/thaishdz/mastering-docker/blob/main/guide.md#paso-4-ejecutar-el-contenedor)

# Paso 1: Instalar Docker 🐳

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

### Verificar la instalación de Docker

Abre una terminal o línea de comandos y ejecuta el siguiente comando para asegurarte de que Docker está instalado correctamente:

```bash
docker --version
```

Si ves algo como `Docker version 20.10.x`, entonces Docker está instalado correctamente.

# Paso 2: Crear el entorno de desarrollo

Ahora que Docker está instalado, vamos a crear un contenedor para ejecutar PHP (por ejemplo). Sigue estos pasos:

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
echo "¡Hola, Docker! 👋\n";
```

#### Crear un archivo `docker-compose.yml`

Si quieres gestionar tu contenedor más easy, creálo. Esto te permite arrancar el contenedor con un solo comando y mantener un entorno más ordenado.

Aparte de sincronizar los cambios que hagas en el código.

Crea el archivo `docker-compose.yml` en el mismo directorio con el siguiente contenido:

```yaml
services:
  php:
    build: .
    volumes:
      - .:/app
    working_dir: /app
    command: php ./index.php
```

Este archivo define un servicio llamado `php`, que usa el `Dockerfile` para construir la imagen y monta el directorio actual dentro del contenedor.

# Paso 3: Crear la imagen 

Ahora que tienes todo configurado, es hora de construir la imagen Docker a partir del `Dockerfile`. Ejecuta el siguiente comando en la terminal (desde el directorio donde está tu `Dockerfile`):

```bash
docker build -t php-docker-app .
```

- **`-t php-docker-app`**: Asigna un nombre a la imagen que estás construyendo (en este caso, `php-docker-app`).
- **`.`**: Indica que Docker debe buscar el `Dockerfile` en el directorio actual.

Este comando descargará la imagen base de PHP y construirá tu imagen personalizada.

# Paso 4: Ejecutar el contenedor

Después de construir la imagen, puedes ejecutar el contenedor con el siguiente comando:

```bash
docker run --rm php-docker-app
```

- **`--rm`**: Borra automáticamente el contenedor una vez que se detenga, para no dejar contenedores innecesarios en tu máquina.
- **`php-docker-app`**: El nombre de la imagen que creaste.

Este comando ejecutará el script `index.php` dentro del contenedor y deberías ver la salida:

```
¡Hola Docker! 👋
```

### Ejecutar con Docker Compose

Puedes levantar el contenedor así:

```bash
docker compose up
```

Este comando construirá y ejecutará el contenedor de acuerdo con las especificaciones del archivo `docker-compose.yml`.

<img width="626" alt="Captura de pantalla 2024-09-26 a las 19 06 05" src="https://github.com/user-attachments/assets/38468b1d-8fca-470c-918a-123dfcf64202">

---

# Nombrar un contenedor

<img width="626" alt="Captura de pantalla 2024-09-26 a las 19 06 05" src="https://github.com/user-attachments/assets/38468b1d-8fca-470c-918a-123dfcf64202">

## ¿Qué es ese `php-php-1`?

`<nombre_del_servicio>-<nombre_del_proyecto>-<número_de_instancia>`

1. **`php`** es el nombre del servicio, que viene de la definición del servicio en el archivo `docker-compose.yml` (por ejemplo, `php:` en el archivo).
2. **`php`** también es el nombre del proyecto. Si no especificas un nombre de proyecto en tu archivo `docker-compose.yml`, Docker Compose usa el nombre de la carpeta en la que se encuentra el archivo como nombre de proyecto. Probablemente, tu directorio se llama `php`.
3. **`1`** es el número de la instancia. Docker Compose asigna un número incremental a cada contenedor, comenzando desde 1. Esto es útil cuando levantas múltiples réplicas del mismo servicio.

### Solución
Si deseas evitar el nombre repetido o controlarlo más explícitamente, puedes hacer una de estas cosas:

1. **Cambiar el nombre del proyecto** usando la opción `-p`:
   ```bash
   docker-compose -p mi_proyecto up
   ```
   Esto cambiará el nombre del proyecto y el contenedor será nombrado de acuerdo con ese nombre de proyecto.

2. **Especificar un nombre de contenedor fijo** en el archivo `docker-compose.yml`:
   ```yaml
   services:
     php:
       container_name: mi_contenedor_php
       ...
   ```
   Esto fijará el nombre del contenedor a `mi_contenedor_php` en lugar de generar nombres automáticamente.

### Resumen de la guía:
1. **Instalación de Docker**: Instala Docker Desktop en Windows/macOS/Linux.
2. **Verificación de Docker**: Confirma que Docker esté correctamente instalado usando `docker --version`.
3. **Crear un proyecto PHP**: Configura un directorio con un `Dockerfile` y un script PHP simple.
4. **Construir la imagen Docker**: Usa `docker build` para crear una imagen de tu aplicación PHP.
5. **Ejecutar el contenedor**: Corre el contenedor y ejecuta el script PHP con `docker run`.
6. **Docker compose**: Usa `docker-compose` para gestionar el contenedor de manera más sencilla y sincronizar los cambios.
