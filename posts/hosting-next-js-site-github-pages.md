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
  - introduced in nov 2019, it allows you to program automated workflows based on events that happen within your git repository. these workflows are concisely defined within yaml files which is added to your repository. we will see an example of one of these such yamls.

# setting up the repo

in order to get a starter next.js app you can follow [this guide](https://nextjs.org/learn/basics/create-nextjs-app). this will give you a good introduction to next.js and its capabilities, as well as provide you with something to deploy. i will be using a site that has been filled out by following this guide up through the *dynamic routes* section, yet the steps will be the same for any next.js app.

once you have gone through this tutorial you should be left with a git repo containing your new app. also, if you haven't already, make sure to setup your github account with ssh key access by following the guide [here](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh). this will help smooth out the authentication step when pushing your git repo so you don't have to type your password every time. in order to push my blog i executed the following commands:

```bash
    $ git remote add origin git@github.com:lagooned/blog.git
    $ git push origin HEAD
```

this will push code to the default branch on your repository. when visiting [the repository](https://github.com/lagooned/blog) on you should see your entire root directory and the readme rendered.

# creating the workflow

great, now we've pushed the code to our repository, we need to setup a workflow which will deploy it. we do this by creating a workflow yaml file. these are placed under the root directory of the repository in the .github/workflow directory. you can create this file like so:

```bash
    $ touch .github/workflows/node.js.yaml
```

this file is called *node.js.yaml* because the build tool we will be using is npm, which hopefully you have some experience with by now after the next.js tutorial. 

[here](https://github.com/lagooned/blog/blob/main/.github/workflows/node.js.yml) is the contents of the workflow. this file defines upon a push to a particular branch, in this case **main**, execute this workflow. in looking closer at the yaml, this workflow is a build process which executes on the latest version of ubuntu (hence ubuntu-latest), and requires the latest version of node.js 12 (hence 12.x). this workflow also is comprised of many jobs which run in order. in looking at the **steps** section of the configuration, we find these steps:

```yaml
    steps:
    - name: checkout
    ...
    - name: require node ${{ matrix.node-version }}
    ...
    - name: install deps
    ...
    - name: build static pages
    ...
    - name: deploy to gh-pages
    ...
```

these are the main steps of our workflow. first we checkout our repo from github; then pull in our build tool runtime, **node.js**, as well as our build tool, **npm**; subsequently install the dependencies defined with in our project manifest (aka the package.json and package-lock.json); export our next.js app to a bundle of static resources; and finally deploy our static resources to github pages.

add this yaml to your repository and push:

```bash
    $ git add .github/workflow/node.js.yaml
    $ git commit -m "added node.js.yaml actions workflow"
    $ git push origin HEAD
```

this post, as with everything i do, will be an agile<sup>tm</sup> work in progress, so please check back for more updates.

-jared
