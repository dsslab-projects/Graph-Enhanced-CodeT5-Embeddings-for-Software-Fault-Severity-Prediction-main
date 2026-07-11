# Graph-Enhanced CodeT5+ Embeddings for Software Fault Severity Prediction

Reproduction package for the paper *Graph-Enhanced CodeT5 Plus Embeddings for Software Fault Severity
Prediction*. One notebook runs the whole experiment — graph-enhanced embedding extraction, downstream
classification, the ablation study and the significance tests — and a reporting layer prints the
RQ1–RQ4 answers, with figures, **from the tables it just computed**.

[![Open in Kaggle](https://kaggle.com/static/images/open-in-kaggle.svg)](https://kaggle.com/kernels/welcome?src=https://github.com/dsslab-projects/Graph-Enhanced-CodeT5-Embeddings-for-Software-Fault-Severity-Prediction-main/blob/main/Graph_Enhanced_CodeT5p_Severity_Prediction.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dsslab-projects/Graph-Enhanced-CodeT5-Embeddings-for-Software-Fault-Severity-Prediction-main/blob/main/Graph_Enhanced_CodeT5p_Severity_Prediction.ipynb)

## Quickstart (Kaggle recommended)

**Kaggle** is the smoothest path — it has a free GPU, keeps the git-LFS checkpoints downloading fast,
and stores the TabPFN token as a Secret.

1. Click **Open in Kaggle** above (or *New Notebook → File → Import* the `.ipynb`).
2. In the right panel: **Settings → Accelerator → GPU**, and **Settings → Internet → On**.
3. *(optional, for TabPFN)* add the token as a Secret — see below.
4. **Run All.** The notebook clones the repo (installing git-lfs if needed), loads the checkpoints,
   extracts embeddings, trains the ten classifiers, runs both ablations, and prints RQ1–RQ4 with
   charts. Watch the `SOURCE:` line before RQ1 — it should say *all tables freshly computed*.

**Colab** works too (click the Colab badge): `Runtime → Change runtime type → GPU`, then Run All. On
Colab there are no Secrets, so paste the TabPFN token into the Config cell (`tabpfn_token = "..."`) or
set the `TABPFN_TOKEN` environment variable.

Nothing is placed by hand — the dataset and checkpoints are pulled from this repo on demand. Results
are **computed fresh every run** (written to `outputs/`); if a stage cannot run, its RQ says
*"not computed"* rather than showing any cached number.

## TabPFN token (optional — one of the ten classifiers)

TabPFN runs through the hosted Prior Labs client and needs a free token. Without it, TabPFN is skipped
and the other nine classifiers still run.

1. Get a key at **https://ux.priorlabs.ai/home** (sign in → copy your API token).
2. **On Kaggle:** open the notebook → **Add-ons → Secrets → Add a new secret** → **Label** it
   `TABPFN_TOKEN`, paste the token as the value, and make sure it is **attached** to the notebook.
   Leave `tabpfn_token = ""` in the Config cell — the notebook reads the Secret automatically.
3. **On Colab / locally:** paste the token into `tabpfn_token` in the Config cell, or export
   `TABPFN_TOKEN` as an environment variable.

The notebook resolves the token as: Config field → Kaggle Secret → environment variable, and installs
`tabpfn-client` on demand. The Prerequisites cell shows `[ ok ] TabPFN token` once it is found.

## Default switches

| Stage | Default | Why |
|---|---|---|
| Embedding extraction, downstream, partial + full ablation | **On** | reproduce the paper's tables |
| Metric extraction, data-flow-graph extraction | Off | the hosted `processed data` already carries the `*_robust` and `data_flow_graph` columns |
| Stage-1 fine-tuning | Off | the released checkpoints are used instead of retraining |

## Data, checkpoints and credentials

The dataset (`processed data/`) and the three fine-tuned checkpoints (`model checkpoints/`, Git LFS)
ship with the repo. The notebook `git clone`s the repo once (installing git-lfs if the runtime lacks
it) to get the real checkpoints, so it works even if you only opened the `.ipynb`. To use your own
files, set `data_dir` / `checkpoint_dir` in the Config cell.

> Result tables are **not committed** and **not hard-coded** — they are computed on each run and
> written to `outputs/`. Git LFS has a monthly bandwidth quota, so many full-checkpoint downloads can
> exhaust it; a full clone or a mirror avoids that. To pin an exact snapshot, set `github_ref` to a
> commit SHA instead of the branch name.

## Repository layout

```
Graph_Enhanced_CodeT5p_Severity_Prediction.ipynb   the pipeline (run this)
CODE_DOCUMENTATION.docx                             full component-by-component guide
requirements.txt
processed data/     train_df.pkl, val_df.pkl, test_df.pkl        (2,673 / 334 / 335)
model checkpoints/  codet5p-fsp-pytorch-{default,no-gnn,no-handcrafted-metrics}-v1/*.pt   (LFS)
outputs/            figures, computed result tables, paper_observations.md
```
