# ADAPT: A Data Analysis Project Template

This is a lightweight and structured template for data analysis projects. The
goal is to keep work organized, reproducible, and easy to pick up or hand off.
We built this template with small-project medical data analysis in mind, but
serves other domains and larger projects as well.

Feedback and suggestions for improvement are welcome. Please create an issue on GitHub.

## Getting this template

### Option 1: Create a repository from this template (recommended)

Follow the [instructions on
GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
to create your own repository from this template. Then, clone your own
repository to your computer to start working on it. There are several ways to
clone a repository, e.g., via [Visual Studio
Code](https://learn.microsoft.com/en-us/azure/developer/javascript/how-to/with-visual-studio-code/clone-github-repository?tabs=activity-bar),
[Git
Tower](https://www.git-tower.com/help/guides/manage-repositories/clone-remote-repository/windows)
or through [GitHub
itself](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository?tool=webui). 

### Option 2: Download the files

Download the latest version of this template from ... Extract as your project
folder.

<!-- TODO: add link to repo -->


## Create a virtual environment and install your project

### Option 1: `conda` (recommended)

1. Create a virtual environment

   ```bash
   conda create --prefix "./.venv" --python 3.13
   ```

   This template requires Python 3.11 or higher.

   Alternatively, you can create a named conda virtual environment. See [the
   documentation](https://docs.conda.io/projects/conda/en/latest/commands/create.html)
   for details. 

2. Activate virtual environment

   ```bash
   conda activate ./.venv
   ```

3. Install project as editable package.

    **NB: Before installing the project, set the project name. See [pyproject.toml](#pyprojecttoml).**

   ```bash
   pip install -e .
   ```

### Option 2: `uv` (advanced users)

1. Create a virtual environment and install project as package.

   **NB: Before installing the project, set the project name. See [pyproject.toml](#pyprojecttoml).**

   ```bash
   uv sync
   ```

2. Run your code.

```bash
uv python analysis/example_script.py
```


## Directory structure

```text
├── .vscode/                # Editor configuration
├── analysis/               # Scripts and notebooks for analysis
├── config/                 # Configuration files (TOML format)
│   ├── .secrets.toml       # API keys, credentials (gitignored)
│   ├── settings.toml       # Base configuration
│   └── settings.local.toml # User-specific overrides
├── data/
│   ├── annotations/        # Processing annotations
│   ├── interim/            # Intermediate processing results
│   ├── processed/          # Final data for analysis
│   └── source/             # Raw data from experiments or instruments
├── docs/                   # Documentation
├── output/
│   ├── figures/            # Saved plots and images
│   ├── reports/            # Generated reports (PDFs, etc.)
│   └── tables/             # Saved tables (CSV, Excel, etc.)
├── references/             # Papers, links, background materials
├── src/                    # Reusable Python modules
│   ├── __version__.py      # Project version
│   └── config.py           # Code to load settings with interpolation
├── pyproject.toml          # Project metadata and dependencies
├── README.md               # Project description
└── TEMPLATE.md             # This file
```

**Note**: Directories in this template that should otherwise be empty contain a
.gitkeep file. These files only exist to have git clone the directories. You can
remove these files after your repository is cloned. 

All data should go in the `data/` folder. `data/source/` only contains source
data. Source data should be read-only. `data/annotations/` should contain extra
information about the source data. For example:

- `data/annotations/subjects.csv` is a list of subject numbers with data file
  names to be included in the analysis
- `data/annotations/selection/selection-{subject_id}.txt` contains the time range to be
  selected from the data of the specific subject

Script and notebook files should go in the `analysis/` directory. Is is useful
to number them in order of logical operation. E.g.:

- `analysis/01-convert_data.py` converts source data to a more workable format
  in `data/interim/01-converted/`
- `analysis/02-filter_data.py` filters data and stores it in
  `data/interim/02-filtered/`
- `analysis/02a-remove_invalid_data.py` (was added after `03-...`)
- `analysis/03-select_features.py` automatically selects data to create the
  final version of the data in `data/processed/`
- `analysis/04-calculate_something.py` generates output data

Reusable should go in the `src/` directory. As a rule: never import code from a
file in `analysis/`, but import code from modules in `src/`. The package has
been set up such that any module inside `src/` can be directly imported. E.g.: 

- `src/helper.py` can be imported using `import helper` 
- `src/plotting/plot_dataset.py` can be imported using `from plotting import
plot_dataset.py` (or `import plotting` and `plotting.plot_dataset(...)`). 

Any output should go in the `output/` directory. It is useful to include in the
file name the number of the analysis file, then a number for subsequent outputs
from the same file, and a good description. E.g.:

- `outputs/figures/02-01-spectrogram-{subject_id}.png` for a spectrogram created
  in `02-filter_data.py`
- `outputs/figures/02a-02-filter-results-{subject_id}.png` for a plot of the
  filtered signal created in `02-filter_data.py`
- `outputs/figures/02a-01-removed_sections-{subject_id}.png` for a plot of
  removed sections from `02a-remove_invalid_data.py`
- `outputs/reports/04-01-feature-analysis-{subject_id}.pdf` for a PDF including
  figures and tables created by `04-calculate_something.py`

If there are many subjects, it might be convenient to put outputs in respective
subdirectories. Always keep the full filename, in case you collect them per
subject.

- `outputs/figures/02-01-spectrogram/02-01-spectrogram-{subject_id}.png`

Optionally, you can add the version number of your project to the output file
name (`02-01-spectrogram-{subject_id}-{version}.png`). The version number can be
gotten with `from __version__ import __version__`. This enables comparing
results between different code versions, and prevents combining up-to-date and
older results.

Documentation and instructions about the analysis process should go in `docs/`.
This should at a minimum contain information on how to run the analysis. It
should also contain a description of how the source data and annotations should
be formatted. As a rule: if you give this project — minus the data — to someone
else, can they reproduce your analysis with their own dataset?

Reference files, e.g., scientific publications explaining your methods, should
be included in `references/`. Code comments or documentation should refer to
these references when appropriate.

## `pyproject.toml`

The pyproject file contains information about your project. You should give the
project a suitable name, description and author(s) information. The version of
the project is stored in the file `src/__version__.py`. 

Project dependencies should be listed in `dependencies`. The only template
dependency is [OmegaConf](https://omegaconf.readthedocs.io/en/2.3_branch/) for
configuration management. After changing dependencies, update your installation
using `pip install -e .` or `uv sync`.

Documentation on `pyproject.toml` can be found [here](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/).

## Configuration

All configuration is done via TOML files in the config/ folder:
 - `settings.toml`: shared project-wide settings
 - `settings.local.toml`: personal overrides (e.g., paths, overrides)
 - `"${...}"` syntax is supported for interpolation (via
   [OmegaConf](https://omegaconf.readthedocs.io/en/2.3_branch/))

`settings.toml` should contain all settings that are shared between computers.
However, settings that are specific to the computer the code runs on (likely only
the source path), should be in `settings.local.toml`. 

Settings should always be preferred over hard-coded values. This makes it easy to
find the parameters used to perform your analysis, but also prevents values
from being defined more than once, resulting in unexpected behavior.

**Example `settings.toml`**:
```toml
[filtering]
method = "lowpass"
cuttof = 25  # Hz
order = 4
```

**Example `settings.local.toml`**:
```toml
[paths.data]
source = "/path/to/server"  # overrides setting in settings.toml
```

Load configuration settings in Python:

```python
from config import settings

print(settings.filtering.order)
```

## Best practices

- Write a README.md (replacing this file) with a short explanation of the repository: what is the
  goal, and how can people use it? For simple projects, this could suffice as
  documentation. For more complex projects, it should refer to the
  documentation. 
- All interim and processed data should be derived from source data and
annotations. This derivation should be reproducible, such that interim and
processed files can be removed without losing information.
- Files in `output/` should be human-readable. Tables used for further analysis,
e.g., a pandas data frame, should be stored as interim data.
- Use git for source version control of your code. Regularly make atomic
  commits. Update the version in `src/__version__.py` and create a git tag at
  the corresponding commit whenever the code has reached a next milestone or
  stable state. Add the version of the project to the name of output
  files.[^gitpython] Data and output do not belong in a git repository. 
- Analysis workflows often have two stages: first process and analysis data from
  individual measurements or subjects, then combine data for a final comparison.
  Avoid looping over measurements or subjects for complex analysis workflows in
  long script files. The best way to prevent this is by creating functions that
  do all the work. A script file then can loop over subjects and call the
  funtion for all subjects. However, this is not always a good option,
  especially in the early phases of analysis. A secondary option is to have a
  script file that performs analysis on a single subject. 

  **Example `02-convert_data.py`**:
  ```python
  # subject_id can be set by a calling script (see below)
  # if not, i.e., you're running this file seperately, the value is set to "S004"
  
  subject_id = globals().get("subject_id", "S004")
  
  # <perform a lot of complicated steps>
  ```

  While developing this workflow, you can run this analysis for a single
  subject. Then, have a secondary file that runs `02-convert_data.py` for all
  subjects.

  **Example `data/annotations/subjects.csv`**:
  ```text
  S001,S003,S004,S005,S010
  ```

  **Example `02a-run-convert_data.py`**:
  ```python
  import csv
  from config import settings

  with (settings.paths.data.annotations / "subjects.csv").open("r") as csvfile:
      # Get the ','-delimited values from the first line
      subjects = next(csv.reader(csvfile, delimiter=","))

  for subject_id in subjects:
      exec(open("analysis/02-convert_data.py").read(), {"subject_id": subject_id})
  ```

  Note that using `exec` is generally not recommended. In this case, it calls
  one of your own files, so it should be safe. Use with caution.

[^gitpython]: Alternatively, you can add the first 7 characters of the current git commit
  hash, removing the need to change the version often. You can get
  the commit hash using `gitpython`. Install `gitpython` using `pip` or `uv`. 

    ```python
    import gitpython
    import os
    repo = Repo(os.getcwd())
    short_commit_sha = repo.head.commit.hexsha[:7]
    ```

    These are random, so results are not chronological if ordered by name. With a
    bit more work, you can add a commit count, resulting in a chronological order
    of files. 

    ```python
    n_commits = len(tuple(repo.iter_commits()))  # get the number of commits
    file_name = f"02-01-spectrogram-{subject_id}-{n_commits:03d}-{short_commit_sha}.png"
    ```
