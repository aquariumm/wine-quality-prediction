# wine-quality-prediction
Experiments with different Machine Learning models (Random Forest, XGBoost, LightGBM, SVM, Logistic Regression) and feature engineering techniques to predict wine quality using the UCI dataset.

I recently ran an experiment with the Wine Quality dataset (UCI), an imbalanced multi-class classification problem. I expected advanced models such as XGBoost or LightGBM to dominate. After all, they’re cutting-edge ensemble models widely regarded as “state-of-the-art”.

What if I told you a Random Forest beat XGBoost and LightGBM on this Wine Quality dataset?

Dataset Characteristics – Imbalanced Class Distribution

Exploratory Data Analysis

Maximize image
Edit image
Delete image


The Wine Quality dataset suffers from severe class imbalance, which has a major impact on model performance:

Two dominant classes: quality 6 wines account for 43.7% of the dataset, while quality 5 wines make up another 32.9%. Together, they cover more than three quarters of all samples.
Rare edge cases: some classes are extremely underrepresented, such as quality 3 (0.5%) and quality 9 (0.08%). With so few examples, it can be anticipated a very challenging task for models to learn meaningful decision boundaries for these grades.
Why It Matters

Illusion of strong performance: this is about model evaluation issues. A model biased toward predicting 5 and 6 can achieve deceptively high accuracy, even if it completely makes wrong predictions on minority classes.
Unreliable generalization: this is about model reliability issues. A model trained on the raw dataset will likely produce skewed predictions, so even if the model looks good during training and testing on this dataset, it might not generalize well in practice. Think about such a model is used on rare-quality wines (e.g. 7, 8, 9), it will perform poorly because it hasn’t been trained with enough data containing rare quality wines.
Measurements

Here are the key metrics I used to evaluate a model’s performance in an attempt to alleviate imbalanced classes.

Macro F1 (Most important here)

This score balances performance across all classes equally, regardless of how many wines are in each class
Why important: Ensures the model isn’t just doing well on the majority (class 5, 6) but ignoring minority classes (class 3, 9)
Weighted F1

This measures performance averaged across classes, weighted by class size
Why important: Reflects how well the model does overall, but due to it favors majority classes, it tends to hide poor performance on rare grades (if the model predicts well on the major classes)
Cross-Validation (Mean & Std F1)

Cross-validation ensures that results are robust across different train/test splits
Low Std = consistent performance, High Std = unstable predictions
Models Tested

Maximize image
Edit image
Delete image


Result

Minimize image
Edit image
Delete image


Maximize image
Edit image
Delete image


Winning Model

Random Forest with Polynomial Features

RF with Poly Performance Overview

Macro F1: 0.411
Weighted F1: 0.685
CV Mean F1: 0.383
CV Std F1: 0.020
Why this model stands out

Best Macro F1 (Fairness across classes)

Macro F1 is the most reliable metric in this imbalanced multi-class setting because it treats all quality classes equally including the rare ones (e.g., quality 3 and 9 wines). Random Forest with polynomial features achieves the highest Macro F1, proving it balances performance across both majority (5 and 6) and minority classes better than any other model.

Strong Weighted F1 (Overall accuracy)

Its Weighted F1 is also the highest. Since Weighted F1 is dominated by the frequent classes, this result confirms that the model performs strongly on the common wine qualities while still maintaining a reasonable balance with rare ones.

Robustness and stability

CV Mean F1 (0.383): the model maintains solid balanced performance across different folds. A proof that it generalizes reliably to unseen data
CV Std F1 (0.020): very small variation across folds demonstrates that its predictions are stable and not overly sensitive to data splits
Impact of polynomial features (feature engineering)

Polynomial transformations allow Random Forest to model non-linear interactions between chemical properties (e.g. acidity × alcohol, sulphates²). This richer feature space enhances its ability to capture complex patterns in wine quality, pushing its performance ahead of more advanced gradient boosting models.

Close Contenders

LightGBM (Poly+Log)

CV Mean F1 = 0.415  the best generalization across folds
However, its Macro F1 (0.381) and Weighted F1 (0.646) fall short, suggesting weaker performance on minority classes compared to Random Forest
XGBoost (Poly+Log, Weighted) and Random Forest (Poly+Log)

Both perform strongly, but slightly trail behind in Macro and Weighted F1
Key Takeaway

It’s not always about using the most advanced model. Sometimes, applying the right feature engineering and choosing the correct evaluation metric makes a bigger difference than jumping to the latest algorithm.

In this case, technique beat complexity.

Question for you

Have you seen a “simpler” model outperform more advanced ones? Would like to hear your thoughts.
