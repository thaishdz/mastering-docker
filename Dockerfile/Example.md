> 👉 El `Dockerfile` definirá la imagen base de PHP y cómo se debe configurar el contenedor para ejecutar scripts PHP. 


Crea un archivo llamado `Dockerfile` en el directorio y agrega el siguiente contenido:

```yaml
# Usar la imagen oficial de PHP
FROM php:8.1-cli

# Configurar el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar todos los archivos del directorio actual donde está ubicado
# el proyecto (fuera del contenedor) al contenedor
COPY . /app

# Comando por defecto para ejecutar el contenedor (puedes cambiarlo si lo deseas)
CMD [ "php", "./index.php" ]
```




### `FROM php:8.1-cli`
```Dockerfile
FROM php:8.1-cli
```
Esta línea establece la **imagen base** que utilizará el contenedor. En este caso, estamos utilizando la imagen oficial de PHP en su versión 8.1, específicamente en la versión **CLI** (*Command Line Interface*).
  
- **¿Qué es una imagen base?**: 
  Una imagen base es el punto de partida de tu contenedor. Define el sistema operativo y las herramientas que estarán disponibles dentro del contenedor. Aquí, la imagen base trae una instalación mínima de PHP (modo CLI), para que puedas ejecutar scripts PHP desde la línea de comandos.
  
- **CLI**: 
  Como es la versión CLI de PHP, es ideal para ejecutar scripts, pero no tiene un servidor web como Apache o Nginx preinstalado. Si necesitaras servir PHP a través de un servidor web, usarías otra versión, como `php:8.1-apache` o instalarías manualmente un servidor en el contenedor.

### `WORKDIR /app`
```Dockerfile
WORKDIR /app
```
Esta instrucción establece el **directorio de trabajo** dentro del contenedor. Esto significa que cualquier comando ejecutado después de esta línea (por ejemplo, `COPY`, `CMD`) asumirá que la raíz del proyecto está en `/app`.

- **Beneficio**:
  No necesitas especificar rutas completas para los archivos o scripts dentro del contenedor; todos los comandos que vienen después se ejecutarán dentro de `/app`. Es equivalente a estar dentro de una carpeta en tu terminal en la máquina host.

- **Ejemplo**: 
  Si tienes un archivo `index.php` en el directorio `/app`, PHP lo encontrará sin necesidad de usar rutas largas.

### `COPY . /app`
```Dockerfile
COPY . /app
```
Esta línea copia **todos los archivos del directorio actual en tu máquina local** (el que contiene el `Dockerfile`) **dentro del contenedor** en la carpeta `/app`.

- **El punto (`.`)**:
  En Docker, el punto (`.`) representa el **directorio actual** desde donde estás construyendo la imagen. Esto incluye todo el contenido de esa carpeta en tu máquina local (archivos PHP, configuraciones, etc.).

- **`/app`**:
  Es el destino dentro del contenedor donde se colocarán esos archivos.

### `CMD [ "php", "./index.php" ]`
```Dockerfile
CMD [ "php", "./index.php" ]
```
Esta línea define el **comando por defecto** que se ejecutará cuando arranques el contenedor. En este caso, ejecuta `php ./index.php` dentro del contenedor.

- **CMD**: 
  Especifica el comando que ejecutará Docker cuando el contenedor esté en funcionamiento. En este caso:
  - `"php"` es el comando que queremos ejecutar (el intérprete de PHP).
  - `"./index.php"` es el script que le pasamos como argumento al comando `php`. El contenedor, al arrancar, buscará un archivo `index.php` en el directorio `/app` (definido por `WORKDIR`) y lo ejecutará.

### Flujo del `Dockerfile` completo:
1. **Se selecciona la imagen base**: Al usar `php:8.1-cli`, obtenemos una instalación funcional de PHP CLI en el contenedor.
2. **Se configura el directorio de trabajo** (`/app`) dentro del contenedor, lo que facilita la ejecución de comandos desde ese directorio.
3. **Se copian todos los archivos del directorio actual** en la máquina host al directorio `/app` dentro del contenedor.
4. **Se establece el comando por defecto**: El contenedor ejecutará el script `index.php` usando PHP al iniciarse.

### Resumen
Cuando construyes y corres el contenedor con este `Dockerfile`, Docker sigue estos pasos:
- Se construye un contenedor basado en PHP 8.1.
- Coloca tu código en la carpeta `/app` del contenedor.
- Cuando arrancas el contenedor, automáticamente ejecuta el comando `php ./index.php`, mostrando cualquier salida del script en tu terminal.
