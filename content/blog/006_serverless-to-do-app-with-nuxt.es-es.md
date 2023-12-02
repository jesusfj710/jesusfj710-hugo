---
title: Aplicación web de tareas sin servidor con Nuxt3
slug: aplicacion-web-tareas-sin-servidor-nuxt3
tags: ["nuxt", "to-do app", "serverless", "pinia", "localforate", "localstorage", "nuxtui", "bun"]
date: 2023-12-02T13:00:00+0200
---
- [Introducción](#introducción)
  - [Stack tecnológico](#stack-tecnológico)
  - [Creando el proyecto con Nuxt3](#creando-el-proyecto-con-nuxt3)
  - [Gestionando los datos](#gestionando-los-datos)
  - [Desplegando los cambios](#desplegando-los-cambios)
- [Objetivos de futuro](#objetivos-de-futuro)

# Introducción

Después de darle muchas vueltas, quería hacer el desarrollo de una aplicación desde el principio hasta el final. Tenía unas necesidades muy especificas que me ha costado encontrar como solventarlas. Entre ellas, la más importante es que la aplicación pueda funcionar completamente sin servidor, es decir, todo el código debía ser entregado por una pagina web que pueda desplegar en un servidor de contenido estático.

Este requisito hizo que aterrizara eligiendo [Nuxt3](https://nuxt.com/docs/getting-started/introduction). Te permite elegir entre multiples posibilidades para renderizar tu código, y para mi, poder elegir `client-side-rendering` sin dolor ninguno es genial. La idea es que desarrolle lo que desarrolle, pueda generar HTML, CSS y JS que todo sea puesto en una web.

Con todo, he desarrollado una aplicación super simple para gestionar tareas. La funcionalidad de la aplicación es muy básica y solo es como concepto, aunque me he divertido (y sufrido) peleándome con TailwindCSS y el diseño en general.

El repositorio lo puedes encontrar [aquí](https://github.com/jesusfj710/nuxt-to-do-app) y la aplicación [aquí](https://jesusfj710.github.io/to-do-app).

## Stack tecnológico

No solo he tenido que elegir [Nuxt3](https://nuxt.com/docs/getting-started/introduction), si no que también he escogido otras tecnologías para complementar su uso. Por ejemplo, para gestionar los estados de [Nuxt3](https://nuxt.com/docs/getting-started/introduction) he elegido [Pinia](https://pinia.vuejs.org), una librería que hace muy fácil esto para principiantes.

Para persistir los datos y gestionarlos, he elegido usar [LocalForage](https://github.com/localForage/localForage), que te permite elegir diferentes "back-ends" como almacenamiento. Más detalles adelante.

## Creando el proyecto con Nuxt3

La verdad es que solo seguí la documentación de [Nuxt3](https://nuxt.com/docs/getting-started/introduction) así como de [NuxtUI](https://ui.nuxt.com/getting-started). Si conoces como funciona VueJS, encontrarás que no hay ningún misterio aquí, ya que Nuxt es un meta-framework que añade más cosas encima de VueJS, como la posibilidad de usar `client-side-rendering` de manera super cómoda.

La idea era tener una aplicación de tareas sencilla, donde pueda añadir nuevas, elegir algunas como favoritas, y realizar otras. Todo esto, por supuesto, *full responsive*.

![](/images/projects/nuxt-to-do-app/to-do-app_laptop.png)

## Gestionando los datos

Como la aplicación puede usarse completamente offline y no va a haber ningún servidor, toda la información necesita ser persistida en el navegador del usuario. Hay muchas opciones, como usar el `localStorage` del navegador, usar `IndexedDB`, y miles más.

Dado que de momento la aplicación es super sencilla, implementé LocalForage. Es como un middle-ware entre tu aplicación y el back-end donde decides persistir los datos. Quizá con un gráfico se ve mejor:

{{< svg "static/images/blog/006/diagram.svg">}}

Puede que parezca que LocalForage es algo innecesario, ya que toda la lógica de persistir archivos podríamos hacerla desde Pinia, usando el `localStorage` sin ningún problema, pero esto es algo que quería hacer así ya que si en un futuro decido cambiar la forma de persistir los datos, solo tengo que cambiar una pequeña configuración en localForage, la implementación sería agnóstica al almacenamiento.

## Desplegando los cambios

Aquí he tomado inspiración claramente de mi [anterior articulo](https://jesusfj710.github.io/es-es/blog/transformando-la-structura-de-la-web/). En un [repositorio](https://github.com/jesusfj710/nuxt-to-do-app) tendré el código, y cada vez que algo sea mergeado en la rama principal, se hará build de la web y se enviará a otro [repositorio](https://github.com/jesusfj710/to-do-app), que lo publicará gracias a las Github Pages. Todo el código está libre y accesible para cualquiera que quiera revisarlo.

Los detalles los puedes ver en [este archivo](https://raw.githubusercontent.com/jesusfj710/nuxt-to-do-app/main/.github/workflows/buildAndPublish.yaml).

# Objetivos de futuro

La idea general es poder tener una PWA, con todos sus tests (unitarios, de componentes, E2E, etc), que pueda funcionar de manera completamente offline, y que quizá también ofrezca la posibilidad de sincronizar tareas entre dispositivos en un futuro.
