# L04 — Advanced Supervised Learning (Trees & Ensembles)

> *Sarah Chen's fifth day at NorthStar Retail. Yesterday Marcus asked: "Can you make this model BETTER tomorrow? Try those tree-based models you mentioned." Today she does.*
> By the end of this lesson you will know how to train Random Forest and Gradient Boosting classifiers, how to tune their hyperparameters with cross-validation, and how to choose between competing models with honest evidence.

---

## Before you start — environment setup

> **If this is your first time with this course, follow the [Setup Guide →](./setup.md)** to install the Python environment. It takes 10–15 minutes.
>
> **Already set up from L01–L03?** Your `dsai-m3` environment has everything (`scikit-learn` is included).

---

## Where L04 fits

| Lesson | What you build | What you carry forward |
|---|---|---|
| **L03 — Supervised Foundations** | First trained model — logistic regression on NorthStar churn | A working pipeline (preprocess + train + evaluate) — and Marcus's question: *"Can you make it better?"* |
| **L04 — Trees & Ensembles** *(you are here)* | Random Forest + Gradient Boosting on the same data — and tune them properly | A toolkit that wins Kaggle and wins production |
| **Kaggle Competition** | Apply this toolkit on a held-out scoring split | First public leaderboard ranking |
| **L05 — Unsupervised Learning** | Find structure without labels | Clustering and dimensionality reduction |

**The narrative thread:** Sarah's L03 logistic regression scored recall = 26% at the chosen threshold. Today she has to beat it.

---

## What you will be able to do by the end

- **Explain** why decision trees beat linear models on data with strong feature interactions — and where they fall over
- **Train** a Random Forest classifier and explain the bagging idea that makes it more stable than a single tree
- **Train** a Gradient Boosting classifier and explain the sequential-error-correction idea that makes it consistently the winning algorithm on tabular data
- **Tune** hyperparameters with `GridSearchCV` and pick a final model with cross-validated evidence
- **Compare** competing models honestly — same data, same metric, same threshold — and recommend one to a non-technical stakeholder

---

## How class time is structured (~3 hrs)

| Phase | Time | Format |
|---|---|---|
| Concept recap (slides) | ~45–60 min | Instructor recaps the key concepts with the lesson slides — you already explored the [interactive key-concepts page](https://su-ntu-ctp.github.io/6m-data-3.4-Supervised-Learning-Advanced/) pre-class |
| Hands-on code-alongs | ~90 min | Three notebooks (~25–30 min each) — Core sections only |
| Class exit survey | ~15 min | Quick survey to capture what clicked and what didn't — helps shape the next session |
| (Self-study after class) | self-paced | Each notebook has a 🟡 Extension section + the Kaggle assignment |

**Why this structure?** Same pacing as L01–L03: ~1 hour of slide-based concept recap + ~1.5 hours of hands-on with clearly marked 🟡 Extension boundaries, closing with a 15-minute exit survey.

---

## Your learning path

### Phase 1 — Before class: self-study (~30 min)

**Goal:** Understand bagging vs boosting at the conceptual level. Arrive at class ready to compare algorithms.

**Start here →** [**pre-class.md**](./pre-class.md)

You will:
- Run `notebooks/01_morning_briefing.ipynb` (~15 min) — Marcus's brief, the L03 baseline you're trying to beat
- Watch two short videos (StatQuest on Random Forest and Gradient Boosting)
- Explore the [**interactive key-concepts page**](https://su-ntu-ctp.github.io/6m-data-3.4-Supervised-Learning-Advanced/) after the videos (GitHub Pages)
- Try three mini-exercises with sample answers

---

### Phase 2 — In class: concept review + hands-on notebooks (~3 hrs)

**Short reference & review →** [**lesson.md**](./lesson.md) (overview, key takeaways, model-shipping checklist, L05→L10 course map)

**Need a recap? →** the [**key concepts page**](https://su-ntu-ctp.github.io/6m-data-3.4-Supervised-Learning-Advanced/) you explored pre-class is there to revisit any time.

**Notebooks — run in order:**

| # | Notebook | Sarah's day | What you explore |
|---|---|---|---|
| 02 | [`02_decision_tree_to_forest.ipynb`](./notebooks/02_decision_tree_to_forest.ipynb) | Late morning | Decision tree mechanics · why a single tree overfits · Random Forest (bagging) |
| 03 | [`03_gradient_boosting.ipynb`](./notebooks/03_gradient_boosting.ipynb) | Early afternoon | Boosting concept · HistGradientBoostingClassifier · feature importance |
| 04 | [`04_tuning_and_comparison.ipynb`](./notebooks/04_tuning_and_comparison.ipynb) | Late afternoon | GridSearchCV · model comparison · the end-of-day recommendation |

Each notebook opens with a business scenario, guides you through the code with **Pause & Predict** prompts, and ends at a clearly marked 🟡 Extension boundary.

---

### Phase 3 — After class: Kaggle competition + assignment (self-paced)

**Assignment →** [`notebooks/assignment.ipynb`](./notebooks/assignment.ipynb)

A Kaggle-style competition on a held-out NorthStar-churn scoring set. Three tiers:

- **Tier 1** — Beat F1 ≈ 0.20 on the scoring set with any default-settings model
- **Tier 2** — Beat F1 ≈ 0.30 (roughly a tuned Random Forest / default Gradient Boosting)
- **Tier 3** — Open optimisation: beat F1 ≈ 0.35 with your best tuned model

Followed by an independent banking-fraud detection scenario.

**Further reading →** [**reference.md**](./reference.md)

---

## Core vs Optional — what this lesson teaches

**🟢 Core (taught in class):**
- sklearn `Pipeline` end-to-end workflow (recap + extended with new estimators)
- Random Forest classifier
- Gradient Boosting (HistGradientBoostingClassifier)
- Hyperparameter tuning with `GridSearchCV`

**🟡 Optional (self-study, not assessed):**
- Ridge / Lasso / ElasticNet regularisation for linear models
- Gini vs Entropy split criteria — the math behind tree splits
- GridSearch vs RandomizedSearch comparison
- XGBoost / LightGBM (if installed)

Optional material lives in [`notebooks/optional_extensions.ipynb`](./notebooks/optional_extensions.ipynb).

---

## File map

```
README.md                              ← You are here
setup.md                               ← One-time environment setup (same dsai-m3 env)
pre-class.md                           ← Phase 1: 30-min self-study guide
lesson.md                              ← Short reference: overview, takeaways, model-shipping checklist, course map
reference.md                           ← Phase 3: Further reading + glossary
environment.yml                        ← Conda environment spec (no new packages vs L03)
docs/
  index.html                           ← Interactive key-concepts page — explore during pre-class (served at https://su-ntu-ctp.github.io/6m-data-3.4-Supervised-Learning-Advanced/ via GitHub Pages)
notebooks/
  data/
    northstar_churn.csv                ← Same dataset as L03 (story continuity)
  01_morning_briefing.ipynb              ← Pre-class hook: revisit the L03 baseline (~15 min, before class)
  02_decision_tree_to_forest.ipynb     ← Part 1: Tree → Random Forest (late morning, in class)
  03_gradient_boosting.ipynb           ← Part 2: Gradient Boosting (early afternoon, in class)
  04_tuning_and_comparison.ipynb       ← Part 3: GridSearch + model comparison (late afternoon, in class)
  assignment.ipynb                     ← Kaggle-style competition (after class)
  optional_extensions.ipynb            ← 🟡 Optional: Ridge/Lasso · Gini vs Entropy · GridSearch vs Random
```

---

## Maintainer note

A few notebooks quote **snapshot metrics** from earlier runs (e.g. the L03 baseline F1 ≈ 0.28 in NB 02, and the NB 01 baseline recall/precision constants at the top of NB 04's capacity-sweep cell). These are labelled as snapshots in the notebooks, but if `northstar_churn.csv` or any train/test split is ever regenerated, re-run NB 01 and update those snapshot values to match.
