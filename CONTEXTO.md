# Contexto del proyecto - Sitio Web Tritones Buceo

## Qué es este proyecto
Sitio web del club de buceo **Tritones** (`www.tritonesbuceo.com`), publicado en Blogger.
El código vive en `/mnt/raid/proyectos_web/site1` en server01, accesible desde el ThinkPad de Manuel vía CIFS en `/mnt/raid/site1`.

## Dos archivos principales
- `index.html` — página completa standalone (para previsualización local con imágenes locales)
- `blogger_embed.html` — el código que se pega en Blogger como **tema completo** (reemplaza el tema por defecto). Usa URLs de Google Drive para las imágenes. **Importante:** en este archivo todos los `&` en URLs deben escribirse como `&amp;` porque Blogger lo parsea como XML — de lo contrario lanza `SAXParseException`.

## Estructura actual del landing (desde 2026-03-30)
1. **Topbar** — logo, "Medellín, Colombia", botón WhatsApp
2. **Hero** — H1 "Tritones Buceo", descripción del club, botones WhatsApp e Instagram, imagen de actividades
3. **Cursos** — 3 cards: Vida marina/naturalista, Nitrox y especialidades, Natación a la medida
4. **Excursiones** — 2 cards: Nacionales (→ /p/excursiones-nacionales.html) e Internacionales (→ /p/excursiones-internacionales.html)
5. **Footer** — logo, WhatsApp, Instagram, dirección sede

## Páginas de subpáginas (en el tema, vía b:elseif)
El tema detecta la URL con `data:blog.url contains` y renderiza el contenido directamente (no usa Blog widget):
- `excursiones-nacionales` → página completa con 6 destinos nacionales + imagen Drive `1wxLfYYR238G__w_edhypex3VMWvnW-up`
- `excursiones-internacionales` → página completa con 6 destinos internacionales + imagen Galápagos Drive `1hoOMUGCZMxkehtL-g36B6aY_v_UM1cLM` (Diego Delso, CC BY-SA 4.0), crédito overlaid en imagen con div absoluto dentro de wrapper relativo

## Sección Noche de Buzos
Removida del landing. Las entradas del blog sobre noches de buzos pasadas son contenido de archivo a reconstruir en el futuro como posts de Blogger. No va en el landing hasta nuevo aviso.

## Imágenes en blogger_embed.html (Google Drive)
- Logo: `https://drive.google.com/thumbnail?id=1z4FYH5yw9OLvQkTKnVp29bFh1IPXckvP&amp;sz=w1000`
- Hero: `https://drive.google.com/thumbnail?id=14sMjW64UhIXkeN0kZriUOe8xC2CqgGM0&amp;sz=w1600`

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
- `version_4_2026-03-30` — estructura Hero→Cursos→Excursiones, quita Noche de Buzos, corrige &amp; en URLs de Drive
- `version_5_2026-03-30` — agrega páginas Excursiones Nacionales e Internacionales (b:elseif en tema), imagen Galápagos con crédito CC BY-SA 4.0 overlaid
- La carpeta `backup_de_blogger/` tiene el XML del tema original de Blogger

## Google Search Console (2026-03-30)
- 23 URLs de posts válidas e indexadas (posts del blog 2013–2020)
- 1 URL con error 404: `.../2019/05/tribuga-especies-y-comunidades-en.html?view=snapshot` — parámetro de Blogger raro, menor, Google lo descartará solo.

## Notas importantes
- `noche_de_buzos_posters/Instagram_files/` está en `.gitignore` (archivos pesados de Instagram, no son código)
- Las versiones en carpetas son snapshots manuales; el historial real está en git
- SSH key del ThinkPad ya está agregada a GitHub (cuenta: manolosuarezv)

## Acceso al RAID desde el ThinkPad
- **En casa**: se monta automáticamente vía CIFS desde `//192.168.1.69/raid/proyectos_web` al arrancar
- **Fuera de casa**: usar Tailscale IP `100.119.118.124` — el montaje automático falla porque el fstab tiene la IP local
  - Montaje manual fuera de casa:
    ```bash
    sudo mount -t cifs //100.119.118.124/raid/proyectos_web /mnt/raid -o credentials=/etc/credentials_server01,uid=1000,gid=1000,iocharset=utf8
    ```
- **Pendiente**: crear script que detecte si está en casa o fuera y monte con la IP correcta automáticamente
