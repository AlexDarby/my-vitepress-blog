---
title: "Publishing a pnpm project on Cloudflare Pages"
description: A quick walkthrough for using pnpm with Cloudflare PAges.
aside: false
date: 2023-03-18
tags:
  - Node
  - pnpm
  - Cloudflare
---

There are several options for managing node/JS/TS projects, and not all of them are enabled within the Cloudflare Pages build process. While there is a [Community Feedback post](https://community.cloudflare.com/t/add-pnpm-to-pre-installed-cloudflare-pages-tools/288514) for an official solution, there are a couple of quick steps we can take to publish with pnpm.

Firstly, we will need to tell the Cloudflare Pages build agent to use a more up-to-date version of node. At time of writing the default is 12.x, but we can instruct the agent to use a version that will allow us to install and run pnpm. This is achieved by adding an environment variable in the Cloudflare Pages settings with the desired version.

```bash
NODE_VERSION=16.7.0
```

Lastly, in the build command configuration for the page, we need to add a one-liner that will install pnpm and then use pnpm to build our project.

```bash
npm install -g pnpm && pnpm i && pnpm build
```

Double check the build output and root directories are correct for your project and there we go, Cloudflare Pages with pnpm!