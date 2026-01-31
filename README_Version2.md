# CodeRefiner

This Generative AI tool analyzes source code to identify bugs, performance issues, and best-practice violations. It provides optimized code rewrites, helping developers improve quality and accelerate development cycles.

## Important: ngrok API key required
CodeRefiner requires an ngrok API key to run. The project uses ngrok for exposing local services (for example, a web UI or webhook endpoints). The code will run if and only if a valid ngrok API key is provided. Without the ngrok API key, the notebooks/scripts that depend on ngrok will fail to start or will not expose the required endpoints.

- Environment variable: NGROK_API_KEY
- Do not hard-code your key in notebooks. Use secure entry methods (see Colab steps below).

## Features
- Static and heuristic analysis to find potential bugs and anti-patterns
- Suggestions for performance improvements and best-practice changes
- Automatic code rewrite proposals (suggested patches)
- Notebook-based workflows designed for experimentation and reproducibility

## Repository structure (expected)
- *.ipynb* — Jupyter Notebooks containing analysis, examples, and demos
- `notebooks/` — Example notebooks and step-by-step workflows (if present)
- `requirements.txt` — Python dependency list (if present)
- `README.md` — This file

(If your repo layout differs, adjust the paths above or add a `notebooks/` directory for demo notebooks.)

## Quick start — Running in Google Colab

There are two straightforward ways to run CodeRefiner in Google Colab:

A. Open a notebook directly from GitHub (recommended for quick experiments)  
B. Clone the repository inside Colab and run notebooks or scripts (recommended if you want to modify files or persist outputs to Drive)

### A — Open a notebook from GitHub in Colab (fast)
1. Open Google Colab: https://colab.research.google.com  
2. File → Open notebook → GitHub tab  
3. Paste the repo URL: `https://github.com/Hrithik-balaji/CodeRefiner`  
   - If you want to open a notebook at a specific commit, use the commit-based URL like:
     `https://github.com/Hrithik-balaji/CodeRefiner/blob/6fb15518d4f58eda0c3a28f71582edbdac759542/path/to/notebook.ipynb`
   - Colab will show available notebooks in the repo; select the notebook you want and click "Open".
4. In Colab, set Runtime → Change runtime type → GPU (if the notebook benefits from GPU acceleration)
5. BEFORE running ngrok-dependent cells: securely provide your ngrok API key as described below (if not set, ngrok features will fail).
6. Run the notebook cells. If the notebook expects dependencies, follow the first cells to install them (or see Option B below).

Tip: If you want a one-click link to open a specific notebook in Colab, use:
`https://colab.research.google.com/github/Hrithik-balaji/CodeRefiner/blob/main/notebooks/example.ipynb`
(replace path/branch with the correct notebook path and branch/commit).

### B — Clone the repository inside Colab (more control)
1. Open a new Colab notebook.
2. In the first code cell, run:

```python
# Clone the repository and switch to the project directory
!git clone https://github.com/Hrithik-balaji/CodeRefiner.git
%cd CodeRefiner
# (Optional) checkout the specific commit used as reference
!git checkout 6fb15518d4f58eda0c3a28f71582edbdac759542
```

3. Install dependencies:
- If the repository contains a `requirements.txt` file, install it:
```bash
!pip install -r requirements.txt
```
- If there is no `requirements.txt`, install common packages the notebooks might need (adjust as necessary):
```bash
!pip install -U pip
!pip install jupyterlab notebook ipywidgets
# Example ML / NLP libs — only install what you need:
!pip install torch transformers sentence-transformers pyngrok
```

4. Provide the ngrok API key securely (required)
- The code will run only when the ngrok API key is provided. Use secure entry to avoid leaking the key in output.
- Recommended in Colab:

```python
from getpass import getpass
import os
os.environ["NGROK_API_KEY"] = getpass("Enter your ngrok API key (keeps it hidden): ")
```

- Example to configure pyngrok (if the project uses pyngrok):
```python
from pyngrok import ngrok
ngrok.set_auth_token(os.environ["NGROK_API_KEY"])
# open a tunnel if needed, e.g.:
# public_url = ngrok.connect(7860)  # adjust port
# print("ngrok public URL:", public_url)
```

- Alternatively, if using the ngrok CLI and it is installed, you can run:
```bash
!ngrok authtoken YOUR_NGROK_API_KEY  # avoid echoing the key in notebook output
```
(Prefer using getpass and environment variables to avoid printing the key.)

5. (Optional) Mount Google Drive to persist outputs or models:
```python
from google.colab import drive
drive.mount('/content/drive')
# Example path: /content/drive/MyDrive/CodeRefiner-output
```

6. Open and run a notebook:
- Use the Colab file browser (left panel) and open the .ipynb files from `/content/CodeRefiner` or
- Run scripts/notebooks after setting up the ngrok key and dependencies.

7. If the notebooks expect other API keys (e.g., LLM providers), set them similarly and securely (use getpass or Colab environment variables).

### Minimal example cell to run analysis (adapt to the repository's notebook contents)
```python
# Example: run the main analysis script or notebook cell logic
# Replace 'analyze.py' or notebook steps with actual files in this repo
!python analyze.py --input example_project/ --output results/
```

## Usage tips and troubleshooting
- If you see package version conflicts, try creating an isolated virtual environment locally, or pin exact versions in `requirements.txt`.
- If a notebook is long-running or requires a lot of RAM/VRAM, choose a Colab Pro/Pro+ runtime for longer runtimes and more memory / GPU.
- If models are large, download them to Google Drive and load from there to avoid repeated downloads.
- To avoid exposing API keys in notebook output, collect them with `getpass()` or set them in Colab’s environment variables without printing them.
- Remember: ngrok-dependent functionality will not work without a valid ngrok API key set in `NGROK_API_KEY`.

## Contributing
- Add issues or feature requests to the GitHub issues page.
- Suggested workflow:
  1. Fork the repo
  2. Create a feature branch
  3. Add notebooks or scripts and update `README.md` or `requirements.txt`
  4. Open a pull request with a clear description and example usage

## License
Specify your project license here (e.g., MIT, Apache-2.0). If you don't have one yet, add a `LICENSE` file.

## Contact
For questions or help, open an issue in this repository or contact the maintainer.