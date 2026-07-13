
# Create a Virtual Environment

```bash
conda create -n LLM_Python python=3.13.9 -y
```

# Activate a Virtual Environment 

```bash
conda activate LLM_Python
```

# Install a Virtual Environment

```bash
pip install -r requirements.txt
```

---

To create a Conda virtual environment and install all packages from a `requirements.txt` file, follow these steps.

### 6. Verify Installation

```bash
pip list
```

or

```bash
conda list
```

---

### 7. Export the Environment (Optional)

Conda environment:

```bash
conda env export > environment.yml
```

Pip packages:

```bash
pip freeze > requirements.txt
```

---



## Useful Conda Commands

### List environments

```bash
conda env list
```

or

```bash
conda info --envs
```

### Deactivate

```bash
conda deactivate
```

### Remove an environment

```bash
conda remove --name LLM_Python --all
```

### Update all packages

```bash
conda update --all
```

---

## If `requirements.txt` Specifies an Older Python Version

Create the environment with the matching version. For example, if it requires Python 3.10:

```bash
conda create -n LLM_Python python=3.10 -y
conda activate LLM_Python
pip install -r requirements.txt

``

This helps avoid dependency conflicts.

---
---

If you want to create a **Python virtual environment** (without Conda), use the built-in `venv` module.

### 1. Navigate to your project

```bash
cd path/to/your/project
```

Example:

```bash
cd D:\Projects\MyProject
```

### 2. Create the virtual environment

```bash
python -m venv .venv
```

or name it `venv`:

```bash
python -m venv venv
```

### 3. Activate the virtual environment

**Windows (Command Prompt)**

```cmd
.venv\Scripts\activate
```

or

```cmd
venv\Scripts\activate
```

**Windows (PowerShell)**

```powershell
.venv\Scripts\Activate.ps1
```

**Linux/macOS**

```bash
source .venv/bin/activate
```

### 4. Upgrade pip

```bash
python -m pip install --upgrade pip
```

### 5. Install dependencies

```bash
pip install -r requirements.txt
```

### 6. Verify the installation

```bash
python --version
pip list
```

### 7. Deactivate the environment

```bash
deactivate
```

## Complete Example

```bash
cd D:\Projects\MyProject

python -m venv .venv

.venv\Scripts\activate

python -m pip install --upgrade pip

pip install -r requirements.txt

python main.py
```

## If you have multiple Python versions installed

Use the specific interpreter:

```bash
py -3.11 -m venv .venv
```

or

```bash
python3.11 -m venv .venv
```

This creates an isolated environment using Python 3.11.

--