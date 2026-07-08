# new_project — Engineering Calc Template

A reusable template folder for engineering calculations. Notebooks are authored in Jupyter Notebook (run virtually via `uv`) and rendered to PDF via Quarto.

---

## Prerequisites

Install the following tools before using this template. Each only needs to be installed once per machine.

| Tool | Purpose | Install |
|---|---|---|
| [uv](https://docs.astral.sh/uv/getting-started/installation/) | Package & virtual environment manager | `winget install astral-sh.uv` |
| [Quarto](https://quarto.org/docs/get-started/) | Renders `.ipynb` → PDF | Download installer from quarto.org |
| [TinyTeX](https://quarto.org/docs/output-formats/pdf-engine.html) | LaTeX engine used by Quarto for PDF output | `quarto install tinytex` (run after installing Quarto) |
| [Git](https://git-scm.com/download/win) | Version control for syncing across machines | Download installer from git-scm.com |

> **Note:** Jupyter Notebook is managed by `uv` and installed automatically into the project's virtual environment when you run `uv sync`. No global Jupyter installation is needed.

> **Fonts:** The `_quarto.yml` uses **Calibri** and **Courier New**, which are bundled with Windows. No additional font installation needed.

---

## Setting Up a New Project Folder

1. Copy the three template files into a new blank folder:
   ```
   _quarto.yml
   notebook_01.ipynb
   pyproject.toml
   ```

2. Open a terminal in that folder. In Windows Explorer, click the address bar, type `cmd`, and press Enter.

3. Run:
   ```powershell
   uv sync
   ```
   This creates a `.venv` folder and installs `jupyter` and `ipykernel` (always-installed packages). Engineering packages remain commented out until needed.

4. To add engineering packages (e.g. `handcalcs`, `forallpeople`), uncomment the relevant lines in `pyproject.toml` under `[dependency-groups] dev`, then re-run `uv sync`. Only uncomment what the specific project needs — this keeps dependencies isolated per project folder.

> **Under the hood:** `uv` only auto-installs its built-in `dev` group on `uv sync`; every other group is skipped unless you name it. The `[tool.uv] default-groups = ["always", "dev"]` line at the top of `pyproject.toml` is what tells `uv` to also install the `always` group (Jupyter + ipykernel) on every sync. Without it, `uv sync` silently skips those packages and `uv run jupyter notebook` fails with `program not found`. If you ever add a new group that should always install, add its name to that list too.

---

## Running Jupyter Notebook

From the project folder in a terminal:

```powershell
uv run jupyter notebook
```

What this does:
- `uv run` — executes the command inside the project's `.venv`, using only the packages installed for this project
- `jupyter notebook` — opens the classic Jupyter Notebook UI in your browser

Open `notebook_01.ipynb` to begin editing calculations.

---

## Rendering to PDF with Quarto

From the project folder in a terminal:

```powershell
quarto render notebook_01.ipynb --to pdf
```

The rendered PDF will appear in the same directory as the notebook. Formatting (fonts, margins, heading sizes, figure settings) is controlled by `_quarto.yml`.

To preview before final render (re-renders automatically on each save):

```powershell
quarto preview notebook_01.ipynb
```

---

## GitHub Sync (Multi-Machine Workflow)

This section explains how to back up your template files to a private GitHub repository and keep them in sync across multiple machines.

Only the three template files are tracked. Generated files (PDFs, `.venv`, cache folders) are excluded via `.gitignore`.

---

### Step 1 — One-Time Git Setup (per machine)

After installing Git, tell it who you are. Open a terminal and run these two commands, substituting your own name and email:

```powershell
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

Also run this to standardize your default branch name to `master` (matching what Git on Windows typically creates):

```powershell
git config --global init.defaultBranch master
```

These only need to be done once per machine.

---

### Step 2 — Create the GitHub Repository (one time only)

1. Go to [github.com](https://github.com) and sign in (or create a free account).
2. Click the **+** icon in the top right → **New repository**.
3. Name it `new_project` (or whatever you prefer).
4. Set visibility to **Private**.
5. Leave all other options unchecked — do **not** initialize with a README or `.gitignore`, as you'll be pushing your own.
6. Click **Create repository**.
7. Copy the repository URL shown on the next page — it will look like:
   ```
   https://github.com/YOUR_USERNAME/new_project.git
   ```

---

### Step 3 — Initialize and Push from Your Main Machine

Open a terminal in your `new_project` folder and run each command in order:

```powershell
# Initialize a git repository in this folder
git init

# Tell git where your remote GitHub repo lives
git remote add origin https://github.com/YOUR_USERNAME/new_project.git

# Stage the files you want to track
git add _quarto.yml notebook_01.ipynb pyproject.toml .gitignore README.md

# Save a snapshot with a message describing what it is
git commit -m "Initial template commit"

# Push to GitHub
git push -u origin master
```

> **Note:** Git on Windows typically names your local branch `master` by default. The push command above uses `master` to match this. If you ever see the error `src refspec main does not match any`, it means Git used `master` — replace `main` with `master` in the push command and it will work.

> **Note:** During `git add` you may see warnings about `LF will be replaced by CRLF`. This is normal on Windows and can be safely ignored — it refers to line ending formatting and will not affect your files.

After this, your files are on GitHub. You can verify by visiting your repo URL in a browser.

---

### Step 4 — Cloning onto a New Machine

On any other machine where you want access to the template:

```powershell
# Download the repo into a local folder called new_project
git clone https://github.com/YOUR_USERNAME/new_project.git

# Navigate into it
cd new_project

# Install packages and set up the virtual environment
uv sync
```

---

### Step 5 — Daily Workflow

**Before starting work on any machine — pull the latest version first:**

```powershell
# Download any changes made on another machine
git pull
```

**After making changes — save and push:**

```powershell
# Stage all changed files
git add -A

# Save a snapshot with a short description of what changed
git commit -m "Brief description of changes"

# Upload to GitHub
git push
```

> **Tip:** Think of `git pull` as "download the latest" and `git push` as "upload my changes." Always pull before you start work, always push when you're done.

---

## Project Structure

```
new_project/
├── _quarto.yml         # Quarto PDF formatting config
├── notebook_01.ipynb   # Engineering calculation notebook
├── pyproject.toml      # uv dependency config (uncomment packages as needed)
├── .gitignore          # Excludes .venv, PDFs, cache, etc. from git
└── README.md           # This file
```
