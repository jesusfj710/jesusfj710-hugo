---
title: Transforming the structure of the web repos
slug: transforming-the-structure-of-the-web-repos
tags: ["Hugo", "Github", "Github Actions", "codabal.ai", "Github Pages"]
date: 2023-05-15T18:00:00+0200
---

**2023/08/05 Update**: Codeball.ai is no longer available. I have remove the actions related to it but the post will remain as it is for archival purposes.

---

Hello everyone!

The other day I was busy going through some information about Hugo, Github Actions, etc and decided to change the way the website works because it seemed like the right thing to do.

## The before

When I first published the website, everything was contained in a single repository.

In this repository I had everything related to Hugo, and the workflow went something like this:
1. I started the Hugo server on my local, running `hugo serve`.
2. I would access my local url and see the server.
3. I would create a branch and make the necessary changes.
4. Review the changes in my locale. Repeat steps 3 and 4 as many times as necessary.
5. Create a *commit* with the changes and push them to origin on Github.
6. Create a *Pull Request*.
7. It runs a {{< abbrebiation abbrebiation="GHA" description="Github Action" >}} of [*codeball.ai*](https://codeball.ai) which parses the code and gives you a trust factor.
8. Merge the {{< abbrebiation abbrebiation="PR" description="Pull Request" >}} to `main`.
9. With each new commit in main, run another GHA that builds the Hugo static files. Basically it runs `hugo` which generates the `public` folder. Once the files are generated, they are published to Github Pages with another GHA.
 
This flow actually works well but has several disadvantages. You can't keep the repository private if you want to use Github Pages, you have a lot of *codebase* and generated files in the same repository, etc.

With the new flow, you could have the repository with the static files public, and the original one, which does the *build* of the web, private (although this is not my case).

## New workflow

In this new workflow we are going to have two repositories, one that is like the old one, where all the Hugo codebase is, and another one where just the web statics will be.

And really, once the changes are implemented, the flow doesn't change much either. The last step of the old flow is where we make changes: now we generate the `public` folder in the original repository, but we don't publish it. Now it is uploaded to the new repository with a GHA. Once this information is uploaded to the new repository, the publication of the static website is executed.

This graphic may explain the flow a little bit, although I'm sure it's poorly defined because I never learned sequential diagrams 😅: {{< svg "static/images/blog/005/diagram.svg">}}

This is useful if for example we want to hide the original reposition. In my case, I'm using *codeball.ai* and it's only free for the public repos, so for now, I'll keep both repos public, but who knows.

---

That's all, thank you very much for reading!
