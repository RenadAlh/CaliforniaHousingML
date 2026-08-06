<p align="center">
  <img src="assets/dataset card.jpeg" alt="California Housing Dataset Card" width="100%">
</p>

<div align="center">

# California Housing | Classification & Regression

Predicting California housing prices two ways: as a classification problem (is a house **"expensive"** relative to the market?) and as a regression problem (what is its actual median value?) using the scikit-learn **California Housing** dataset, with a linear/simple model benchmarked against a stronger non-linear model on each task.

![Python](https://img.shields.io/badge/Python-3776AB?style=plastic&logo=python&logoColor=FFD43B)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=plastic&logo=scikitlearn&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=plastic&logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=plastic&logo=jupyter&logoColor=white)
[![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=plastic&logo=kaggle&logoColor=white)](https://www.kaggle.com/datasets/camnugent/california-housing-prices)


[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RenadAlh/CaliforniaHousingML/blob/main/california_housing_ml.ipynb)

</div>


## Dataset details

- **Source:** `sklearn.datasets.fetch_california_housing` derived from the 1990 US Census.
- **Size:** 20,640 rows × 8 features, no missing values.
- **Features:** `MedInc`, `HouseAge`, `AveRooms`, `AveBedrms`, `Population`, `AveOccup`, `Latitude`, `Longitude`.
- **Target:** `MedHouseVal`, median house value per district, in units of $100,000, capped at 5.0 ($500k).
- **Classification label:** houses are labeled "expensive" if `MedHouseVal` is above the dataset's own median (a relative split, not a fixed dollar threshold).

<br>

## Insights

### Classification; *is a house "expensive"?*

Logistic Regression is benchmarked against a Random Forest on the same train/test split.

<div align="center">
  <img src="assets/figures/correlation_heatmap.png" alt="Correlation Heatmap" width="600">
</div>

> The correlation heatmap flags `MedInc` as the feature most linearly related to price, which is why it dominates the Logistic Regression decision boundary. 

<div align="center">
  <img src="assets/figures/confusion_matrix.png" alt="Confusion Matrix" width="600">
</div>

> The confusion matrix shows the errors are split almost evenly, 357 false positives vs. 360 false negatives out of 4,128 test houses, so the model isn't biased toward over- or under-predicting "expensive."

<div align="center">
  <img src="assets/figures/model_comparison_bar.png" alt="Model Comparison Bar Chart" width="600">
</div>

> Random Forest beats Logistic Regression on every metric, which means the true relationship between features and price bracket isn't fully linear.

<div align="center">
  <img src="assets/figures/feature_importance.png" alt="Feature Importance" width="600">
</div>

> The Random Forest's feature importance chart shows which variables it actually leans on, useful for sanity-checking that the model's decisions line up with domain intuition rather than picking up on noise.

<br>

### Regression; *what is the actual price?*

Polynomial Regression (degree 2) is benchmarked against a plain Linear Regression baseline on the same features.

<div align="center">
  <img src="assets/figures/actual_vs_predicted_side_by_side.png" alt="Actual vs Predicted Side by Side" width="800">
</div>

> Side by side, the Polynomial Regression's points hug the diagonal (perfect-prediction line) more tightly than the Linear Regression's, especially in the middle price range, visual confirmation that the degree-2 terms are capturing real curvature in the price relationship, not just adding noise.

<div align="center">

<table>
<tr>
<td align="center">

**Price Distribution**

<img src="assets/figures/hist_house_value.png" alt="Distribution of Median House Values" width="450">

</td>
<td align="center">

**Income vs. Price**

<img src="assets/figures/income_vs_value.png" alt="Median Income vs Median House Value" width="450">

</td>
</tr>
</table>

</div>

> Most median house values are between 1.0 and 2.5, with fewer high-value houses. The spike at 5.0 indicates that values above this limit were recorded as 5.0. The income-vs-price scatter shows a clear upward trend with meaningful spread, the raw signal both models are trying to capture.

<br>

## Findings / Results

<h3 align="center">Classification</h3>

<div align="center">

<table>
  <thead>
    <tr>
      <th>Model</th>
      <th>Accuracy</th>
      <th>F1-score</th>
      <th>ROC-AUC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Logistic Regression</td>
      <td>82.6%</td>
      <td>0.83</td>
      <td>0.826</td>
    </tr>
    <tr>
      <td><strong>Random Forest</strong></td>
      <td><strong>89.7%</strong></td>
      <td><strong>0.90</strong></td>
      <td><strong>0.961</strong></td>
    </tr>
  </tbody>
</table>

</div>

- Logistic Regression's cross-validated accuracy (83.0%) closely tracks its test accuracy (82.6%), so it isn't overfit, it's simply less expressive than the Random Forest.
- Random Forest improves accuracy by **+7.1** points and ROC-AUC by **+0.135** over Logistic Regression.

<br>

<h3 align="center">Regression</h3>

<div align="center">

<table>
  <thead>
    <tr>
      <th>Model</th>
      <th>MSE</th>
      <th>R²</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Linear Regression</td>
      <td>0.556</td>
      <td>0.576</td>
    </tr>
    <tr>
      <td><strong>Polynomial Regression (Degree = 2)</strong></td>
      <td><strong>0.464</strong></td>
      <td><strong>0.646</strong></td>
    </tr>
  </tbody>
</table>

</div>

- Polynomial Regression cuts MSE by **~16.5%** and explains **~7** more percentage points of variance (R²) than the plain linear baseline.
- Degree was fixed at 2; this is a lower bound on how much a non-linear model could improve on the linear baseline.

<br>

## Team

<div align="center">

[![Renad Alharthi](https://img.shields.io/badge/Renad_Alharthi-0A2F1F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RenadAlh)
[![Rawan Alahmadi](https://img.shields.io/badge/Rawan_Alahmadi-0A2F1F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/iiRawanj)
</a>

</div>



---

<div align="center">


If this helped you, consider starring the repository!

<a href="https://github.com/RenadAlh/CaliforniaHousingML">
  <img src="https://img.shields.io/badge/⭐_Star_this_repo-181717?style=for-the-badge&logo=github&logoColor=white" alt="Star this repository">
</a>

</div>
