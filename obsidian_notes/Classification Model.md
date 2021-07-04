# Classification Model
Type of [[Supervised Machine Learning]] algorithms which focuses on predicting a label, could be binary (0,1) or multiple (0,1,2,3). Every class is given an equal importance, which has a problem with [[Unbalanced Datasets]]. 
## Types
Classification models which I'm familiar with:
 - Decision Tree: divide the observations according to either the [[Gini Index]] or [[Entropy]] or information gain. Produces an easy-to-interpret tree that divides the dataset into nodes which are labeled accordingly. 
 - Random Forest:  Combination of multiple decision trees to avoid problems with starting points. Better performance than decision trees but less interpretability. 
 - Boosted Forest: Similar to Random Forest, but instead of having randomly run multiple decision trees, it has some type of Gradient Boosting to learn from each decision tree. Outputs the 'best iteration' decision tree which is a Decision Tree.
 - Logistic Regression: type of regression that predicts an categorical value. It can be extended to multiple categories (multinomial) or even ordinal values. Good for non-categorical features as it assumes some type of lineal relationship between the features and label. 
## Performance Metrics
- Accuracy: Number of correctly classified data points over total data points.
- Confusion Matrix: Table with True Positive, True Negative, False Positive and False Negative values. 
- Precision: True Positive / (True Positive + False Positive), correct _labeled_ truths over total _labeled_ truths. Important when one of the classes is more important than the other.
- Recall: True Positive / (True Positive + False Negative), correctly _labeled_ truths over total _actual_ truths. Important when missing one correct classification is worse than qualifying wrongly.
- F1 Score: 2 * (Precision * Recall) / (Precision + Recall). Harmonic mean of Precision and Recall, combines both measures.
- False Positive Rate: False Positive / (False Positive + True Negative), used for the AU-ROC curve.
- True Positive Rate: True Positive / (True Positive + False Negative), used for the AU-ROC Curve
- AU-ROC: Area Under the Receiver Operating Curve. Chart with the False Positive Rate as x-axis and True Positive Rate as y-axis and actual scores at different classification thresholds. 
## Packages & Code

