# Breathe Simulate

A python based api wrapper for running the breathe simulate model.

> **Registration Required**: To use the Breathe Simulate API, you must register for a free trial. You can sign up at [https://breathebatteries.com/knowledge-hub/simulate-trial/](https://breathebatteries.com/knowledge-hub/simulate-trial/).

## Installation steps

### In a new project or existing project

In the virtual environment in your new or existing project, simply install `breathe_simulate` with

```
pip install breathe_simulate
```

### Using this repo

If you want to work with the examples in this repo, follow these steps...

- Open your terminal or command prompt.
- Navigate to the directory or create new one where you want to setup.
- Run the following command to clone our GitHub repository.

  ```bash
  git clone https://github.com/BreatheBatteries/breathe_simulate.git .
  ```

To set up everything in one go, just run `.\setupEnvironment.ps1`.

That will create a virtual environment and install the requirements.

## Alternative manual setup

Alternatively, run following command to create the virtual environment:

```bash
python -m venv myvenv
```

Replace `myvenv` with the desired name for your virtual environment.

- Activate the virtual environment:

  - On **Windows**:
    ```cmd
    myvenv\Scripts\activate
    ```
  - On **macOS/Linux**:
    ```bash
    source myvenv/bin/activate
    ```

  After activating the virtual environment, you'll see `(myenv)` in your terminal prompt to indicate that the environment is active.

- Install the `breathe_simulate` package in the virtual environment:

  ```
  pip install breathe_simulate
  ```

# Examples

Check out the python notebooks in [docs/examples](docs/examples) for how to use the api.

## Long simulations and reattaching

`run_sim` and `run_ageing_sim` print the job id when they submit (`Submitted run_sim job <uuid>`) and stop waiting after 20 minutes. The simulation keeps running on the platform after a timeout, a Ctrl-C or a kernel restart, so you can fetch it later by its id:

```python
import datetime

result = model.run_ageing_sim(..., timeout=datetime.timedelta(hours=2))  # or timeout=None

# later, or in a new kernel:
raw = model.get_job_result("<uuid>")              # waits up to 20 minutes
raw = model.get_job_result("<uuid>", wait=False)  # raises if still pending
```

`get_job_result` returns the raw result dict, not the `run_sim` / `run_ageing_sim` results object.

# Reporting Bugs

If you find a bug, or run into an error, please raise an issue on the GitHub account.
