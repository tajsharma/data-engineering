# Module 1 Notes — Dockerizing the Pipeline

> Study notes from `01-docker-terraform/docker-sql/03-dockerizing-pipeline.md`.
> For the pipeline I built and homework answers, see the [module README](../01-docker-terraform/README.md).

---

## The one big idea

A **Dockerfile** is a recipe that bakes your code *and* its environment into a portable **image**. `docker build` cooks the recipe into the image; `docker run` serves it as a running container. The payoff: your pipeline now runs identically on any machine.

---

## Core concepts

**Dockerfile → image → container**
- *Dockerfile*: a text file of instructions describing how to build the environment (the recipe).
- *Image*: the built, frozen result of running those instructions (the baked dish).
- *Container*: a running instance of the image (a served plate).
One Dockerfile builds one image; one image runs as many containers as you want.

**Instructions live in the file, not the terminal**
`FROM`, `RUN`, `COPY`, etc. are Dockerfile instructions — they only mean something to Docker when it reads the file during a build. Typing them into your shell fails with "command not found" because they aren't shell commands. In the terminal you only ever type `docker ...` commands.

**Build context — the `.`**
In `docker build -t test:pandas .`, the `.` is the build context: the folder Docker is allowed to see and copy from. Every file a `COPY` line references must live inside it. That's why the Dockerfile sits in the same folder as `pipeline.py` and the dependency files.

**Image naming and tags**
`-t test:pandas` names the image `test` with tag `pandas` (format `name:tag`). Omit the tag and it defaults to `latest`. Tags let you version images (e.g. `myimage:v1`, `myimage:v2`).

**Layer caching — why dependency files are copied first**
Docker builds in layers and caches them. The uv Dockerfile copies `pyproject.toml`/`uv.lock` and runs `uv sync` *before* copying `pipeline.py`. So when you edit only your code, Docker reuses the cached dependency layer instead of reinstalling everything — much faster rebuilds. Copy the slow-changing stuff (deps) before the fast-changing stuff (your code).

**Build-time vs run-time**
`RUN` executes while *building* the image (installing packages). `ENTRYPOINT` defines what runs when the *container starts*. Arguments you pass to `docker run` get appended to the ENTRYPOINT — so `docker run -it test:pandas 10` effectively runs `python pipeline.py 10` inside the container.

---

## Dockerfile instruction reference

| Instruction | What it does |
| --- | --- |
| `FROM` | The base image to build on (e.g. `python:3.13-slim`) |
| `RUN` | Execute a command at **build time** (e.g. install dependencies) |
| `WORKDIR` | Set the working directory inside the image |
| `COPY` | Copy files from the build context into the image (`COPY source dest`) |
| `ENV` | Set an environment variable inside the image |
| `ENTRYPOINT` | The command that runs when the **container starts** |

---

## Command cheatsheet

| Command | What it does |
| --- | --- |
| `docker build -t test:pandas .` | Build an image named `test:pandas` from the Dockerfile in the current folder |
| `docker run -it test:pandas 10` | Run the image as a container, passing `10` to the entrypoint |
| `docker images` | List images you've built locally |

**Gotcha:** the file must be named exactly `Dockerfile` (capital D, no extension), and you build from the folder it lives in.

---

## Why this matters for data engineering

The Dockerfile is the **shippable, reproducible definition of your whole pipeline** — environment + dependencies + code in one artifact that runs the same on your laptop, in CI, and on the cloud. This is the unit you actually deploy. It also sets up the next steps: you're about to run Postgres as a container and connect your pipeline to it, which is the real shape of a containerized data stack.

---

## Self-check (quiz yourself)

1. What's the difference between a Dockerfile, an image, and a container?
2. Why does the uv Dockerfile copy `pyproject.toml`/`uv.lock` *before* copying `pipeline.py`?
3. What does the `.` in `docker build -t test:pandas .` refer to?
4. What's the difference between `RUN` and `ENTRYPOINT`?
5. In `docker run -it test:pandas 10`, where does the `10` end up?
6. Why does typing `FROM python:3.13-slim` into your shell fail?

<details>
<summary>Answers</summary>

1. A Dockerfile is the recipe (instructions); an image is the built/frozen result; a container is a running instance of an image.
2. Layer caching — dependencies change rarely, so caching that layer means editing only your code doesn't trigger a full reinstall. Faster rebuilds.
3. The build context — the folder Docker can see and copy files from during the build.
4. `RUN` runs at build time (e.g. installing packages into the image); `ENTRYPOINT` runs at container start time (what the container does when launched).
5. It's appended to the ENTRYPOINT, becoming the argument to the script — effectively `python pipeline.py 10` inside the container, read as `sys.argv[1]`.
6. `FROM` is a Dockerfile instruction, not a shell command. The shell has no `FROM` program, so it errors. Those lines only mean something to Docker when it reads the Dockerfile during a build.

</details>
