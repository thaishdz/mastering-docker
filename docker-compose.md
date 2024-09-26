
# docker-compose

<p align="center">
  <img src="https://github.com/user-attachments/assets/8113ae18-24fc-4d6f-be9d-2a3ca0ecc22f" height="300" />
</p>

Se usa para que cualquier cambio que hagas en el código desde tu máquina local (es el *host*) se refleje automáticamente __DENTRO__ del contenedor **sin necesidad de reconstruir la imagen** o reiniciar el contenedor. 

> ☝️ Esto es útil cuando estás desarrollando, ya que puedes probar tus cambios de inmediato sin pasos adicionales.

Esto lo logramos con la instrucción `volumes` en el archivo `docker-compose.yml`:

```yaml
volumes:
  - .:/app
```

### ¿Qué significa esta línea?
1. **"."**: La parte a la izquierda de los dos puntos (`:`) representa el directorio actual en tu máquina local (el *host*), es decir, donde está tu código fuente.
2. **"/app"**: La parte a la derecha de los dos puntos es el directorio dentro del contenedor donde queremos montar ese código.

### ¿Qué hace Docker con esto?
Cuando defines un volumen de esta manera:
- Docker monta el directorio local (`.`) en el contenedor, en el directorio `/app`. 
- Si haces cualquier cambio en el código en tu máquina local (el directorio que corresponde a `.`), esos cambios se reflejan automáticamente en el contenedor, en la carpeta `/app`.
- No necesitas volver a construir la imagen o reiniciar el contenedor, ya que siempre estará trabajando con los archivos más actualizados de tu máquina.

### Ejemplo de flujo de trabajo:

1. Inicias el contenedor con `docker-compose up`.
2. Editas tu archivo `index.php` en tu máquina local, añadiendo una nueva línea como:
    ```php
    echo "Nueva línea de código\n";
    ```
3. No necesitas detener el contenedor ni volver a construir la imagen. Simplemente recargas o ejecutas el contenedor, y el código actualizado estará disponible de inmediato dentro del contenedor.

En resumen, con esta configuración, tu código en el contenedor estará **"sincronizado" en tiempo real** con el de tu máquina local, haciendo mucho más ágil el proceso de desarrollo.

### ¿Por qué es útil?

Cuando desarrollas, no quieres estar constantemente reconstruyendo la imagen o volviendo a crear el contenedor cada vez que cambias una línea de código. El uso de volúmenes te permite trabajar de manera fluida y rápida, como si estuvieras ejecutando PHP directamente en tu máquina, pero dentro de un contenedor.
