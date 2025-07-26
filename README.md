# Cardekho Used Car Price Prediction

This project aims to predict the selling price of used cars using data obtained from [Cardekho.com](https://www.cardekho.com/). The project follows a standard machine learning workflow, including data cleaning, exploratory data analysis (EDA), feature engineering, and model training. The final model is an **AdaBoost Regressor** that has been fine-tuned using hyperparameter optimization to achieve the best performance.

## 📊 Dataset Features

The dataset used contains various technical and situational attributes that influence the price of a car.

| Feature Name | Description | Data Type |
| :--- | :--- | :--- |
| `car_name` | The full name of the car (e.g., Maruti Alto). | Categorical |
| `brand` | The brand of the car (e.g., Maruti). | Categorical |
| `model` | The model of the car (e.g., Alto). | Categorical |
| `vehicle_age` | Age of the car in years. | Numerical |
| `km_driven` | The total kilometers driven by the car. | Numerical |
| `seller_type` | The type of seller (e.g., Individual, Dealer). | Categorical |
| `fuel_type` | The fuel type of the car (e.g., Petrol, Diesel). | Categorical |
| `transmission_type` | The transmission type (e.g., Manual, Automatic). | Categorical |
| `mileage` | The fuel efficiency of the car (km/l or km/kg). | Numerical |
| `engine` | The engine displacement in CC. | Numerical |
| `max_power` | The maximum power of the engine in BHP. | Numerical |
| `seats` | The number of seats in the car. | Numerical |
| `selling_price` | The selling price of the car in INR. | **Numerical (Target Variable)** |

---

## ⚙️ Project Workflow

The project workflow is broken down into several key stages:

### 1. Data Cleaning and Preprocessing

- **Removal of Redundant Columns:** The `Unnamed: 0` column, which was an artifact of the CSV export, was dropped.
- **Handling Duplicate Entries:** The dataset contained 167 duplicate rows, which were identified and removed to prevent bias in the model.
- **Anomaly Detection and Correction:** It was observed that some entries in the `seats` column had a value of '0', which is physically impossible. These were corrected to '5', a common value for passenger cars.

### 2. Exploratory Data Analysis (EDA)

- **Outlier Detection and Management:**
    - Extreme outliers were identified in the `selling_price` and `km_driven` columns. Cars with selling prices above 15,000,000 INR and kilometers driven over 1,000,000 km were filtered out to create a more robust model.
- **Visualization of Feature Relationships:**
    - **Scatter plots** were used to analyze the relationship between `vehicle_age`, `km_driven`, and `selling_price`.
    - **Box plots** were generated to understand the distribution of `selling_price` across different categorical features like `fuel_type`, `transmission_type`, and `seats`. This helped visualize how these factors impact the price.
- **Correlation Analysis:** A correlation matrix for numerical features was created, revealing a strong positive correlation between `selling_price` and `max_power` (0.77).

### 3. Feature Engineering

- **Encoding Categorical Features:** To prepare the data for the model, categorical variables were converted into a numerical format using two strategies based on their cardinality:
    - **One-Hot Encoding:** Applied to features with low cardinality (`seller_type`, `fuel_type`, `transmission_type`). This method creates binary columns for each category without implying any ordinal relationship.
    - **Frequency Encoding:** Applied to features with high cardinality (`car_name`, `brand`, `model`). This method replaces each category with its frequency in the training set, which is a simple way to capture the importance of a category without creating an excessive number of new features.

### 4. Model Training and Evaluation

- **Baseline Model:** An initial **AdaBoost Regressor** was trained on the preprocessed data. This baseline model achieved an R² score of approximately **0.61**.
- **Hyperparameter Tuning:** To improve performance, `RandomizedSearchCV` was used to find the optimal hyperparameters for the AdaBoost model. The search space included:
    - `n_estimators`:
    - `learning_rate`: [0.001, 0.01, 0.1, 1.0, 2.0]
    - `loss`: ["linear", "square", "exponential"]
- The best-performing combination was found to be `n_estimators=50`, `loss='exponential'`, and `learning_rate=0.1`.

---

## 📈 Results

The performance of the AdaBoost Regressor improved significantly after hyperparameter tuning.

| Model | R² Score | Mean Squared Error (MSE) | Mean Absolute Error (MAE) |
| :--- | :--- | :--- | :--- |
| **Baseline AdaBoost** | 0.613 | 191,378,973,367 | 338,926 |
| **Tuned AdaBoost** | **0.748** | **129,335,169,666** | **224,023** |

The tuned model demonstrated a **22% improvement** in the R² score, indicating a much better fit to the data and more accurate predictive power.

## 🏁 Conclusion

This project successfully developed a machine learning model to predict the price of used cars. The key takeaways are:
- Thorough data cleaning and EDA are crucial for identifying and handling issues like duplicates, anomalies, and outliers, which directly impact model performance.
- Feature engineering, particularly the strategic encoding of categorical variables, is essential for preparing the dataset for regression models.
- Hyperparameter tuning via `RandomizedSearchCV` proved highly effective, substantially boosting the AdaBoost Regressor's R² score from 0.61 to 0.75.
- The analysis confirmed that `max_power` and `engine` size are strong predictors of a car's selling price, as indicated by the correlation matrix.
