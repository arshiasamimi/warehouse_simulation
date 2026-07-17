# Warehouse Order Fulfillment Simulation

Discrete-event simulation of an e-commerce warehouse order-fulfillment center,
submitted under the Alternative Simulation Bonus Opportunity for the Computer
Simulation course (Dr. Rahman Farnoosh, Spring 2026, Iran University of Science and
Technology). Orders arrive randomly, are verified, and require a picker to gather
items from the floor; pickers are a limited resource, a finite staging area absorbs
overflow, and orders can end in one of three exceptional outcomes: backorder,
cancellation, or an unresolved picking error.

This project is intentionally structured as a genuinely different process and
decision problem from the required hospital bed-allocation simulation (see
Section 2 of `warehouse_report.pdf` for the full requirement-by-requirement
mapping), while matching its statistical and technical rigor.

## File Structure

```
.
├── Warehouse_Simulation_Full.ipynb  # Full implementation: engine, verification,
│                                     # metrics, experiments, statistical analysis
├── warehouse_report.pdf             # Final report (assumptions, results, CIs,
│                                     # scenario comparison, recommendation)
├── plots and tables/                # All generated figures and CSV outputs
│                                     # (order-level tables, hourly monitoring
│                                     # tables, replication metrics, scenario
│                                     # summaries) — produced by running the notebook
├── requirements.txt                 # Python dependencies
├── venv/                            # Local virtual environment (not tracked in git)
├── README.md                        # This file
└── .gitignore
```

## Requirements

- Python 3.10+
- Dependencies listed in `requirements.txt` (numpy, pandas, scipy, matplotlib, and
  Jupyter)

## Setup

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Running the Simulation

1. Activate the virtual environment (see above).
2. Launch Jupyter and open `Warehouse_Simulation_Full.ipynb`:
   ```bash
   jupyter notebook Warehouse_Simulation_Full.ipynb
   ```
3. Run all cells top to bottom (**Cell → Run All**, or **Restart & Run All**).
   The notebook is self-contained and will:
   - Define and verify the discrete-event simulation engine (pickers, staging
     queue, picking-error/redo mechanic).
   - Run the base case and generate its required visualizations.
   - Run all three experiments (picker-capacity sweep, urgency-based picking
     policy, and the exponential-arrival demand stress test) plus the combined
     capacity + policy scenario, each with 30 independent replications.
   - Build the consolidated confidence-interval comparison tables and
     practical-significance matrices, separately for uniform and exponential
     arrivals.
   - Save every table (CSV) and figure (PNG) into the working directory —
     move or copy these into `plots and tables/` to reproduce that folder.

Total runtime is roughly 6-8 minutes on a typical laptop (13 scenarios x 30
replications), most of it spent in the simulation runs.

## Reproducing Results

All random seeds are derived deterministically from a scenario key and replication
index (`seed_for`), so re-running the notebook end to end reproduces the exact same
numbers reported in `warehouse_report.pdf`.

## Report

`warehouse_report.pdf` contains the full write-up: the minimum-complexity checklist
mapping, model assumptions, implementation explanation, experiment design and
justification, results, confidence intervals, scenario comparisons (uniform and
exponential arrivals kept separate throughout), and the final recommendation. The
LaTeX source is available on request.

## Verification

The notebook programmatically checks, for every one of its 30-replication runs
(390 replications total across all 13 scenarios):
- No picker is assigned to more than one order at the same time.
- Busy pickers never exceed picker count; staging queue length never exceeds
  staging capacity.
- Every fulfilled/failed order has complete picker/assignment/completion records.
- Every backorder/cancelled order has a complete exception record.
- All replications use distinct random seeds.

All checks pass with zero issues across every scenario reported in this project.
