
# FAQ


### ¿Por qué mi contenedor termina al finalizar la ejecición de mi código?

Un contenedor en Docker **siempre se detiene** cuando el proceso principal (normalmente un programa o script) que se ejecuta en su interior finaliza. En muchos casos, el proceso que se ejecuta en el contenedor PHP podría ser un script o una aplicación que finaliza inmediatamente después de imprimirse un mensaje.

Si quieres que el contenedor siga corriendo, necesitas:

1. **Ejecutar un proceso que no termine inmediatamente**, como por ejemplo un servidor web o un bucle infinito.
   
2. Modificar el script para que haga algo más prolongado o lo dejes en modo interactivo.


#### `php-1 exited with code 0` 

<img width="609" alt="image" src="https://github.com/user-attachments/assets/48b4c273-cad4-4f29-83df-37b5a6967540">


El mensaje `php-1 exited with code 0` significa que el contenedor llamado `php-1` (tu servicio `php`) ha terminado su ejecución con éxito.

En Docker, los contenedores devuelven un **código de salida** cuando terminan. Este código indica el estado final del proceso principal que se ejecutó dentro del contenedor:

- **Código `0`**: Indica que el contenedor terminó **correctamente** y sin errores.
- **Códigos distintos de `0`**: Indican algún tipo de **error** o finalización anormal del proceso principal.


### ¿Cómo evitar que se detenga el contenedor?

Si estás rulando PHP y quieres que el contenedor permanezca activo, una opción es hacer que el contenedor se ejecute en modo "demonio" (usando algo como `php -S` si estás ejecutando un servidor embebido).

Aquí te doy un ejemplo de cómo modificar el comando que el contenedor ejecuta para evitar que finalice inmediatamente:

```yaml
services:
  php:
    image: php:7.4-cli
    command: php -S 0.0.0.0:8000 # Esto levantará un servidor web y mantendrá el contenedor en ejecución
    ports:
      - "8000:8000"
```

Esto hará que el contenedor se quede en ejecución mientras el servidor PHP esté funcionando.
