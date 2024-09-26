
# Dockerfile

> Contiene **instrucciones** que Docker sigue para construir una **imagen** de contenedor. 

La imagen es básicamente un paquete que incluye todo lo que tu aplicación necesita para ejecutarse: 
- sistema operativo
- bibliotecas
- dependencias
- tu código de mierda 💩

### ¿Por qué usar un `Dockerfile`?

1. **Reproducibilidad**:
   - Con un `Dockerfile`, puedes **garantizar que tu aplicación se ejecute de la misma manera en cualquier entorno**. No importa si estás en tu PC local, en un servidor o en una máquina de otro equipo, el contenedor será idéntico en todos los casos.

   - En lugar de preocuparte por "¿Funciona en mi máquina?", tendrás un entorno controlado donde siempre funciona.

3. **Independencia del entorno**:
   - No importa qué sistema operativo uses (Windows, macOS, Linux), el contenedor será siempre el mismo. Esto elimina los problemas de compatibilidad entre desarrolladores o entre el entorno de desarrollo y producción.

     - P.e: Si tu código PHP necesita una versión específica como la 8.1, puedes asegurarte de que siempre tendrás esa versión en el contenedor, independientemente de lo que esté instalado en la máquina host.

4. **Aislamiento de dependencias**:
   - Un `Dockerfile` te permite instalar todas las dependencias de tu aplicación **aisladas del sistema operativo host**. Esto significa que tu sistema no se ensucia con instalaciones o configuraciones específicas (por ejemplo, versiones de PHP o bibliotecas), ya que todo se mantiene dentro del contenedor.

      - P.e: Si necesitas diferentes versiones de PHP para distintos proyectos, puedes tener un contenedor con PHP 7.4 y otro con PHP 8.1, sin que entren en conflicto. 👍

5. **Automatización**:
   - Un `Dockerfile` permite **automatizar** el proceso de construcción y despliegue de tu aplicación. Si tienes instrucciones claras de cómo instalar dependencias, preparar archivos, ejecutar scripts, todo eso queda registrado en el `Dockerfile`.

      - P.e: Configuras en el `Dockerfile` cómo instalar las extensiones de PHP que necesita tu proyecto. Cada vez que se construye la imagen, Docker seguirá esos pasos.

6. **Portabilidad**:
   - Con un `Dockerfile`, puedes mover tu aplicación a cualquier entorno con Docker. Todo lo que necesita para ejecutarse (excepto los datos externos) está dentro del contenedor. Puedes construirlo una vez y correrlo en cualquier sistema que tenga Docker.

      - P.e: Si desarrollas localmente, puedes compartir tu `Dockerfile` con otros miembros del equipo para que tengan la misma configuración. También puedes usar el mismo contenedor en tu entorno de producción.

7. **Facilidad de escalabilidad**:
   - Cuando trabajas en entornos de producción (por ejemplo, en la nube), los contenedores son muy utilizados para **escalar aplicaciones rápidamente**.

   - Un `Dockerfile` bien configurado te permite desplegar múltiples contenedores idénticos de tu aplicación, asegurando una distribución uniforme de la carga.

8. **Estandarización**:
   - Usar un `Dockerfile` permite a los equipos de desarrollo y operaciones seguir el mismo proceso de configuración y despliegue. Con una estructura estándar, es más fácil colaborar y entender cómo se debe preparar el entorno de la aplicación.
   
      - P.e: Si más personas se unen al proyecto, pueden levantar el entorno de desarrollo rápidamente sin tener que instalar dependencias manualmente en su sistema.

### ¿Qué pasa si no uso un `Dockerfile`?

Si no usas un `Dockerfile`, tendrías que hacer todo manualmente cada vez que necesites levantar tu entorno, por ejemplo:
- Instalar PHP y sus dependencias en tu máquina local.
- Asegurarte de que estás usando las versiones correctas de todo.
- Configurar rutas de archivos, variables de entorno, etc., cada vez que trabajas en una máquina nueva o con un equipo distinto.
- Además, cada miembro del equipo tendría que asegurarse de que todas las configuraciones son exactamente iguales, lo que es propenso a errores.

### Ejemplo de una situación:
Imagina que trabajas en un equipo en el que algunos usan **Windows** y otros usan **Linux**. Sin un `Dockerfile`, cada uno tendría que configurar manualmente PHP y las dependencias necesarias en su sistema operativo, lo que puede generar inconsistencias (problemas con versiones, configuraciones, o dependencias). 

Con un `Dockerfile`, todos simplemente construyen la imagen Docker y saben que el entorno de trabajo es **exactamente el mismo** para todos.

### Resumen:

Usar un `Dockerfile` te proporciona:
- **Consistencia y control** sobre el entorno de ejecución de tu aplicación.
- **Aislamiento** de dependencias y configuraciones.
- **Automatización** del proceso de instalación y configuración.
- **Portabilidad** para que tu aplicación se ejecute en cualquier lugar de la misma manera.
- **Escalabilidad** para desplegar y ejecutar aplicaciones en múltiples entornos de manera sencilla.

En resumen, un `Dockerfile` te permite construir **imágenes reproducibles y aisladas**, lo que garantiza que tu aplicación funcione de manera confiable en cualquier entorno.
