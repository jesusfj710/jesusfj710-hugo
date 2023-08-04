---
title: Creating a shortcode to insert SVG in Hugo
slug: creating-a-shortcode-to-insert-svg-in-hugo
tags: ["Hugo", "shortcode", "SVG"]
date: 2023-05-14T18:00:00+0200
---

Hello!

I recently changed my workflow to work with the web, and to show the details, I wanted to insert a graphic. I was using [Excalidraw](https://excalidraw.com), and the advantage is that you can export it as [{{< abbrebiation abbrebiation="SVG" description="Scalable vector graphics" >}}](https://www.w3.org/Graphics/SVG/).

## Limitations encountered 🚧

When I tried to import the SVG directly into the content of the entry, I ran into some problems. More than problems, they were limitations such as the following:

* The need to define a background.
* Limited to a single colour profile. You cannot have one for light mode and one for dark mode.
* Cannot modify styles, as it was inserted as an `<img>` tag, which does not support inherited styles (useful info available [here](https://discourse.gohugo.io/t/solved-inject-an-svg-file-into-my-html/7446/9)).

## Creating the *shortcode* 🥾

With the information from the last point in the previous section, we can create our own *shortcode*. So I decided to apply that and extend the behaviour a bit more. Here's an example using the following snippet:

```
{{</* svg "static/images/blog/004/example.svg" */>}}
```

{{< svg "static/images/blog/004/example.svg">}}

The first limitation I see in my implementation is the width, which in this example looks clearly huge and is not necessary, so I need to make the width an optional parameter.

We moved on from this *shortcode* that did not allow to set the width (it was predefined as 100% in the styles):
```
{{$svg := .Get 0}}
<div class="svg-fill-background">
    {{ $svg | readFile | safeHTML }}
</div>
```
To the next one, where we can optionally indicate the percentage width, and the change can be seen below (we could extend this to more properties, but it's not necessary for now):

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

## Light mode and dark mode 🌗

If you switch the web between the two modes, you can see how the graphic changes colour. It actually works as a single SVG graphic that is coloured with [{{< abbrebiation abbrebiation="CSS" description="Cascading Style Sheets" >}}](https://www.w3.org/Style/CSS/).

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

You can check how the changes have turned out in the [PR](https://github.com/jesusfj710/jesusfj710-hugo/pull/28) I prepared in my Github repository.

Cheers!
