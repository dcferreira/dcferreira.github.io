---
title: Making and Deploying an AI Web App in 2023 (Part 3)
subtitle: Setup the Python Project with Modern Tools

# Summary for listings and search engines
summary: How to setup a Python project for easy developing with black, ruff, pre-commit hooks, and type hints.

# Link this post with a project
projects: []

# Date published
date: "2023-03-08T00:03:00Z"

# Date updated
lastmod: "2023-03-08T00:03:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)"
  focal_point: ""
  placement: 2
  preview_only: false

authors:
  - admin

tags:
  - AI
  - Machine Learning
  - Web App
  - txtai
  - Web Development
  - Serverless
  - NLP

categories:
  - AI
  - NLP
---

{{% callout note %}}
This is part of a multi-part blogpost about how to build an AI Web App.
Please refer to [Part 1](/post/2023-03-01-ai-web-app) for more context.
{{% /callout %}}

In this post we will have a look at how to setup Python projects using modern
tools in the Python ecosystem.
This post is all about the tools, and there's basically no code.

The tools recommended here are mostly very new, and in my opinion, better than
the older alternatives.
If you haven't yet tried these tools but are familiar with the alternatives,
I'd suggest you give them a go: all of these tools had basically 0 friction
while integrating with my workflows.

# Project Manager

{{% callout note %}}
Alternatives to `hatch`: [Poetry](https://python-poetry.org/), [PDM](https://pdm.fming.dev/latest/),
[Conda](https://docs.conda.io/en/latest/), [venv](https://docs.python.org/3/library/venv.html).
{{% /callout %}}

My currently favorite project manager is [hatch](https://hatch.pypa.io/).
If you've never used a project manager before, its main job is keeping track of your dependencies,
making sure there are no incompatibilities between libraries, and keeping your environment clean of unnecessary clutter.
Project managers also allow you to build or publish your own Python packages with a single command.

I like `hatch` in particular because it's super fast at resolving dependencies
(unlike Poetry [[1]](https://github.com/python-poetry/poetry/issues/2094) [[2]](https://github.com/python-poetry/poetry/issues/4924))
and has a utility for running scripts and an environment manager.
These last 2 features allow `hatch` to replace [tox](https://tox.wiki/en/latest/) for many use cases.

{{% callout warning %}}
At the moment `hatch` has a big downside compared to `Poetry`, in that it can't pin dependencies.
You can specify the library versions you want, but their dependencies' versions won't be fixed.
(See [their FAQ about libraries vs applications](https://hatch.pypa.io/latest/meta/faq/#libraries-vs-applications))

Hopefully this will be solved in the future.
{{% /callout %}}

So the first step is to install `hatch`

```bash
pip install hatch
```

We can then start a new project by running the following command and giving a project name and description

```bash
hatch new -i
```

This command will create a new directory and populate it with an empty template

```bash
ai-web-app
├── ai_web_app
│   ├── __about__.py
│   └── __init__.py
├── tests
│   └── __init__.py
├── LICENSE.txt
├── README.md
└── pyproject.toml
```

The most interesting file here is `pyproject.toml`, where you will keep all your project's configurations.

# Linter

{{% callout note %}}
Alternatives to `ruff`: [flake8](https://flake8.pycqa.org/en/latest/), [FlakeHeaven](https://flakeheaven.readthedocs.io/en/latest/),
[FlakeHell](https://flakehell.readthedocs.io/).

I'm not aware of good alternatives to `black` or `isort`.
{{% /callout %}}

For linting, my favorite tool is [ruff](https://github.com/charliermarsh/ruff).
It's significantly faster than any other linter, and very simple to setup and forget.

Another great tool for keeping the style the same throughout the code is [black](https://black.readthedocs.io/en/stable/).
It's an opinionated code formatter.
I love it because I can just set it up in my IDE to run whenever I save a file, and then I don't have to worry about
style or formatting when writing the code.
If you're collaborating on the same code with someone else, `black` is really indispensable: it forces your team to keep
the same style throughout the code, and it makes the git differences much simpler.

The final tool here is [isort](https://pycqa.github.io/isort/).
This is a very simple tool, which forces imports to have a specific order.
That's it!
That also makes the git diffs simpler, and is simple to setup and forget.

To install all of these tools, you just need to edit the `pyproject.toml` file.
Look for the dev dependencies section (it's the `default` env in `hatch`), and just add them there.
At this point, your dev dependencies section should look like this:

```toml
[tool.hatch.envs.default]
dependencies = [
  "pytest",
  "pytest-cov",
  "black",
  "isort",
  "ruff",
]
```

`isort` needs a bit of configuration to play nice with `black` (see [their compatibility guide](https://pycqa.github.io/isort/docs/configuration/black_compatibility.html)).
Just add the following to the end of `pyproject.toml`:

```toml
[tool.isort]
profile = "black"
```

The final step in this section is to make use of `hatch` and its scripting features to run our linting.
Look for the scripts section in the `pyproject.toml` file (`[tool.hatch.envs.default.scripts]`) and these 2 new lines
(the first lines should be there if you used `hatch init`):

```toml
[tool.hatch.envs.default.scripts]
cov = "pytest --cov-report=term-missing --cov-config=pyproject.toml --cov=ai_web_app --cov=tests {args}"
no-cov = "cov --no-cov {args}"
lint = ["ruff .", "black . --check -q", "isort . --check -q"]
format = ["black .", "isort ."]
```

# Pre-commit hooks

Another essential tool, especially if you work in a team: pre-commit git hooks.
These are scripts that will be executed before every commit you make, to make
sure you don't make a stupid mistake and commit it into the repo.
You can enforce linting in this step, and make it automatic.

For this you need the [pre-commit](https://pre-commit.com/) Python package (just add it to your dev environment, like above),
and a config file.
You can just take my configuration, which uses the tools mentioned above.
Create a `.pre-commit-config.yaml` file and paste this:

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
      - id: check-yaml
      - id: end-of-file-fixer
      - id: trailing-whitespace
  - repo: https://github.com/pycqa/isort
    rev: 5.11.2
    hooks:
      - id: isort
        name: isort (python)
  - repo: https://github.com/psf/black
    rev: 22.12.0
    hooks:
      - id: black
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    # Ruff version.
    rev: "v0.0.211"
    hooks:
      - id: ruff
        # Respect `exclude` and `extend-exclude` settings.
        args: ["--force-exclude"]
```

Finally, run

```bash
hatch run pre-commit install
```

Your git hooks will be installed, and all those listed tools will run for every commit that you make.

# Type Hinting

Python now supports type hinting, and I've been trying to write almost all of my code using it.
If you're using an IDE, it will often alert you of simple mistakes you make, or edge cases you didn't consider.

However, unfortunately the whole ecosystem isn't great.
Ideally every library you use would have type hints, and you could run [mypy](https://mypy-lang.org/) in
[strict mode](https://mypy.readthedocs.io/en/stable/getting_started.html?highlight=strict#strict-mode-and-configuration).
But most libraries don't have type hints, and then mypy has no way of knowing what the types of your imports are.

All that being said, I would still recommend developing with type hints, and using at least your IDE for warnings.
If most of the libraries you're using support type hints, you should also be running `mypy` often (maybe even as a pre-commit hook?).

In this particular case, the main library I'm using is `txtai` and it doesn't have type hints, so I won't be using
`mypy` much.
However, this is how you would set it up: add `mypy` to your dev dependencies (like above), and add a new script in
`pyproject.toml` to run it (e.g., `types = "mypy ."`).
Furthermore, you should also make an empty file `py.typed` (see [PEP-561](https://peps.python.org/pep-0561/#packaging-type-information))
in your Python package.

```bash
touch ai_web_app/py.typed
```

{{% callout note %}}
If you've been following these instructions, your code should look like this:
https://github.com/dcferreira/ai-web-app/tree/26dde85a68232db3ee84967098b872d54e8414fe
{{% /callout %}}

To continue this tutorial, go to [Part 4](/post/2023-03-04-ai-web-app).

For comments or questions, use the
[Reddit discussion](https://www.reddit.com/user/dlcferreira/comments/11mv2rl/making_and_deploying_an_ai_web_app_in_2023_part_3/)
or reach out to me [directly via email](mailto:daniel.ferreira.1@gmail.com).
