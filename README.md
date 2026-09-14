# Hello Django — mi práctica con Django + Docker

Proyecto de práctica de backend, construido
sobre el template open-source [`docker-django-example` de Nick Janetakis](https://github.com/nickjj/docker-django-example).
No es una app mía desde cero: es un ejercicio para aprender a levantar
y entender un proyecto Django profesional corriendo en Docker.

## Qué hice acá

- Cloné el template y levanté el stack con Docker Compose: `web` (Django + gunicorn),
  `postgres`, `redis`, `worker` (Celery) y los watchers de assets `js`/`css`.
- Preparé el entorno copiando `.env.example` a `.env`.
- Corrí los comandos de Django dentro del contenedor con el script del template
  (`./run manage migrate`) y anoté la diferencia con un proyecto Django estándar,
  donde se usa `python manage.py migrate`.
- Estudié cómo está armado: la diferencia entre proyecto y aplicación, las
  migraciones y qué archivo hace qué en cada uno.
- Dejé mis apuntes en el notebook `Copia de Fundamentos a django.ipynb`: SSH,
  comandos básicos de Linux, contenedores, la estructura de Django y por qué hay
  que apagar los servicios con `docker compose down` para no gastar recursos.

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

## Ejemplo de uso

Con el stack corriendo:

1. Abre <http://127.0.0.1:8000>: aparece la página de inicio del template, la misma de la captura de abajo.
2. Crea un usuario administrador con `./run manage createsuperuser`.
3. Entra con ese usuario a <http://127.0.0.1:8000/admin> y revisa el panel de Django.
4. Cuando termines, apaga todo con `docker compose down` para no dejar contenedores gastando recursos.

## Capturas

Pantalla de inicio del proyecto al levantarlo (la captura es la del template original de Nick Janetakis):

![Pantalla de inicio del template docker-django-example](.github/docs/screenshot.jpg)

## Créditos y licencia

- Template original: **[docker-django-example](https://github.com/nickjj/docker-django-example)**
  de **Nick Janetakis** ([@nickjj](https://github.com/nickjj)). Todo el
  crédito de la arquitectura Docker es suyo; mi trabajo fue levantarlo,
  entenderlo y documentarlo como práctica.
- El proyecto se distribuye bajo la **licencia MIT** (ver [`LICENSE`](LICENSE)),
  heredada del template original.

## Contacto

**Felipe** — GitHub: [@pipeTawns-x](https://github.com/pipeTawns-x)
