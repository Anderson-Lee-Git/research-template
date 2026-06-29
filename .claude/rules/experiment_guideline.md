# Experiment Guidelines

Best practices for ML research experimentation: intent, logging, observability, reproducibility, and organization. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward rigor and recall over speed. A throwaway sanity check doesn't need the full apparatus — but the moment a result might inform a decision, treat it as a real experiment.

## 1. Every Experiment Starts With Intent

**Write down what you're testing and why — before you run it.**

- State the question or hypothesis in one sentence. ("Does X improve Y over baseline Z?" not "try X")
- Record what outcome would confirm it and what would refute it.
- Name the baseline you're comparing against.
- If you can't articulate the intent, you're not ready to run.

The test: Someone reading the experiment cold should know why it exists without asking you.

## 2. Capture Enough to Reproduce

**An experiment you can't rerun is an anecdote.**

Every run records, automatically, at launch:
- The exact config / hyperparameters — saved as a file, not just CLI flags in shell history.
- Code version (git SHA) and whether the working tree was dirty (save the diff if so).
- Random seed(s).
- Environment: key dependency versions, hardware, dataset name/version/path.
- The exact command used to launch it.

Prefer emitting this from code at run start over reconstructing it later from memory.

The test: You can reproduce any past number from what's saved in its run directory alone.

## 3. One Experiment, One Directory

**Never overwrite. Never mix runs.**

- Each run gets its own directory named with a sortable ID (timestamp + short slug): `2026-06-29_1430_baseline-lr3e4/`.
- Config, logs, metrics, checkpoints, and figures for that run live inside it.
- Treat run directories as immutable once written — re-running produces a *new* directory.
- Keep raw outputs separate from derived/analysis artifacts.

Suggested layout:
```
experiments/
  <run-id>/
    config.yaml      # frozen config for this run
    meta.json        # git SHA, seed, command, env, start/end time
    logs/            # stdout + structured logs
    metrics/         # metrics over time (jsonl/csv)
    checkpoints/     # model artifacts
    figures/         # plots generated from this run's metrics
    NOTES.md         # intent, observations, conclusion
```

## 4. Log for Observability, Not Just the Final Number

**You should be able to see what happened during a run, not only after it.**

- Log metrics over time (step/epoch) to a machine-readable file (jsonl/csv), and to your tracker of choice.
- Log enough to diagnose failure: loss curves and key intermediate stats, not just final accuracy.
- Make runs observable in flight — you should be able to tell a diverging run from a healthy one without waiting for it to finish.
- Send errors and warnings to a file in the run dir; don't let them vanish in a scrollback buffer.

The test: When a run looks wrong, the logs tell you *where* it went wrong.

## 5. Results Organization Maps Back to Intent

**Every derived result is traceable to the runs that produced it.**

- Aggregate/comparison artifacts (tables across runs) reference run IDs explicitly.
- Don't hand-copy numbers — generate summary tables from the saved metrics.
- Maintain a top-level index/manifest: run ID → intent → headline result → conclusion.
- Update the index when a run completes. A result with no recorded conclusion is unfinished.

The test: From the index alone you can find the run behind any reported number.

## 6. Analysis and Plots Are Reproducible Artifacts

**No throwaway analysis. If a plot matters, a script regenerates it.**

- Analysis and visualization read from saved metrics; they never re-run training.
- Keep the analysis script alongside its outputs, and make it deterministic from run dirs.
- Save plots to disk (the run's `figures/`, or a dedicated comparison dir) — not just shown interactively.
- Label plots fully: axes, units, run IDs, what's being compared. A plot you can't interpret in six months is noise.

The test: You can regenerate every figure in a report by running a script, not by remembering what you clicked.

## 7. Record the Conclusion

**An experiment isn't done when it finishes running — it's done when you've written what it told you.**

- Write the outcome into the run's `NOTES.md` and the top-level index: confirmed / refuted / inconclusive, with the number.
- Note surprises and the next question they raise.
- Negative and failed results are still results — record them so you don't repeat them.

The test: Months later you can answer "what did we learn from X?" from the notes, not from memory.

---

**These guidelines are working if:** you can find any past run and know why it was run and what it showed; any number can be reproduced from its directory; and analysis and plots regenerate from a script rather than from recollection.
