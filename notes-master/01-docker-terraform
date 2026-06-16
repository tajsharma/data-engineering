# Module 1 Notes — Docker Fundamentals

> Study notes from `01-docker-terraform/docker-sql/01-introduction.md`.
> For the pipeline I built and homework answers, see the [module README](../01-docker-terraform/README.md).

---

## The one big idea

A container is a **disposable, isolated, reproducible environment**, and *you* decide what persists. Everything else in this section is a detail hanging off that sentence.

---

## Core concepts

**Image vs. container**
An *image* is a snapshot/blueprint (e.g. `python:3.9.16-slim`). A *container* is a running instance of an image. One image → many containers.

**Stateless by default**
Changes made inside a running container are lost when it stops. This is a feature, not a bug: every run starts from a clean, known state, which is what makes Docker reproducible. Install Python in an Ubuntu container, exit, restart → Python is gone.

**Isolation**
A container is sealed off from the host. Running something destructive *inside* a container (the `rm -rf /` demo) wrecks the container, not your Mac. Run the same thing on your host and you'd destroy your machine.

**Volumes**
The deliberate mechanism for persisting or sharing data past a container's lifetime. A volume maps a host path to a container path, so the container can read/write files that outlive it. This is *the* concept the rest of the module depends on — e.g. mounting a volume into Postgres so loaded data survives a restart.

**Base images**
Start from an image that already has what you need instead of building from scratch. `ubuntu` = bare Linux (no Python). `python:3.9.16-slim` = Python pre-installed, `-slim` = smaller footprint. Prefer slim variants to keep images lean.

---

## Command cheatsheet

| Command | What it does |
| --- | --- |
| `docker --version` | Check the installed Docker version |
| `docker run hello-world` | Smoke-test that Docker works |
| `docker run -it ubuntu` | Start an interactive Ubuntu container (`-it` = interactive terminal) |
| `docker run -it --rm ubuntu` | Same, but auto-delete the container on exit (`--rm`) |
| `docker ps -a` | List all containers, including stopped ones |
| `docker rm $(docker ps -aq)` | Remove all containers (cleanup) |
| `docker run -it --rm --entrypoint=bash python:3.9.16-slim` | Override the default entrypoint to get a bash shell |
| `docker run -it --rm -v $(pwd)/test:/app/test --entrypoint=bash python:3.9.16-slim` | Mount a host folder into the container (`-v host:container`) |

**Flags to remember:** `-it` interactive shell · `--rm` auto-delete on exit · `-v` mount a volume · `--entrypoint` override what the container runs on start.

---

## Why this matters for data engineering

A data pipeline is just **code in a container operating on data via a volume**, running identically on a laptop or in the cloud. The stateless-container + volume pattern is what lets a pipeline be reproducible and portable — build it once on your Mac, run it unchanged on GCP. The next steps in this module turn that abstract idea into a real Postgres + ingestion-script pipeline.

---

## Self-check (quiz yourself)

1. What happens to changes made *inside* a container when it stops? Why is that desirable?
2. If statelessness wipes everything on stop, how do you make data persist or share it with the host?
3. Why is `rm -rf /` safe inside a container but catastrophic on your host?
4. What's the difference between the `ubuntu` and `python:3.9.16-slim` base images, and why prefer `-slim`?
5. What does `--rm` do, and why use it routinely?
6. In `-v $(pwd)/test:/app/test`, which side is the host and which is the container?

<details>
<summary>Answers</summary>

1. They're discarded — the container reverts to its image's clean state. Reproducibility: every run starts identical.
2. Use a **volume** to map a host path into the container so data outlives the container.
3. The container's filesystem is isolated from the host; destroying it doesn't touch your machine.
4. `ubuntu` is bare Linux with no Python; `python:3.9.16-slim` ships Python pre-installed. `-slim` is a smaller image — faster pulls, less bloat.
5. Auto-deletes the container on exit so stopped containers don't pile up.
6. `$(pwd)/test` is the **host** path; `/app/test` is the **container** path. Format is `host:container`.

</details>
