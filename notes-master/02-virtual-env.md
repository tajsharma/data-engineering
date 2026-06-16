# Module 1 Notes — Virtual Environments & Data Pipelines

> Study notes from `01-docker-terraform/docker-sql/02-virtual-environment.md`.
> For the pipeline I built and homework answers, see the [module README](../01-docker-terraform/README.md).

---

## The one big idea

A **virtual environment** isolates a project's dependencies so the project is reproducible and doesn't collide with other projects or your system Python. `uv` is the fast, modern tool that creates and manages that environment for you.

---

## Core concepts

**What a data pipeline is**
A service that takes data in and puts more data out. Classic shape: read a CSV → transform/clean it with pandas → write it somewhere (a Parquet file, a Postgres table, a warehouse). The `pipeline.py` script is a tiny first version of exactly that.

**Command-line arguments (`sys.argv`)**
Running `python pipeline.py 10` passes `10` into the script. `sys.argv` is the list of arguments: `sys.argv[0]` is the script name (`pipeline.py`), `sys.argv[1]` is the first real argument (`"10"`, as a string). Forgetting the argument is why you'd get `IndexError: list index out of range` — there's no `sys.argv[1]` to read.

**Why virtual environments exist**
`pip install pandas` installs packages *globally*. If Project A needs pandas 1.x and Project B needs 2.x, they fight. A virtual environment gives each project its own isolated set of dependencies, separate from other projects and from system Python. Same isolation idea as a Docker container, just at the Python-package level.

**uv**
A modern Python package + project manager (written in Rust, much faster than pip). It creates and manages the virtual environment automatically, so you don't hand-manage `venv`/`activate`.

**The project files uv creates**
- `pyproject.toml` — declares your project's dependencies (the source of truth)
- `.python-version` — pins which Python version the project uses
- `uv.lock` — locks exact resolved versions so installs are reproducible
- `.venv/` — the actual isolated environment folder (don't commit it)

**`uv run` vs. plain `python`**
`uv run python` uses the project's isolated environment. Plain `python` (or `python3`) uses your system Python. They're different interpreters with different installed packages — that difference is the whole point.

---

## Command cheatsheet

| Command | What it does |
| --- | --- |
| `pip3 install uv` | Install uv itself (on macOS, `pip` may not be on PATH — use `pip3`) |
| `uv init --python=3.13` | Initialize a project; creates `pyproject.toml` + `.python-version` |
| `uv add pandas pyarrow` | Add dependencies — updates `pyproject.toml` and installs into the venv |
| `uv run python pipeline.py 10` | Run a script inside the venv, passing `10` as an argument |
| `uv run which python` | Show the path to the venv's Python |
| `uv run python -V` | Show the venv's Python version |
| `which python` / `python -V` | Show system Python (for comparison) |

**Mac gotchas:** use `pip3`, not `pip`. The flag is `--python`, not `--python3`. And make sure the filename you run matches the file exactly — `pipeline.py` ≠ `pipline.py`.

---

## Why this matters for data engineering

Every pipeline needs **isolated, reproducible dependencies** so it behaves identically on your laptop, in CI, inside a container, and on the cloud. `pyproject.toml` + `uv.lock` are that reproducibility contract — they let the next step (Dockerizing the pipeline) install the *exact* same package versions inside the container. Virtual environment first, container second: same isolation principle, two layers.

---

## Self-check (quiz yourself)

1. What problem does a virtual environment solve that global `pip install` causes?
2. What's the difference between running `uv run python` and just `python`?
3. What does `uv init` create, and what is each file for?
4. What does `uv add pandas` change, beyond installing the package?
5. In `python pipeline.py 10`, what is `sys.argv[1]` and what type is it?
6. Why add `*.parquet` to `.gitignore`?

<details>
<summary>Answers</summary>

1. Global installs put every project's packages in one shared place, so projects needing different versions of the same package conflict. A venv isolates each project's dependencies.
2. `uv run python` uses the project's isolated virtual environment; plain `python` uses your system Python. Different interpreters, different installed packages.
3. `pyproject.toml` (declared dependencies), `.python-version` (pinned Python version), and `.venv/` (the environment itself). `uv.lock` gets created/updated when you add packages, locking exact versions.
4. It also records the dependency in `pyproject.toml` and updates `uv.lock`, so the environment is reproducible — not just a one-off install.
5. `"10"` — the first command-line argument, and it's a **string**, which is why the script casts it with `int(sys.argv[1])`.
6. It's a generated binary output file; you don't want generated artifacts cluttering or bloating your git history.

</details>
