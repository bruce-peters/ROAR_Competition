# ROAR_Competition

Starter code for the ROAR competition. There's a [documentation](https://roar.gitbook.io/roar-competition-documentation/) accompanying this repository.

This is a fork of [MightyMark3/ROAR_Competition](https://github.com/MightyMark3/ROAR_Competition), which descends from the Berkeley [augcog/ROAR_Competition](https://github.com/augcog/ROAR_Competition) skeleton. It is currently identical to upstream at commit `a5674bd`. The solution is a waypoint-following controller for the Monza map, run inside the CARLA simulator through the `ROAR_PY` control suite.

## Run it

> **Untested here.** This needs the CARLA simulator with the ROAR Monza map, which ships as a Windows (AMD64) executable, and a GPU-capable machine. Neither was available in this review. The only check was `python -m py_compile` on every `.py` file, and all of them passed. The steps below follow the official [installation guide](https://roar.gitbook.io/roar-competition-documentation/installation).

Prerequisites:
- The **Monza map** simulator build from <https://roar.berkeley.edu/monza-map/> (Windows AMD64). It is CARLA-based and listens on `127.0.0.1:2000`.
- **Python 3.8** (the docs use conda)
- **ROAR_PY**: the `roar_py_core` interface and the `roar_py_carla` backend from <https://github.com/augcog/ROAR_PY>
- The CARLA Python API pinned to **0.9.12**

```sh
conda create -n roar_competition python=3.8
conda activate roar_competition

git clone https://github.com/augcog/ROAR_PY
pip install -e ROAR_PY/roar_py_core
pip install -e ROAR_PY/roar_py_carla
pip install carla==0.9.12 pygame gymnasium numpy
```

Start the Monza map executable first. Then, from this repo:

```sh
cd competition_code
python competition_runner.py
```

`competition_runner.py` connects to the simulator at `127.0.0.1:2000`, spawns the car at the first waypoint, and runs `RoarCompetitionSolution` from `submission.py` for 3 laps with a manual-control viewer window. When it finishes it prints the elapsed time.

## Project structure

| Path | What it is |
|---|---|
| `competition_code/submission.py` | The solution: `RoarCompetitionSolution` (the file a competitor edits) |
| `competition_code/competition_runner.py` | Evaluation harness: lap/waypoint progress rule, collision respawn, timing |
| `competition_code/infrastructure.py` | Agent wrapper and the pygame `ManualControlViewer` |
| `competition_code/new_code/` | Experimental variants: `submission_v1` … `submission_v5`, a runner that imports `submission_v3`, a debug infrastructure, and waypoint dumps |
| `competition_code/old/` | Earlier PID and waypoint-following planner code |
