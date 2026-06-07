# REPRODUCIBILITY (Google Colab)

This release was executed and verified in Google Colab. The instructions
below describe the minimal steps to reproduce the pipeline and figures
using Google Colab. Do NOT commit API keys or secrets into the repository.

1. Open the notebooks in Colab

-- Option A (from Google Drive): Upload this repository folder into your
Google Drive and open the desired notebook using Colab's "Open file"
dialog.

- Option B (from GitHub): Use Colab's "Open with Google Colaboratory"
  feature on the repository or notebook file.

2. Mount your Google Drive (if using Drive storage)

```python
from google.colab import drive
drive.mount('/content/drive')
```

3. Set `PROJECT_ROOT`

Update the top cell in each notebook (or run a small cell) to point to
the repository location inside your Drive, for example:

```python
from pathlib import Path
PROJECT_ROOT = Path('/content/drive/MyDrive/DOC-ICDM')
```

Some notebooks include a sanitized placeholder `PROJECT_ROOT` and will
raise a clear error if the path is not set.

4. Install Python dependencies

Run this cell once in Colab to install the pinned dependencies:

```python
!pip install -r {PROJECT_ROOT}/requirements.txt
```

5. Set API keys / secrets

There are multiple ways to provide keys in Colab. Two simple options:

- Temporarily set environment variables in a cell (do not save them):

```python
import os
os.environ['OPENAI_API_KEY'] = 'sk-...'
os.environ['ANTHROPIC_API_KEY'] = 'api-...'
os.environ['GROQ_API_KEY'] = 'groq-...'
```

- Or enter keys interactively (recommended for short runs):

```python
from getpass import getpass
os.environ['OPENAI_API_KEY'] = getpass('OpenAI key: ')
```

Notebooks may reference provider secrets via a small `userdata` helper in
Colab; if so, set the corresponding key values as shown above or adapt
the notebook cell to read keys from `os.environ`.

6. Recommended notebook execution order

- `01_setup_verification.ipynb` — run first to validate environment and
  available benchmark files.
- `02_task_pool_generation.ipynb` — generates task pools (skip if you
  already have prepared pools under `PROJECT_ROOT/benchmarks`).
- `03_orchestrator_pilot.ipynb` — runs the orchestration/simulations and
  produces `runs/` JSONL outputs under `PROJECT_ROOT/runs/`.
- `04_analysis.ipynb` — loads run outputs and generates figures/tables.
