# Implementation Preferences

Personal tooling and implementation defaults for this repository. This is a **living document** — preferences get added, revised, and dropped as the project evolves. Treat entries as defaults to follow unless a task gives a reason to deviate; when in doubt, ask rather than silently picking something else.

**How to use this file:** Each preference is a short, self-contained note. Prefer adding a new note or amending an existing one over rewriting the whole file. Keep entries open-ended ("I lean toward X because Y") rather than rigid rules, so they're easy to revisit.

## Config

- **Hydra** for configuration management. Compose configs from groups; avoid hardcoding hyperparameters or paths in code.

## Environment

- **`uv`** for Python environment and dependency management.

## Inference

Choice of inference path depends on the model-loading pattern of the job:

- **Same model across many jobs → run a server.** Stand up a **vLLM** server and turn inference into requests against it. This avoids reloading the same weights repeatedly and lets jobs share one warm model.
- **Different models across jobs → offline interface.** Use the offline **`llm.chat()`** interface so each job loads what it needs without server overhead. More efficient when weights differ from job to job.

The deciding question: *are these jobs hitting the same model or different ones?*

## Slurm / Cluster

- Compute nodes have **no internet access**. Pre-download anything from Hugging Face onto the shared filesystem before submitting jobs.
- Local download locations:
  - **`./models`** for HF models.
  - **`./datasets`** for HF datasets.
- Point code at these local paths on the cluster rather than relying on on-the-fly downloads.
- Allocation, queue buckets, and partition choice: see `cluster_guideline.md`.

---

*Add new preferences below as they come up. Keep them short and revisable.*
