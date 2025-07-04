---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.17.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

Stability
------------------

We're going to start simple here - let's run a local relaxation (optimize the unit cell and positions) using a pre-trained EquiformerV2-31M-OMAT24-MP-sAlex checkpoint. This checkpoint has a few fun properties
1. It's a relatively small (31M) parameter model
2. It was pre-trained on the OMat24 dataset, and then fine-tuned on the MPtrj and Alexandria datasets, so it should emit energies and forces that are consistent with the MP GGA (PBE/PBE+U) level of theory

:::{note} Need to install fairchem-core or get UMA access or getting permissions/401 errors?
:class: dropdown

```{include} ../../core/simplified_install.md
```
:::

```{code-cell} ipython3
from __future__ import annotations

import pprint

from ase.build import bulk
from ase.optimize import LBFGS
from quacc.recipes.mlp.core import relax_job

# Make an Atoms object of a bulk Cu structure
atoms = bulk("Cu")

# Run a structure relaxation
result = relax_job(
    atoms,
    method="fairchem",
    name_or_path="uma-s-1",
    task_name="omat",
    opt_params={"fmax": 1e-3, "optimizer": LBFGS},
)
```

```{code-cell} ipython3
pprint.pprint(result)
```

Congratulations; you ran your first relaxation using an OMat24-trained checkpoint and `quacc`!
