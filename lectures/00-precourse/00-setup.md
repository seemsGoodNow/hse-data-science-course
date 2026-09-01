# Set up your tools

Reading this before the course starts? Good, follow it as written and you will be ready on day one. I
also walk through these same steps live in the first demo, so you can come back afterwards to redo
anything slowly or to catch a step you missed. Nothing here is graded and nothing here is hard, but a
working environment is the one thing the rest of the course assumes.

**You are done when you can do these three things:**

1. run a notebook and see the result of a cell;
2. open the course files, including the warehouse data;
3. have an AI assistant open in a browser tab.

There are two ways to get there. **Path A (Colab)** takes five minutes and installs nothing. **Path B
(your own laptop)** takes about twenty minutes and is what I use for real work. Colab always counts in
this course. If you have no preference, do Path A first and Path B when you have a calm hour.

---

## Path A — Google Colab, nothing to install

Colab is Jupyter running on Google's computer. You need a Google account.

1. Go to [colab.research.google.com](https://colab.research.google.com) and sign in.
2. **File → Open notebook → GitHub**. Paste the course repository address:
   `https://github.com/seemsGoodNow/hse-data-science-course` and press Enter.
3. Pick `lectures/00-precourse/python-primer.ipynb` from the list.
4. Click the first code cell and press **Shift+Enter**. Colab warns that the notebook was not written by
   Google. Click **Run anyway**.
5. **File → Save a copy in Drive.** Do this before you type anything of your own. Without it, your edits
   live only in the browser tab and disappear when you close it. The copy lands in your Drive under
   `Colab Notebooks`.

That is the whole setup. The primer needs no data files, so you can work through it right away.

### Getting the course data in Colab

Notebooks that read warehouse data need the course files next to them. Colab starts with an empty
machine, so the notebooks have a ready line for that in the setup cell. Remove the `#` in front of the
two marked lines and run the cell:

```python
!git clone https://github.com/seemsGoodNow/hse-data-science-course.git course
DATA = Path("course/data/alpha")
```

The `!` at the start means "this is a terminal command, not Python". The clone copies the course files
onto the Colab machine and takes a few seconds.

One thing to know: **Colab throws that machine away** after about 90 minutes of inactivity. Your saved
notebook is safe in Drive, but the downloaded files are not. When the data disappears, run the clone
line again.

---

## Path B — your own laptop

### B1. Install Python

Download the latest version from [python.org/downloads](https://www.python.org/downloads/) (anything
3.11 or newer is fine) and run the installer.

**On Windows this one checkbox matters:** at the bottom of the first installer screen, tick **Add
python.exe to PATH** before clicking Install. If you skip it, your terminal will not find Python and
you will get the most common error on this page.

On macOS, run the installer and accept the defaults.

If a terminal window was already open while you installed, close it and open a new one. A terminal
reads these settings once, at startup.

### B2. Open a terminal

- **Windows:** type `PowerShell` in search and open it.
- **macOS:** type `Terminal` in search and open it.

A terminal is a window where you type a command and the computer does it. That is all it is. You will
need it about five times this whole course.

### B3. Check that Python is there

The command has a different name on each system. Type the one for yours and press Enter.

**Windows:**

```bash
python --version
```

**macOS:**

```bash
python3 --version
```

You should see something like `Python 3.13.1`. On macOS, use `python3` and `pip3` everywhere below,
starting with the `pip install` in the next step.

### B4. Install the libraries

```bash
pip install pandas pyarrow matplotlib jupyter
```

This downloads and installs four packages. `pandas` is the table library the whole course runs on,
`pyarrow` lets pandas read our data files, `matplotlib` draws charts, `jupyter` runs notebooks. Expect a
lot of text and about a minute of waiting. Warnings in yellow are normal; a red `ERROR` is not.

Later sessions need a few more packages, and you never have to guess which. Every notebook's setup
cell carries a commented `%pip install` line naming exactly what that lecture needs, so uncomment it
and run it when you open that notebook.

Check it worked:

```bash
pip list
```

`pandas` should be in the list.

### B5. Install VS Code

Download it from [code.visualstudio.com](https://code.visualstudio.com/download), install, open.

Then install two extensions. Click the squares icon in the left bar (Extensions), search for each name,
click Install:

- **Python** (by Microsoft)
- **Jupyter** (by Microsoft)

### B6. Get the course files

Two ways, pick one.

**Without git (simplest):** open `https://github.com/seemsGoodNow/hse-data-science-course` in a
browser, click the green **Code** button, choose **Download ZIP**, and unpack it somewhere you will
find again. Downloads is not somewhere you will find again. Make a folder like `Documents/ds-course`.

**With git:** install [git](https://git-scm.com/downloads), then in the terminal:

```bash
git clone https://github.com/seemsGoodNow/hse-data-science-course.git
```

Git is worth it later, because one command gets you every update instead of a fresh ZIP. Not for the
first week.

### B7. Open the notebook

1. In VS Code: **File → Open Folder**, choose the course folder you just unpacked.
2. In the file tree on the left, open `lectures/01-intro/02-first-look.ipynb`.
3. Top right of the notebook, click **Select Kernel → Python Environments** and pick the Python version
   you installed in B1. This tells the notebook which Python to run.
4. Click the first cell, press **Shift+Enter**.

If a table of warehouse events appears a few cells later, you are set up.

---

## The terminal commands you actually need

| Command | What it does |
|---|---|
| `python --version` | prints the Python version, and proves the terminal can find it |
| `pip install pandas` | installs a library (any library, same shape of command) |
| `pip list` | lists what is installed |
| `cd Documents/ds-course` | moves into a folder |
| `ls` (macOS) / `dir` (Windows) | lists what is in the current folder |

Two habits that save time: pressing **Tab** completes a folder name you started typing, and pressing
**Up arrow** brings back the previous command.

## How a notebook actually runs

A notebook is a list of cells. Text cells are notes. Code cells run Python and print the result
underneath.

- **Shift+Enter** runs the current cell and moves to the next one.
- Cells run **in the order you press them**, not in the order they are written on screen. This is the
  single most confusing thing about notebooks at first.
- The number in `[ ]` on the left of a cell is its run order. `[*]` means the cell is still running.
- Everything a cell creates stays in memory, so a variable defined in cell 3 is available in cell 10.
  It is also still there after you delete cell 3, which is how notebooks start lying to you.
- When results stop making sense, restart the kernel and run every cell again. In VS Code: click
  **Restart** in the notebook toolbar, then **Run All**. In Colab: **Runtime → Restart and run all**.
  It clears memory and runs everything top to bottom, in order.

The word **kernel** just means the Python process attached to your notebook, the thing that holds those
variables.

## When something breaks

Almost every first-week problem is in this table.

| What you see | What it means | What to do |
|---|---|---|
| `'python' is not recognized...` (Windows) | Python is installed, but the terminal cannot find it | Run the installer again and choose **Modify**. Click **Next** past Optional Features, and on the Advanced Options screen tick **Add Python to environment variables**. Then close the terminal and open a new one |
| Typing `python` opens the Microsoft Store | You hit the Windows placeholder instead of real Python | Install from python.org with the PATH box ticked, or type `py` instead of `python` |
| `'pip' is not recognized` (Windows) | The same PATH problem, this time for pip | Type `py -m pip install pandas pyarrow matplotlib jupyter`. The `py -m` prefix works whenever `py` works. Or fix PATH as in the first row |
| `command not found: python` (macOS) | macOS calls it `python3` | Use `python3` and `pip3` |
| macOS offers to install the **command line developer tools** | The python.org install did not land, so macOS is offering its own stub instead | Close the popup, run the python.org installer again, then open a new terminal |
| `ModuleNotFoundError: No module named 'pandas'` | The library is missing from the Python your notebook uses | Run `pip install pandas` in the terminal. In VS Code also check the kernel in the top right is the Python you installed |
| `ModuleNotFoundError: No module named 'pyarrow'` | pandas needs this helper to read our data files | `pip install pyarrow` |
| `FileNotFoundError: ...picking_events.parquet` | The notebook is looking for the data in the wrong folder | Run `print(Path.cwd())` in a cell to see where it is looking. In Colab, run the `git clone` line first |
| `NameError: name 'events' is not defined` | The cell that creates `events` has not run in this session | Restart and run all, from the top |
| VS Code shows **Select Kernel** and nothing runs | No Python is attached to the notebook yet | Click it, choose Python Environments, pick your Python |
| In Colab the data files are gone | Colab discarded the machine after idle time | Run the `git clone` cell again |

Not in the table? Ask your AI assistant before you ask me. Paste the **full error text** into the
chat, not a description of it, and add one line of context: what you ran, on which system, and what
you expected. Copy the red block from the bottom up, because the last line names the actual error.
Section 10 of [the Python primer](python-primer.ipynb), "Stuck? How to ask an AI properly", shows the
shape of a good request.

If the AI's fix does not work after two tries, write me. Send the same error text plus what the AI
suggested and what you already tried.

## Get an AI assistant

We use one from the first session, so open an account before the course starts. Any of these work:

- [DeepSeek](https://chat.deepseek.com), [Qwen](https://chat.qwen.ai), [GLM](https://chat.z.ai) all have
  free web chat and handle course-level Python well. Start here.
- [ChatGPT](https://chatgpt.com) and [Claude](https://claude.ai) are what I demo in class.

Ask in English, and ask the way the primer's last section shows: who you are, what you have, what you
want, and the full error if something failed. A good request is a well-posed question, which is the
skill we are practising anyway.

## Words you will hear

| Word | Plain meaning |
|---|---|
| terminal | a window where you type commands to the computer |
| library (package) | someone else's code you install once and then use |
| pandas | the library that gives Python tables |
| notebook | a document of text and runnable code cells |
| cell | one block in a notebook, text or code |
| kernel | the Python process running behind your notebook |
| Colab | Jupyter in a browser, running on Google's machine |
| repository (repo) | a project folder stored on GitHub, ours holds all course materials |
| path | the address of a file, for example `data/alpha/picking_events.parquet` |
| parquet | a file format for tables, smaller and faster than CSV |
| DataFrame | one table inside pandas |

## Next

- Work through [the Python primer](python-primer.ipynb). It takes one to two hours and
  the solutions are inside.
- Then open [02-first-look.ipynb](../01-intro/02-first-look.ipynb), the first demo notebook.
