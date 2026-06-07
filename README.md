Project: "Mining Strategic Friction in Multi-Turn LLM Collaboration"

What you'll find

- `01_setup_verification.ipynb`, `02_task_pool_generation.ipynb`,
  `03_orchestrator_pilot.ipynb`, `04_analysis.ipynb` — core notebooks.
- `requirements.txt` — pinned Python dependencies used for verification.
- `Figures/` — generated, paper-ready figures included for convenience.

Quick reproduction (Colab or local)

1. Open the desired notebook in Google Colab (from Drive or GitHub), or
   open the notebook locally after cloning this repository.

2. (Optional Colab) Mount Google Drive to store runs and data:

```python
from google.colab import drive
drive.mount('/content/drive')
```

3. Set `PROJECT_ROOT` to the repository folder inside your Drive (or a working
   directory in Colab). Example (Drive):

```python
from pathlib import Path
PROJECT_ROOT = Path('/content/drive/MyDrive/DOC-ICDM')
```

If running locally, you can set `PROJECT_ROOT = Path('.')` or point to an
explicit path where you cloned this repository.

4. Install the required packages once per session (example):

```python
!pip install -r {PROJECT_ROOT}/requirements.txt
```

5. Provide API keys (temporary; do not store them in the repo). Example:

```python
import os
os.environ['OPENAI_API_KEY'] = 'sk-...'
os.environ['ANTHROPIC_API_KEY'] = 'api-...'
```

Notebooks reference provider secrets via a small `userdata` helper in Colab.
If needed, adapt the top cell to read keys from `os.environ` instead.

6. Recommended execution order

- `01_setup_verification.ipynb` — verify environment and dataset availability.
- `02_task_pool_generation.ipynb` — create task pools from benchmarks.
- `03_orchestrator_pilot.ipynb` — run the orchestration (requires API access).
- `04_analysis.ipynb` — load run outputs and generate figures/tables.

Setup Notes (important)

- Notebooks contain a few intentionally commented helper lines so the
  repository runs cleanly in both Colab and local environments. Before
  executing any notebook, confirm the two items below:
  1.  Drive mounting (Colab only): some notebooks include a commented
      `drive.mount('/content/drive')` line. This is commented to avoid
      forcing Drive on local runs. In Colab, uncomment and run the cell
      to mount your Drive, then set `PROJECT_ROOT` to the folder where
      you placed this repository in Drive. Example:

```python
from google.colab import drive
drive.mount('/content/drive')
from pathlib import Path
PROJECT_ROOT = Path('/content/drive/MyDrive/DOC-ICDM')
```

2.  `PROJECT_ROOT` placeholder: notebooks use a sanitized placeholder
    (e.g. `# (sanitized path) PROJECT_ROOT = Path("PROJECT_ROOT")`) to
    avoid committing local paths. Replace that placeholder with the
    actual repository path before running. Examples:

- Colab / Drive: `PROJECT_ROOT = Path('/content/drive/MyDrive/DOC-ICDM')`
- Local: `PROJECT_ROOT = Path('.')` (or an explicit absolute path)

- Secrets and API keys: do not commit keys. For Colab tests use
  temporary environment variables or `getpass()` for interactive entry.

- Verification step: run `01_setup_verification.ipynb` first; it checks
  that `PROJECT_ROOT` is set correctly and that required data files are
  available under `PROJECT_ROOT/`.

These notes ensure reproducible, publication-quality execution across
environments.
