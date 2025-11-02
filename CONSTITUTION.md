# AGENT PLAYBOOK

This repository is designed so that coding agents only add new task configs. Do
not modify the core script or any other files unless a maintainer explicitly
asks for a broader change.

## Repository structure

- `src/data_parasite.py` – the single entry-point script. **Always read this
  before attempting a task** so you understand the expected schema, prompts, and
  CLI arguments.
- `tasks/` – subdirectories for each task; every task contains a YAML config and
  its CSV inputs. The `Path2Power/` folder is the canonical reference
  example. Place the JSONL output for each run inside the same task folder,
  alongside the input files for that task.

## Task types

- **Provided dataset** – the user supplies a CSV. Build the corresponding
  `tasks/<task>/<task>_config.yaml`, mirroring the Path2Power example.
- **Curated dataset** – the user requests insights about a list that is not
  already in the repo (e.g., CEOs of the S&P 500). Attempt to curate the list by
  searching for an authoritative source. **Always use web search** to gather the data, 
  then create a dedicated `tasks/<task>/` folder and write a CSV via simple shell commands.
  Keep the generated file alongside the config.

## Workflow rules for agents

1. **Review first**  
   - Read `src/data_parasite.py` to confirm how configs are parsed.
   - Inspect `tasks/Path2Power/Path2Power_config.yaml` and its CSV
     to mirror formatting, schema declarations, and prompts.
2. **Create only configs**  
   - Your job is to add a new `tasks/<task>/<task>_config.yaml` (and any
     accompanying CSV the user supplies or that you curated).  
   - Do **not** edit existing configs, Python files, documentation, or tooling.
     No new folders outside `tasks/<task>/` without maintainer approval.
3. **Generated data handling**  
   - Never alter a user-provided CSV. If you must filter or transform it,
     produce a separate derived file using simple shell commands and store it in
     the same task folder.
   - For curated tasks, ensure the CSV you create clearly documents its source.
4. **Runtime safeguards**  
   - Before running any Python commands, `cd` to the repo root (if needed) and
     activate the virtual environment with `source venv/bin/activate`.
   - Always ask for user permission before running `src/data_parasite.py`.
   - Suggest running with `--sample 10` first, e.g.:
     ```
     python src/data_parasite.py --config_file tasks/<task>/<task>_config.yaml --csv_file tasks/<task>/input.csv --output_file tasks/<task>/<task>.jsonl --sample 10
     ```
   - Do not run full datasets unless the user explicitly requests it.
   - Never install new Python packages unless the user explicitly requests it.
5. **Leave everything else untouched**  
   - No refactors, no formatting passes, no changes to `.gitignore`, README, or
     other files. Stay scoped strictly to the requested config work.

Following this checklist keeps the framework stable while allowing users to add
new tasks safely.
