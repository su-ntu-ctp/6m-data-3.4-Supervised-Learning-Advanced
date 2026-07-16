# Lesson — L04 Advanced Supervised Learning: Trees & Ensembles

> **Chapter 4 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · Day 5.*
> Marcus's brief from yesterday: *"Can you make this model BETTER tomorrow? Try those tree-based models you mentioned."* Same dataset as L03 — `northstar_churn.csv`, 10,000 customers, 11 features, target = `churned`. By Friday Sarah has to show whether trees and ensembles beat the L03 logistic-regression baseline.

This document is a **short reference** — the lesson itself is taught in the notebooks. Read it for orientation before class, then come back for the takeaways, the model-shipping checklist, and the course map.

---

## How L04 is taught

| Stage | Where to go |
|---|---|
| **Pre-class** | `pre-class.md` + `notebooks/01_monday_morning.ipynb` |
| **In-class — Part 1: Decision tree → Random forest** | `notebooks/02_decision_tree_to_forest.ipynb` |
| **In-class — Part 2: Gradient boosting** | `notebooks/03_gradient_boosting.ipynb` |
| **In-class — Part 3: Tuning & comparison** | `notebooks/04_tuning_and_comparison.ipynb` |
| **Self-study** | `notebooks/assignment.ipynb` + `notebooks/optional_extensions.ipynb` |
| **Reference & review** | This document |

The notebooks are the spine. Run them in order.

---

## Overview

Marcus's L03 question — *can you make this better?* — sends Sarah into the workhorse algorithms of tabular ML: decision trees, random forests, and gradient boosting. By Friday she has three tuned candidates competing against the L03 baseline. The day's real lesson isn't "which algorithm wins" (on real datasets they're often within a few F1 points of each other) — it's how to **tune honestly**, **compare fairly**, and **pick the model the team will actually be able to operate**.

---

## Key takeaways

1. **Trees handle non-linear interactions for free.** A tree can split on "tenure < 12 months AND returns_per_purchase > 0.10" without you hand-engineering the interaction. Logistic regression cannot.
2. **Trees don't need scaling.** They split on thresholds, not distances. You still need to one-hot encode categoricals; you do not need `StandardScaler`.
3. **Random forest = bagging.** Many deep trees, each trained on bootstrap samples with random feature subsets, predictions averaged. Variance drops; bias stays low. The robust default first try.
4. **Gradient boosting = sequential error correction.** Each new tree fits the residuals of the running ensemble. Higher accuracy ceiling than random forest, but more sensitive to hyperparameters and more prone to overfitting.
5. **Tune `learning_rate` and `n_estimators` together for boosting.** Lower learning rate needs more rounds; use early stopping to auto-pick. Default hyperparameters are reasonable but rarely optimal.
6. **Use the same scoring metric for tuning, validation, and the business decision.** Tuning for `f1` and reporting accuracy at the end mixes two different optimisation targets.
7. **The highest F1 isn't automatically the right ship.** Explainability, operational stability, team familiarity, and dependency cost are real inputs to the final choice — especially when the F1 gap is small.

---

## Choosing the model to ship — a checklist

After tuning, you'll often have two or three candidates within a few F1 points of each other. Before declaring a winner, walk through this three-step check:

1. **Is the accuracy gap big enough to matter operationally?** A 0.335 vs 0.325 F1 is statistically real but may not justify a more complex stack. If the gap is within the cross-validation standard deviation, treat the models as tied and let the other factors decide.
2. **What does the team have to maintain?** Sklearn-only is simpler than adding XGBoost or LightGBM as a new dependency. Random forest is more stable across retrains; gradient boosting can be brittle. The model someone else can debug at 2am wins ties.
3. **How explainable does this need to be?** If a regulator, a sceptical executive, or an unhappy customer might ask "why did the model flag me?", random forest's feature-importance story beats gradient boosting's. Match the model to the audience that will challenge it.

Skip any of these and you'll ship the model that won on a metric, not the model the business can actually operate.

---

## Key concepts — plain-English review

A quick self-check before the course map. Read each concept; if any feels fuzzy, jump back to the notebook Part that teaches it.

**Decision tree** — A model that asks a series of yes/no questions about the data ("tenure under 12 months?") and follows the answers down branches to a prediction. Easy to read, easy to overgrow.
*Real-world use:* A telephone triage nurse's flowchart — "chest pain? → yes → call ambulance" — is a hand-built decision tree.

**Overfitting** — A model that memorises the quirks of its training data instead of learning general patterns; it aces the past and flops on new cases. Deep, unpruned trees are the classic offenders.
*Real-world use:* A student who memorises last year's exam answers word-for-word and fails when the questions change slightly.

**Bagging / random forest** — Train many trees, each on a random sample of the data with random subsets of features, then let them vote. Individual trees are erratic; the crowd's average is stable.
*Real-world use:* Asking 100 independent property agents to value a house and averaging — one agent may be way off, the average rarely is. The robust default for tabular data like NorthStar's churn table.

**Boosting / gradient boosting** — Build trees one after another, each new tree focusing on the mistakes the ensemble is still making. Higher accuracy ceiling than a forest, but touchier to tune and easier to overfit.
*Real-world use:* Banks' credit-risk models and Kaggle-winning retail demand forecasts are usually gradient-boosted trees.

**Hyperparameter tuning** — Systematically trying different model settings (tree depth, learning rate, number of trees) inside cross-validation, so the winner earned its score on unseen folds rather than luck.
*Real-world use:* A streaming service grid-searching its recommender's settings before rollout — same discipline Sarah uses on the churn booster.

**Feature importance** — A ranking of which inputs the ensemble leaned on most. It tells you *what* drove predictions overall — useful for explanation and for spotting suspicious shortcuts.
*Real-world use:* An insurer discovering "months since last claim" dominates its pricing model — a story executives and regulators can follow.

**The interpretability trade-off** — More complex models often score slightly better but are harder to explain, maintain, and debug. The best F1 isn't automatically the model you ship.
*Real-world use:* A hospital may choose a slightly less accurate but explainable readmission model, because doctors won't act on a black box — the same call Sarah faces when the booster beats the forest by one F1 point.

---

## Where L04 fits in the course

L04's discipline — ensemble methods, hyperparameter tuning inside a cross-validation scaffold, honest model comparison — is the supervised-ML toolkit you'll lean on through L10.

| Lesson | How L04 shows up |
|---|---|
| **L05 — Unsupervised Learning** | No labels means no F1 to tune for — but the "tune one hyperparameter at a time, with cross-validation, against a chosen metric" discipline transfers directly to choosing `k` for k-means or `n_components` for PCA. |
| **L06 — Time Series** | Tree-based models (and gradient boosting in particular) are the standard ML approach to forecasting tabular time series with engineered features. |
| **L07 — Neural Networks** | The tuning logic moves from `GridSearchCV` to manual loops + learning-rate schedules, but the discipline of comparing-fairly-against-a-baseline is identical. |
| **L08 — Computer Vision** | Transfer learning's fine-tune-vs-feature-extract choice is the same "complexity vs operational cost" trade-off L04's model-comparison taught. |
| **L09 — NLP & Embeddings** | When labelled retrieval data exists, gradient boosting reranks remain a competitive baseline against learned rerankers. |
| **L10 — Transformers & GenAI** | Prompt and pipeline variants are compared the same way model variants were here: same eval set, same metric, same cross-validated comparison. The candidate space changes; the discipline doesn't. |

---

> *"Nice. Now — what about all those customers who DON'T churn but also don't log in for a year? Can we find natural clusters of customer behaviour? Without labels?"* — Marcus, after Sarah ships the tuned booster.
>
> That question — *can we find structure when there's no target column?* — is the engine of **L05 (Unsupervised Learning)**.
