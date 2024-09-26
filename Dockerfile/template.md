
# PHP solo CLI

```yaml
# Usar la imagen oficial de PHP
FROM php:8.3-cli

# Configurar el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar todos los archivos del directorio actual al contenedor
COPY . /app

# Comando por defecto para ejecutar el contenedor (puedes cambiarlo si lo deseas)
CMD [ "php", "./index.php" ]
```
