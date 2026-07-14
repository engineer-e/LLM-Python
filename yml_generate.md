The usual practice is to **maintain both files**:

* `requirements.txt` → for `pip`/`venv` users.
* `environment.yml` → for Conda users.

You don't manually keep them in sync every time. You can generate one from the other or regenerate them from your environment.

## 1. Create `environment.yml` from an existing Conda environment

If you've already created and configured a Conda environment:

```bash
conda activate myenv
```

Export it:

```bash
conda env export > environment.yml
```

Or, without build numbers (recommended):

```bash
conda env export --no-builds > environment.yml
```

---

## 2. Create `requirements.txt` from the current environment

Using pip:

```bash
pip freeze > requirements.txt
```

Or, if you only want manually installed packages:

```bash
pip list --format=freeze > requirements.txt
```

---

## 3. Update `requirements.txt`

After installing a new package:

```bash
pip install transformers
```

Regenerate:

```bash
pip freeze > requirements.txt
```

---

## 4. Update `environment.yml`

After installing a new Conda package:

```bash
conda install -c conda-forge xeus-cling
```

Export again:

```bash
conda env export --no-builds > environment.yml
```

---

## 5. Recommended project structure

```
project/
│
├── requirements.txt
├── environment.yml
├── README.md
└── src/
```

---

## 6. Example

### requirements.txt

```text
torch>=2.2.2,<2.6
jupyterlab>=4.0
numpy>=1.26
pandas>=2.2.1
matplotlib>=3.7.1
tqdm>=4.66.1
psutil>=5.9.5
tensorflow>=2.18.0
tiktoken>=0.5.1
```

### environment.yml

```yaml
name: myenv

channels:
  - conda-forge

dependencies:
  - python=3.12
  - jupyterlab>=4.0
  - xeus-cling
  - cmake
  - ninja
  - pip

  - pip:
      - torch>=2.2.2,<2.6
      - numpy>=1.26
      - pandas>=2.2.1
      - matplotlib>=3.7.1
      - tqdm>=4.66.1
      - psutil>=5.9.5
      - tensorflow>=2.18.0
      - tiktoken>=0.5.1
```

---

## 7. Automatically generate `environment.yml` from `requirements.txt`

If you already have a `requirements.txt`, you can use this simple Python script:

```python
from pathlib import Path

requirements = Path("requirements.txt").read_text().splitlines()

with open("environment.yml", "w") as f:
    f.write("name: myenv\n")
    f.write("channels:\n")
    f.write("  - conda-forge\n")
    f.write("dependencies:\n")
    f.write("  - python=3.12\n")
    f.write("  - pip\n")
    f.write("  - pip:\n")

    for line in requirements:
        line = line.strip()
        if not line or line.startswith("#"):
            continue
        f.write(f"      - {line}\n")
```

This copies all pip-installable packages from `requirements.txt` into the `pip:` section of `environment.yml`. You can then manually add Conda-only packages such as `xeus-cling`, `cmake`, or `ninja` to the `dependencies:` section. This approach avoids duplicating your pip package list by hand.
