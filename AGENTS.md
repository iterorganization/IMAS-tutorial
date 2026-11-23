# AGENTS.md

Guidance for AI coding assistants working in this repository.

## What this repo is

A tutorial for newcomers to IMAS (ITER Integrated Modelling and Analysis Suite), made of
Jupyter notebooks in `notebooks/`, meant to be followed in order. There is no package, build, linter
or test suite: the notebooks are the product. They cover only open-source IMAS components and are
designed to run on **Google Colab** (the README badge opens `00_Start_Here.ipynb` there), not on a
pre-configured IMAS environment.

The notebooks are also rendered, executed, on the IMAS documentation hub (repo
`olivhoenen/IMAS-docs`), which copies `notebooks/*.ipynb` at build time and runs them with
myst-nb (all except `05_SimDB`). So every notebook must run top to bottom in a plain Jupyter kernel,
not only in Colab, and links between notebooks must resolve there too.

## Notebook conventions

- **Naming / ordering**: `NN_Topic.ipynb`, where `NN` is the section number. Several notebooks may
  share a number when they are alternatives within one section (e.g. `03_IMAS_Core` and
  `03_IMAS_Python` under "Data access"). When adding, renaming or reordering a notebook, update the
  table of contents in `README.md` and the cross-links between notebooks (markdown links such as
  `[IMAS-Python](03_IMAS_Python.ipynb)`).
- **Self-contained**: each notebook installs its own dependencies from PyPI in its first code cell
  (`%pip install imas-data-dictionary`, `imas-core`, `imas-python[netcdf]`, `imas-idstools`,
  `imas-simdb`; use `%pip`, not `!pip`, so packages land in the running kernel) and works in the current directory with relative paths (`./testh5`,
  `./mycoreprofiles.nc`, `imas:hdf5?path=./mytestpulse`). Don't rely on state from another notebook.
  Note that the SimDB client package is `imas-simdb`; the PyPI package `simdb` is unrelated.
- **Commit notebooks with outputs cleared** (all committed notebooks have zero outputs). Clear them
  before committing, e.g. `jupyter nbconvert --clear-output --inplace notebooks/<file>.ipynb`.
- **Don't commit downloaded or generated data**. `04_IDStools.ipynb` downloads IDS data from Zenodo
  (`iter_scenario_53298_seq1_DD4.nc`, `iter_md_wall_116000_5.nc`) and a SPARC EQDSK file; running
  notebooks also creates HDF5/netCDF data entries and `sim_folder/`. These end up untracked in
  `notebooks/` and should stay out of git.
- Shell commands go through `!cmd`; IDStools is demonstrated via its IPython magics
  (`%idslist`, `%idsprint`, `%eqdsk2ids`, `%plotequilibrium`, `%plotmachinedescription`), which
  are registered by `import idstools`.
- Links to a section of another notebook use Jupyter-style anchors, i.e. the heading text with
  spaces replaced by `-` and case kept: `02_IMAS_Data_Dictionary.ipynb#Alternate-package-with-all-versions`.
  The anchor must match an existing heading. Link to files outside `notebooks/` with absolute
  GitHub URLs, not relative paths.

## Running notebooks outside Colab

- `04_IDStools.ipynb` enables the Colab widget manager inside a `try/except ImportError`, so it
  runs elsewhere too. Keep Colab-only code guarded this way.
- `05_SimDB.ipynb` talks to the ITER SimDB server (`https://simdb.iter.org/scenarios/api/`), which
  requires ITER authentication; its remote cells cannot be validated without credentials.
- `03_IMAS_Core.ipynb` deliberately shows that the PyPI `imas-core` wheel has no MDSplus backend
  (the `imas:mdsplus` cell logs an error and returns a non-zero status, without raising).

## Contributing

Per `CONTRIBUTING.md`: open or find an issue first, then work on a fork with a feature branch and
open a pull request. Contributors must follow `CODE_OF_CONDUCT.md`.
