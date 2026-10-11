# Particle Position with Random Forest

This project predicts where a particle hits a sensor, as an (x, y) point, from the signals the sensor records. It was the final project of the **Data Science Lab** course at **Politecnico di Torino** (Winter 2024).

---

## About this project

This was my **first coding project**. I took this course in my first semester of the master's. Before that, my background was in **product design**, and I had never written code.

I finished it with **one of the highest marks in the class**.

---

## The problem in short

1. A sensor called **RSD** (Resistive Silicon Detector) has **12 pads** on it.
2. When a particle passes through the sensor, each pad records a signal.
3. From these signals, we want to find the **(x, y) position** where the particle passed.
4. This is a **regression** problem with **two outputs** (x and y).

**Score:** the average distance between the predicted point and the true point. **Lower is better.**

---

## The data

- **385,500 events** for training (with the true x and y).
- **128,500 events** for testing (without x and y). The predictions were sent to the course leaderboard.
- For each event there are **5 features**, each read **18 times**:

| Feature | What it means |
|---|---|
| pmax | Height of the positive peak of the signal |
| negpmax | Height of the negative peak of the signal |
| tmax | When the positive peak happens |
| area | Area under the signal |
| rms | Average strength of the signal (root mean square) |

That makes **90 columns**. But the sensor has only 12 pads, so **6 of the 18 readings are just noise**.

---

## What I did

**1. Found the noise**

I drew a histogram (a bar chart of the values) for every column. The readings **0, 7, 12, 15, 16 and 17** looked wrong: they showed a signal in every event, which a real pad should not do. So these 6 readings are the noise.

**2. Removed the noise**

I removed these 6 readings (30 columns) from both datasets. **60 columns** were left.

**3. Trained a Random Forest Regressor**

I chose a Random Forest because:
- it needs very little data preparation,
- it can predict **two outputs (x and y) at once**,
- it gets better with tuning.

**4. Tuned the model**

First I changed the settings by hand. More trees gave better results. Then I ran a small **grid search** (trying many setting combinations) to find the best ones.

**Final settings:**

| Setting | Value |
|---|---|
| n_estimators (number of trees) | 1500 |
| max_depth | 30 |
| min_samples_split | 5 |
| min_samples_leaf | 2 |
| max_features | sqrt |

---

## What helped and what didn't

| Idea | Did it help? |
|---|---|
| Removing the noise columns | Yes |
| Tuning the settings (grid search) | Yes, the biggest gain |
| More trees, up to 1500 | Yes |
| More than 1500 trees | No |
| Scaling the data (min-max, z-score) | No |
| Using only the `tmax` columns | No |

---

## Folders

```
data-science-lab/
├── particle_position_random_forest.ipynb   the code, with its outputs
├── report.pdf                              the 3-page report I submitted
└── requirements.txt
```

---

## How to run

```
pip install -r requirements.txt
jupyter notebook
```

- Put `development.csv` and `evaluation.csv` in a folder named `data`, next to the notebook.
- Then open the notebook and run it from top to bottom.
- It writes the predictions to `finalResult.csv`.
- The data was given by the course, so it is **not** in this repository.

---

## Good to know

- The grid search cell is turned off (commented out), because it takes hours to run.
- The grid search was small (only 20 trees per test) because my computer was not strong enough for a bigger one.
- The notebook is uploaded **as it was submitted**, with its outputs. Only the file paths were changed.

---

## License

The code is under the [MIT License](LICENSE). The data is not mine. It was given by the Data Science Lab course at Politecnico di Torino.
