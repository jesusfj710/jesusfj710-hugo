---
title: Creando un shortcode para insertar SVG en Hugo
type: page
topic: hugo, shortcode, svg
date: 2023-05-14T18:00:00+0200
---

¡Hola!

Recientemente cambié mi flujo de trabajo para trabajar con la web, y para mostrar los detalles, quería insertar un gráfico. Estaba utilizando [Excalidraw](https://excalidraw.com), y la ventaja es que puedes exportarlo como [{{< abbrebiation abbrebiation="SVG" description="Scalable vector graphics" >}}](https://www.w3.org/Graphics/SVG/).

## Limitaciones encontradas 🚧

Al intentar importar el SVG directamente en el contenido de la entrada, me encontré con ciertos problemas. Más que problemas, eran limitaciones como las siguientes:

* La necesidad de definir un fondo.
* Limitado a un único perfil de color. No se puede tener uno para el modo claro y otro para el modo oscuro.
* No se pueden modificar los estilos, ya que se insertaba como una etiqueta `<img>`, que no admite estilos heredados (información útil disponible [aquí](https://discourse.gohugo.io/t/solved-inject-an-svg-file-into-my-html/7446/9)).

## Creando el *shortcode* 🥾

Con la información del último punto en la sección anterior, podemos crear nuestro propio *shortcode*. Así que decidí aplicar eso y extender un poco más el comportamiento. Aquí tienes un ejemplo utilizando el siguiente fragmento:

```
{{</* svg "static/images/blog/004/example.svg" */>}}
```

{{< svg "static/images/blog/004/example.svg">}}

La primera limitación que veo en mi implementación es el ancho, que en este ejemplo se ve claramente enorme y no es necesario, así que necesito hacer que el ancho sea un parámetro opcional.

Pasamos de este *shortcode* que no permitía establecer el ancho (estaba predefinido como 100% en los estilos):
```
{{$svg := .Get 0}}
<div class="svg-fill-background">
    {{ $svg | readFile | safeHTML }}
</div>
```
Al siguiente, donde sí podemos indicar opcionalmente el porcentaje de ancho, y el cambio se puede ver a continuación (podríamos extender esto a más propiedades, pero no es necesario por ahora):

```
{{$svg := .Get 0}}
{{$width := .Get 1 | default "100%"}}

<div class="svg-fill-background" style="width: {{$width}}; margin: 0 auto">
    {{ $svg | readFile | safeHTML }}
</div>
```

```
{{</* svg "static/images/blog/004/example.svg" "40%"*/>}}
```

{{< svg "static/images/blog/004/example.svg" "40%">}}

## Modo claro y modo oscuro 🌗

Si cambias la web entre los dos modos, puedes ver cómo el gráfico cambia de color. En realidad, funciona siendo un único gráfico SVG que se colorea con [{{< abbrebiation abbrebiation="CSS" description="Cascading Style Sheets" >}}](https://www.w3.org/Style/CSS/).

```scss
body .svg-fill-background svg {
    background: var(--code-bg);
    max-width: 100%;
    height: 100%;

    path {
        stroke: var(--primary)
    }

    text {
        fill: var(--content)
    }
}
```

----

Podeis revisar como han quedado al final los cambios en la [PR](https://github.com/jesusfj710/jesusfj710-hugo/pull/28) que preparé en el repositorio de mi Github.

¡Salud!

