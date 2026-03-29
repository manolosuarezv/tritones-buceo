# Contexto del proyecto - Sitio Web Tritones Buceo

## Qué es este proyecto
Sitio web del club de buceo **Tritones** (`www.tritonesbuceo.com`), publicado en Blogger.
El código vive en `/mnt/raid/proyectos_web/site1` en server01, accesible desde el ThinkPad de Manuel vía CIFS en `/mnt/raid/site1`.

## Dos archivos principales
- `index.html` — página completa standalone (para previsualización y referencia)
- `blogger_embed.html` — el código que se pega en Blogger como **tema completo** (reemplaza el tema por defecto). Este es el que se publica en `www.tritonesbuceo.com`.

## Estado actual del sitio (2026-03-29)
El sitio tiene tres secciones construidas hasta ahora:
1. **Hero** — sección principal de bienvenida con imagen de fondo y CTA
2. **Cursos** (`div.cursos` o similar) — sección de cursos de buceo
3. **Footer** — pie de página

Pendiente: seguir construyendo las demás secciones del sitio.

## Infraestructura
- **Servidor nginx** corriendo en `http://192.168.1.69:8090` sirviendo `/mnt/raid/proyectos_web/site1/`
  - Config en `/etc/nginx/sites-available/tritones` en server01
  - Arrancar/recargar: `sudo systemctl reload nginx`
- **Repositorio GitHub**: `git@github.com:manolosuarezv/tritones-buceo.git` (SSH configurado)
  - Push directo sin token ni contraseña desde el ThinkPad de Manuel

## Versiones guardadas
- `version_1_2026-03-26` — primera versión
- `version_2_2026-03-27` — agrega sección cursos
- `version_3_2026-03-27` — versión final de ese día
- La carpeta `backup_de  _blogger/` tiene el XML del tema original de Blogger

## Notas importantes
- `noche_de_buzos_posters/Instagram_files/` está en `.gitignore` (archivos pesados de Instagram, no son código)
- Las versiones en carpetas son snapshots manuales; el historial real está en git
- SSH key del ThinkPad ya está agregada a GitHub (cuenta: manolosuarezv)
