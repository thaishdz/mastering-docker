

# ¿Estás en entorno Apple? ✋, entonces lee esto 👇👇👇

Las imágenes de Docker construidas con Apple Silicon (u otra arquitectura basada en ARM64) pueden generar problemas al desplegar dichas imágenes en un entorno basado en *AMD64, 
como Linux o Windows (por ejemplo, AWS EC2, ECS, etc.). 

Por ejemplo, podrías intentar subir tu imagen de Docker creada en un chip M1 a un repositorio de AWS ECR y que no funcione. 
Por lo tanto, necesitas una forma de construir imágenes basadas en `AMD64` en la arquitectura `ARM64`, ya sea:

- Utilizando `Docker build` (para imágenes individuales) 
- O `docker compose build` (para aplicaciones con múltiples imágenes que se ejecutan en una red de `docker-compose`).


### Ayuditas 🛎️
- [Stackoverflow - Forcing docker to use linux/amd64 platform by default on macOS](https://stackoverflow.com/questions/65612411/forcing-docker-to-use-linux-amd64-platform-by-default-on-macos/69636473#69636473)
