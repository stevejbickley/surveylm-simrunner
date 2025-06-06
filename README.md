# surveylm-simrunner
A lightweight wrapper for simulation execution via the [SurveyLM](https://surveylm.panalogy-lab.com/Platform) platform.

This repo acts as a browser-level wrapper (via Selenium) for submitting agent and survey inputs, configuring simulation parameters, and downloading output data — enabling streamlined, scriptable SurveyLM workflows.

---

## Features

- Uploads Excel-based survey and agent profile inputs
- Configures simulation parameters (models, roles, prompts, temperature, etc.)
- Supports both batch and test runs
- Automates estimation and simulation execution
- Downloads simulation result files (usage, completed, uncompleted)
- Pattern-based matching of file pairs
- Crawl subfolders or selectively target input sets
- Credential handling via environment variables

---

## Project Setup

### 1. Clone the repository

```bash
git clone https://github.com/your_org/surveylm-simrunner.git
cd surveylm-simrunner
```

### 2. Set up the Python environment using Poetry

```bash
poetry install
```

To activate the environment:

```bash
poetry shell
```

Requires Poetry installed globally. See: https://python-poetry.org/docs/#installation

---

## Environment Variables

Set your SurveyLM credentials using environment variables for secure access:

```bash
export SURVEYLM_USERNAME="Your Name"
export SURVEYLM_USERID="your_username"
export SURVEYLM_EMAIL="your@email.com"
export SURVEYLM_PASSWORD="your_password"
export SURVEYLM_APIKEY="sk-xxxxxx"
```

You can also add these to your .zshrc or .bashrc for persistence.

---

## Folder Structure

| Path            | Description                                                                      |
|-----------------|----------------------------------------------------------------------------------|
| `data/inputs/`  | Input directory for your `questions_input_*.xlsx` and `sample_profiles_input_*.xlsx` files |
| `data/outputs/` | Output directory for all downloaded result files                                |


---

## How to Run

Activate the poetry environment and run:

```bash
poetry run python run_simulations.py
```

This will:

1. Look for input files in simulation_outputs/inputs/
2. Match question and agent profile files based on filename patterns
3. Launch Selenium, run simulations via the SurveyLM platform, and download results

---

## Customizing Parameters

Within run_simulations.py, edit the parameters dictionary to adjust simulation settings:

parameters = {
    "batch_survey": False,
    "reset_parameters": False,
    "test_run": True,
    "test_q": 3,
    "model": "GPT-4",
    "temperature_low": 0.2,
    "temperature_high": 0.8,
    "max_retries": 5,
    "agent_role": ["Person", "Assistant"],
    "justification": ["Yes"],
    "critic": ["No"],
    # Optional:
    # "agent_role_prob_dist": "0.7;0.3",
    # "justification_prompt": "Example prompt",
}

You can also call process_all_files(...) with a specific_pattern argument to only run selected studies:

specific_pattern = ['SMITH_STUDY_1', 'JONES_STUDY_A']

---

## Simulation Flow

The run_simulations.py script performs the following:

1. Match Files: Pairs input survey and agent profile Excel files using filename pattern matching. 
2. Open Browser: Launches Chrome with Selenium and navigates to the SurveyLM interface. 
3. Authenticate: Logs in using credentials provided via env vars. 
4. Upload Inputs: Uploads the matched input files. 
5. Configure Simulation: Sets all relevant parameters programmatically. 
6. Run Simulation: Estimates cost, runs the full simulation, waits for completion. 
7. Download Results: Downloads usage, completed, and uncompleted data (if available). 
8. Repeat: Iterates across all matched input pairs.

---

## Notes & Tips

1. ChromeDriver must be installed and available in your system PATH. 
2. Adjust timing delays (e.g., time.sleep(...)) if running on slower hardware or remote servers. 
3. headless mode is not yet enabled by default but can be added to setup_selenium(...). 
4. Works with both local and hosted versions of SurveyLM (localhost:8501 or production URL).

---

## To-Do

1. Add CLI support for pattern targeting and dry-run modes 
2. Enable headless mode via flag 
3. Improve error handling & logging
4. Add Docker container for easier deployment

