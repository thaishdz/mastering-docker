
# Verificar contenedores en ejecución ☑️

```bash
docker ps
```

Para ver los contenedores que han sido ejecutados anteriormente (incluidos los detenidos), usa:

```bash
docker ps -a
```

# Detener y limpiar contenedores ♻️

Para detener un contenedor que está corriendo, puedes usar el siguiente comando:

```bash
docker stop <container_id>
```

Si ejecutas `docker ps` para ver los contenedores en ejecución, obtendrás un **ID de contenedor** que puedes usar para detenerlo.

Para __eliminar__ contenedores o imágenes innecesarias y liberar espacio, puedes ejecutar:

```bash
docker system prune
```
