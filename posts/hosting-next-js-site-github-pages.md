---
title: "hosting next.js sites on github pages with github actions"
date: "2020-12-22"
---

hey y'all! as you may or may not know, github has a service called pages, which allows you to host static files for free. this post will go over how to setup a continuous integration workflow to deploy a blog to pages.

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

this will push code to the default branch on your repository. when visiting [the repository](https://github.com/lagooned/blog) on github, you should see your entire root directory and the readme rendered.

# creating the workflow

great, now we've pushed the code to our repository, we need to setup a workflow which will deploy it. we do this by creating a workflow yaml file. these are placed under the root directory of the repository in the .github/workflow directory. you can create this file like so:

```bash
$ touch .github/workflows/node.js.yaml
```

this file is called *node.js.yaml* because the build tool we will be using is npm (node package manager), which hopefully you have some experience with by now after the next.js tutorial. 

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

the final step requires the setup of a [personal access token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) with the following permissions:

- repo:status
- repo_deployment
- public_repo
- repo:invite

add this token's value to your repository as a secret with the id **ACTIONS_DEPLOY_ACCESS_TOKEN** (as referred in the yaml via the *secrets.\** namespace). this token will allow the workflow to push to the gh-pages branch of your repository.

add this yaml to your repository and commit:

```bash
$ git add .github/workflow/node.js.yaml
$ git commit -m "added node.js.yaml actions workflow"
```

# configure base path

when deploying to github pages, there is a few major caveats, the first being the implicit url path segmented added to the url which matches the repo name. for example, if i created the repository
[https://github.com/lagooned/blog](https://github.com/lagooned/blog) the cooresponding pages host url will become [https://lagooned.github.io/blog](https://lagooned.github.io/blog). this **/blog** on the end of the domain creates an issue, as the static generated files will miss this very necessary path segment, thus causing 404's when attempting to load static assets.

to fix this, create a file called *next.config.js* under the root directory of your project with something along the lines of the following contents:

```javascript
module.exports = {
  basePath: '/blog'
}
```

make sure this value matches your repo name, and perspective base path segment. don't forget to commit!

```bash
$ git add next.config.js
$ git commit -m "added basepath configuration via next.config.js"
```

# setup npm tasks

npm tasks are very useful for automating common tasks for your repo. the definitions for these tasks reside in the **scripts** section of the package.json. modify the *build* task and add the following task called *deploy* to your package.json so that it looks like so:

```json
...
"scripts": {
  "dev": "next dev",
  "build": "next build && next export -o build",
  "start": "next start",
  "deploy": "touch build/.nojekyll && gh-pages -d build -t true"
},
...
```

this modification to the **build** task will include an instrumental step- to generate the static files to deploy to pages, and place the output in a directory called *build*. in addition, the new **deploy** task will create an empty file called .nojekyll in the newly created build directory and deploy using the gh-pages npm package.

there are a few things to unpack here. the .nojekyll file is to circumvent a pretty big quirk of github pages. [jekyll](https://jekyllrb.com) is a ruby-based, markdown blog generation platform, kinda similar to the one we're currently making. it does a similar process of generating static files, however, it uses special files which start with underscores. github pages natively supports interpreting these files, and this support conflicts with next.js' default static file *_next* directory. adding this .nojekyll file will disable this additional processing and allow our static assets to be retrieved without any issues.

finally, commit and push:

```bash
$ git add package.json
$ git commit -m "modified build and added deploy tasks"
$ git push origin HEAD
```

# conclusion

visit your repo, click the actions tab, and watch your workflow start and run to completion. this will create a branch on your repository called 'gh-pages' which will contain your newly-generated static assets.

visit your pages url and enjoy your app, lmk via email if you have any trouble. :)

-jared
