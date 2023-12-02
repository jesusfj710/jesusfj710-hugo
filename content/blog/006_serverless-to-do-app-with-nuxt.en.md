---
title: Serverless web-based to-do application with Nuxt3
slug: to-do-web-application-serverless-nuxt3
tags: ["nuxt", "to-do app", "serverless", "pinia", "localforate", "localstorage", "nuxtui", "bun"]
date: 2023-12-02T13:00:00+0200
---
- [Introduction](#introduction)
  - [Technology Stack](#technology-stack)
  - [Creating the Project with Nuxt3](#creating-the-project-with-nuxt3)
  - [Managing Data](#managing-data)
  - [Deploying Changes](#deploying-changes)
- [Future Objectives](#future-objectives)

# Introduction

After much consideration, I wanted to document the development of an application from start to finish. I had some very specific requirements that I found challenging to address. The most crucial among them was the need for the application to function entirely without a server. In other words, all the code had to be delivered by a static content server.

This requirement led me to choose [Nuxt3](https://nuxt.com/docs/getting-started/introduction). It allows you to choose from multiple rendering options, and for me, the ability to choose `client-side-rendering` painlessly is fantastic. The idea is that whatever I develop, I can generate HTML, CSS, and JS that can all be deployed on a static hosting service.

For this, I developed a super-simple task management application. The functionality of the application is very basic, serving more as a concept, although I had fun (and struggled) working with TailwindCSS and design in general.

You can find the repository [here](https://github.com/jesusfj710/nuxt-to-do-app) and the application [here](https://jesusfj710.github.io/to-do-app).

## Technology Stack

I not only had to choose [Nuxt3](https://nuxt.com/docs/getting-started/introduction) but also selected other technologies to complement its use. For managing states in [Nuxt3](https://nuxt.com/docs/getting-started/introduction), I chose [Pinia](https://pinia.vuejs.org), a library that makes this very easy for beginners.

To persist and manage data, I chose to use [LocalForage](https://github.com/localForage/localForage), allowing you to choose different backends for storage. More details later.

## Creating the Project with Nuxt3

To be honest, I just followed the documentation of [Nuxt3](https://nuxt.com/docs/getting-started/introduction) and [NuxtUI](https://ui.nuxt.com/getting-started). If you know how VueJS works, you'll find that there's no mystery here, as Nuxt is a meta-framework that adds more things on top of VueJS, such as the ability to use `client-side-rendering` very comfortably.

The idea was to have a simple task application where I could add new tasks, choose some as favorites, and mark others as completed. All this, of course, *fully responsive*.

![](/images/projects/nuxt-to-do-app/to-do-app_laptop.png)

## Managing Data

As the application can be used completely offline and there won't be any server, all information needs to be persisted in the user's browser. There are many options, such as using the browser's `localStorage`, using `IndexedDB`, and many more.

Since the application is currently very simple, I implemented LocalForage. It acts as middleware between your application and the backend where you decide to persist the data. Perhaps it's clearer with a diagram:

{{< svg "static/images/blog/006/diagram.svg">}}

LocalForage might seem unnecessary, as we could handle file persistence from Pinia using `localStorage` without any problem. But I wanted to do it this way because if in the future I decide to change the way data is persisted, I only have to change a small configuration in LocalForage, and the implementation would be agnostic to storage.

## Deploying Changes

Here I clearly took inspiration from my [previous article](https://jesusfj710.github.io/blog/transforming-the-structure-of-the-web-repos). In one [repository](https://github.com/jesusfj710/nuxt-to-do-app), I have the code, and each time something is merged into the main branch, the web is built, and it is sent to another [repository](https://github.com/jesusfj710/to-do-app), which publishes it thanks to GitHub Pages. All the code is free and accessible for anyone to review.

You can see the details in [this file](https://raw.githubusercontent.com/jesusfj710/nuxt-to-do-app/main/.github/workflows/buildAndPublish.yaml).

# Future Objectives

The general idea is to have a PWA, with all its tests (unit, component, E2E, etc.), that can work completely offline and might also offer the possibility of synchronizing tasks between devices in the future.
