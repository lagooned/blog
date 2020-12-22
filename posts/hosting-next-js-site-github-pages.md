---
title: "hosting next.js sites on github pages with github actions"
date: "2020-12-22"
---

hey y'all, as you all may know, github has a service called pages, which allows you to host static files for free. this post will will go over how to setup a continuous integration workflow to deploy a blog to pages.

# terms
- **[github pages](https://pages.github.com)**
  - a service that allows you to pick a particular branch and subdirectory of a git repository to use as a static hosting directory. it is a very handy service for creating small single page apps which run entirely on the browser.

- **[next.js](https://nextjs.org)**
  - a jam stack implementation based off react which provides many goodies for single-page front-end applications. i picked it because of it's ability to render markdown easily with a lightweight setup. this tutorial wont go too deep into next.js (as i admittedly don't know that much) and will only show what is necessary to get a next.js app working on github pages.

- **[github actions](https://github.com/features/actions)**
  - introduced in nov 2019, it allows you to program automated workflows based on events that happen withing your git repository. these workflows are concisely defined within yaml files which is added to your repository. we will see an example of one of these such yamls.

this post, as with everything i do, will be an agile<sup>tm</sup> work in progress, so please check back for more updates.

-Jared
