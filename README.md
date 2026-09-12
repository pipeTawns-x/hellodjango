# Hello Django — mi práctica con Django + Docker

Proyecto de práctica del curso de **Programador Front-end de EBAC**, construido
sobre el template open-source [`docker-django-example` de Nick Janetakis](https://github.com/nickjj/docker-django-example).
No es una app mía desde cero: es un ejercicio honesto para aprender a levantar
y entender un proyecto Django profesional corriendo en Docker.

## Qué hice acá

- **Levanté el stack completo con Docker Compose**: `web` (Django + gunicorn),
  `postgres`, `redis`, `worker` (Celery) y los watchers de assets `js`/`css`.
  Entendí qué hace cada servicio y por qué el proyecto no es "solo Django".
- **Renombré el proyecto con el script del template**: ejecuté
  `bin/rename-project hellodjango` y verifiqué el cambio en
  `COMPOSE_PROJECT_NAME=hellodjango` (ver `.env.example`) y en los archivos de
  `src/config`.
- **Configuré las variables de entorno**: copié `.env.example` a `.env` y
  revisé cada bloque — perfiles de Compose, `SECRET_KEY` de desarrollo, puerto
  y flags de Python — en vez de copiar sin leer.
- **Corrí los comandos de gestión dentro del contenedor** con el script `run`:
  `./run manage migrate` para crear las tablas y `./run manage createsuperuser`
  para entrar al admin de Django en `http://127.0.0.1:8000/admin`.
- **Estudié la estructura del proyecto**: `src/config` (settings y URLs),
  `src/pages` (app de ejemplo), los templates y el entrypoint
  `bin/docker-entrypoint-web`. Dejé mis apuntes del proceso en el notebook
  `Copia de Fundamentos a django.ipynb`.
- **Problemas encontrados y cómo los resolví**: al inicio intenté ejecutar
  `python manage.py migrate` directo en mi máquina y falló, porque las
  dependencias viven dentro de la imagen, no en mi sistema — la solución era
  usar siempre `./run manage ...`. También me costó entender por qué la app no
  respondía hasta que vi que el servicio `web` espera a que `postgres` pase su
  healthcheck antes de arrancar.

## Tecnologías

- **Django 5.2** / **Python 3.13**
- **Docker** y **Docker Compose** (multi-servicio con perfiles)
- **PostgreSQL** (base de datos) y **Redis** (broker de Celery / caché)
- **Celery** (tareas en segundo plano) y **gunicorn** (servidor WSGI)
- **esbuild + watchers** para los assets estáticos (JS/CSS)

## Instalación y uso

Requisito: tener Docker y Docker Compose instalados.

```bash
# 1. Clonar el repositorio
git clone https://github.com/pipeTawns-x/hellodjango.git
cd hellodjango

# 2. Crear el archivo de entorno
cp .env.example .env

# 3. Construir las imágenes y levantar todos los servicios
docker compose up --build
```

En otra terminal, con el stack corriendo:

```bash
# Crear las tablas en PostgreSQL
./run manage migrate

# Crear un usuario administrador
./run manage createsuperuser
```

La app queda disponible en <http://127.0.0.1:8000> y el admin de Django en
<http://127.0.0.1:8000/admin>.

Otros comandos útiles del template:

```bash
./run manage shell      # shell interactivo de Django dentro del contenedor
./run psql              # cliente psql contra la base del contenedor
./run lint              # linters (Dockerfile y scripts shell)
./run quality           # chequeos de formato y calidad
docker compose down     # detener todos los servicios
```

## Capturas

El template levantado y respondiendo en local:

![Screenshot de la app corriendo](.github/docs/screenshot.jpg)

## Créditos y licencia

- Template original: **[docker-django-example](https://github.com/nickjj/docker-django-example)**
  de **Nick Janetakis** ([@nickjj](https://github.com/nickjj)). Todo el
  crédito de la arquitectura Docker es suyo; mi trabajo fue levantarlo,
  personalizarlo, entenderlo y documentarlo como práctica de EBAC.
- El proyecto se distribuye bajo la **licencia MIT** (ver [`LICENSE`](LICENSE)),
  heredada del template original.

## Contacto

**Felipe** — GitHub: [@pipeTawns-x](https://github.com/pipeTawns-x)
