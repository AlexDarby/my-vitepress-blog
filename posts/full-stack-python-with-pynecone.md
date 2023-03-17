---
title: "Full-Stack Python with Pynecone."
description: The story so far.
aside: false
date: 2023-03-15
tags:
  - Python
  - Fly.io
---

I love making stuff in the simplest way possible. I really love reducing tooling to its simplest form. Over the last few years, I've been learning with a focus on a singular technology to deliver a complete experience. Frontend, backend, all in one. This has taken me through some fantastic frameworks, Django, TailwindCSS, and latterly a little Rails and Phoenix. 

I've learned a lot from these frameworks and approaches and 

This is why I was super excited to discover [Pynecone](https://pynecone.io/). Pynecone is certainly in the early days of its development but seems as though its focus on rapid prototyping and developer-friendly approach has it off to a compelling start.

## Prerequisites

To build and deploy our demo app, we'll need 
- Python 3.7+, pip, venv
- node 12.22.0+
- Docker 
- flyctl (I'm hosting with Fly.io, but any provider that can host a container, or static content will do)

So let's get straight into it... 

Let's set up a new directory and environment for our project

```bash
mkdir pynecone-app
cd pynecone-app
python3 -m venv .
source bin/activate
```

Now let's add and initialize Pynecone, and create a requirements file to record the project dependencies.

```bash
pip3 install pynecone
pc init
pip3 freeze > requirements.txt
```

Great! ... all done! now we can run the local server and navigate to localhost:3000 and see the Pynecone landing page...

```bash
pc run
```

All the display logic and styling for the page is held in the pynecone_app.py. Refer to the official [docs](https://pynecone.io/docs/getting-started/introduction) to make any changes you want, add wrapped React components if you can't achieve something out-of-the-box, or set up a SQLite database using the pc.Model ORM.

Pynecone do provide their own hosting platform, which at the time of writing is behind a waitlist. Can't wait to try deploying our app? Great, maybe fly.io can help.

We can create a Docker image of the project by adding the [Dockerfile](https://github.com/pynecone-io/pynecone/blob/main/docker-example/Dockerfile) supplied by the Pynecone team to the root of our project. Ensure the requirements.txt file is in the project root as well.

We will also need to change the pcconfig.py file to accommodate running within Docker...

```python
import pynecone as pc

config = pc.Config(
    app_name="app",
    api_url="0.0.0.0:8000",
    bun_path="/app/.bun/bin/bun",
    db_url="sqlite:///pynecone.db",
)
```

Now lets initialize flyctl and launch our container...

```bash
fly launch // This will take us through some guided steps to set up the project within Fly.io
fly deploy
```