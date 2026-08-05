<h1 align="center">Shinobi Dosho</h1>

<p align="center">
  <em> Shinobi and their tools.</em>
</p>

<p align="center">
  Reproducible radio-astronomy pipelines, and the libraries they stand on.
</p>

---

## What this is

A pipeline stack for radio interferometry, built around one idea: **robust and
flexible simplicity**.

Recipes are plain Python. A step is a function call; a step's output is a Python
value you wire into the next call. No YAML expression language, no alias
propagation, no stacked config libraries — control flow is just Python, and it
doesn't need reinventing.

```python
from pydantic import BaseModel
from shinobi import Cab, step


class ImageInputs(BaseModel):
    ms: str = "obs.ms"
    prefix: str = "img"


wsclean = Cab(
    name="wsclean",
    command="wsclean",
    image="quay.io/stimela/wsclean:latest",
    inputs_model=ImageInputs,
)


@step(wsclean, backend="native")
def image(ctx):
    """Image the visibilities."""
    return ctx.run()
```

Run it straight from the command line — the step's schema *is* the CLI, no
entrypoint script required:

```console
$ ninja run myrecipe.py:image --ms data.ms --prefix out
$ ninja run myrecipe.py:selfcal --ms data.ms --dryrun   # render the graph, run nothing
```

Steps execute on pluggable backends — `native`, `docker`/`podman`/`apptainer`,
`slurm`, `kubernetes` — all shelling out to the relevant CLI rather than a
Python SDK.

## The stack

| | | |
| :--- |-----|---|
| **[stimela&#8209;ninja](https://github.com/shinobi-dosho/stimela-ninja)** | Stimela 3.0: the framework — cabs, steps, recipes, backends, the `ninja` CLI | [docs](https://stimela-ninja.readthedocs.io)  [pypi](https://pypi.org/project/stimela-ninja/) |
| **[dosho](https://github.com/shinobi-dosho/dosho)** | The native cab library: ~100 tools ready to drop into a recipe — WSClean, CubiCal, QuartiCal, AOFlagger, Tricolour, CASA tasks, and more | [docs](https://dosho.readthedocs.io)  [pypi](https://pypi.org/project/dosho/) |

## The libraries

Each stands alone as a Python API and a command line — and doubles as pipeline
steps.

| | | |
|---|---|---|
| **[msutils](https://github.com/shinobi-dosho/msutils)** | Everyday Measurement Set operations: inspect, subset, average, manage columns and flags | [docs](https://msutils.readthedocs.io)  [pypi](https://pypi.org/project/msutils/) |
| **[fitstoolz](https://github.com/shinobi-dosho/fitstoolz)** | FITS data with *named* axes — WCS read once, so celestial, spectral and Stokes axes work the same way in the API and on the CLI. Xarray and Zarr supported | [docs](https://fitstoolz.readthedocs.io)  [pypi](https://pypi.org/project/fitstoolz/) |
| **[simms](https://github.com/shinobi-dosho/simms)** | Simulate interferometric arrays and the visibilities they would record, then simulate sky models into them | [docs](https://simms.readthedocs.io)  [pypi](https://pypi.org/project/simms/) |

## Getting started

```bash
pip install stimela-ninja     # the framework: `ninja` CLI + the `shinobi` package
pip install dosho[run]        # the cab library, plus the framework to run it
```

> **Note** — the framework installs from **`stimela-ninja`** but imports as
> `shinobi`. The unrelated `shinobi` package on PyPI is not this project.

## Status

Early. The interfaces above are real and tested, but `stimela-ninja` and
`dosho` are pre-1.0 and not yet ready to carry production pipelines —
follow the repos for progress. `msutils`, `fitstoolz` and `simms` are usable
today, on their own or inside a recipe.

Issues and pull requests are welcome on any repository. Everything here is
open source under MIT, GPL-2.0 or GPL-3.0 — see each repository for its terms.
