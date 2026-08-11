# Multi-Agent Sheepherding Simulation

Agent-based model of sheep herding, built to study how the number of dogs required to herd a flock scales with flock size. Sheep follow local flocking rules: topological neighbour interactions, a two-mode grazing/fleeing behaviour, and a density-gradient approximation of the flock's centre of mass. Dogs collect the flock into a containment zone, hold it there, then drive it out through a gate. The main simulation campaign covers 10 team sizes (1 to 35 dogs) and 11 flock sizes (5 to 400 sheep), with 100 runs per condition.

Developed by Avrile Floro and Victor Micha (Institut Polytechnique de Paris), in collaboration with Ada Diaconescu (Télécom Paris).

## Contents

```
model/
  sheepherding.nlogox        Main NetLogo model. The BehaviorSpace experiments
                             define the simulation campaign and the sensitivity re-runs.
  gradient_logging.nlogox    Variant that logs, at every tick, the angular difference
                             between the density-gradient direction and the true
                             centre-of-mass direction.
analysis/
  herding_analysis.ipynb     Success rates, completion times, flock spread,
                             dog travel distance, predictor comparison, failure
                             breakdown, minimum dogs and crowding effect.
  failure_analysis.ipynb     Failure phases for two specific conditions.
  repulsion_sensitivity.ipynb  Effect of a smaller sheep repulsion radius.
  gradient_validation.ipynb  Accuracy of the density-gradient heuristic.
data/
  all_runs_merged_full.csv   Main campaign, 11,000 runs, one row per run
                             (density gradient on, repulsion radius 3).
  repulsion_2.5/             Re-runs of four conditions with repulsion radius 2.5.
  gradient_summary.csv       Aggregated accuracy of the density gradient.
requirements.txt             Python dependencies for the analysis.
LICENSE                      MIT.
```

## Requirements

- NetLogo 7.0.3 or later, for the simulation.
- Python 3 with the packages in `requirements.txt`, for the analysis.

## Running the simulation

Open `model/sheepherding.nlogox` in NetLogo, choose `nb-sheep` and `nb-dogs`, then click Setup and Go. A run succeeds when all sheep have exited through the gate. The `use-density-gradient?` switch selects between local-only cohesion (density gradient) and global cohesion (true centre of mass).

The experiments under Tools > BehaviorSpace reproduce the simulation campaign. The per-tick gradient logs are produced by `model/gradient_logging.nlogox`, one file per (D, N) condition.

## Running the analysis

```bash
pip install -r requirements.txt
jupyter lab analysis/
```

`herding_analysis.ipynb`, `failure_analysis.ipynb` and `repulsion_sensitivity.ipynb` read the tables in `data/` and run in seconds. `gradient_validation.ipynb` reads the raw per-tick logs (about 400 MB, not included), which can be regenerated with `model/gradient_logging.nlogox` (one repetition per (D, N) condition, files placed in `analysis/`); its aggregated result is included as `data/gradient_summary.csv`.

## License

MIT, see [LICENSE](LICENSE).
