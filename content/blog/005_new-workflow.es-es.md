---
title: Transformando la estructura de la web
slug: transformando-la-structura-de-la-web
tags: ["Hugo", "Github", "Github Actions", "codabal.ai", "Github Pages"]
date: 2023-05-15T18:00:00+0200
---

**2023/08/05 Actualización**: Codeball.ai ya no está disponible. He eliminado las acciones relacionadas con él, pero el post permanecerá tal cual para fines de archivo.

---

¡Hola a todos!

El otro día me entretuve en revisar información sobre Hugo, Github Actions, etc y decidí cambiar la forma en la que funciona la web porque me parecía que era lo correcto.

## El antes

Cuando publiqué por primera vez la web, todo estaba contenido en un solo repositorio.

En este repositorio tenia todo lo relativo a Hugo, y el flujo de trabajo era algo así:
1. Arrancaba el servidor de Hugo en mi local, ejecutando `hugo serve`.
2. Accedía a la url de mi local y veía el servidor.
3. Creaba una rama y hacía los cambios necesarios.
4. Revisaba los cambios en mi local. Repetir pasos 3 y 4 tantas veces como sea necesario.
5. Creaba un *commit* con los cambios y los pusheaba a origin en Github.
6. Creaba una *Pull Request*.
7. Se ejecuta una Github Action de [*codeball.ai*](https://codeball.ai) que analiza el código y te da un factor de confianza.
8. Se mergea la {{< abbrebiation abbrebiation="PR" description="Pull Request" >}} a `main`.
9. Con cada commit nuevo en main, se ejecuta otra Github Action que hace el build de los archivos estáticos de Hugo. Básicamente ejecuta `hugo` que genera la carpeta de `public`. Una vez generados los archivos, se publican en Github Pages con otra Action.
 
Lo cierto es que este flujo funciona bien pero tiene varias desventajas. No puedes tener el repositorio en privado si quieres usar Github Pages, tienes en un mismo repositorio mucho *codebase* y archivos generados, etc.

Con el nuevo flujo, podrías tener el repositorio con los archivos estáticos público, y el original, que hace el *build* de la web, privado (aunque este no sea mi caso).

## Nuevo flujo de trabajo

En este nuevo flujo vamos a tener dos depositorios, uno que es como el antiguo, donde está todo el codebase de Hugo, y otro donde simplemente estarán los estáticos de la web.

Y realmente, una vez implementados los cambios, tampoco cambia mucho el flujo. En el último paso del antiguo flujo es dónde hacemos cambios: ahora generamos la carpeta `public` en el repositorio original, pero no la publicamos. Ahora está se sube al repositorio nuevo con una Github Action. Una vez se pushea esta información al nuevo repositorio, se ejecuta la publicación del sitio web estático.

Este gráfico puede explicar un poquito el flujo, aunque estoy seguro que está mal definido porque nunca llegue a aprender diagramas secuenciales 😅: {{< svg "static/images/blog/005/diagram.svg">}}

Esto es útil si por ejemplo queremos ocultar el reposition original. En mi caso, estoy usando *codeball.ai* y solo es gratis para los repositorios públicos, así que de momento, voy a mantener ambos repos públicos, pero quién sabe.

---

Eso es todo, ¡muchas gracias por leerme!
