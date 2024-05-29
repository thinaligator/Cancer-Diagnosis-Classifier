```markdown
# Developing a Classifier to Assist in Diagnosing the Type of Breast Cancer Using the Breast Cancer Wisconsin (Diagnostic) Dataset

The dataset contains patient data consisting of 10 real-valued attributes:
1. Radius
2. Texture
3. Perimeter
4. Area
5. Smoothness
6. Compactness
7. Concavity
8. Concave points
9. Symmetry
10. Fractal Dimension

For each area, the following features were calculated:
- Mean
- Standard Error
- Worst Value (mean of the three largest values)

This gives a total of 30 features.

It is also necessary to check the data for any missing columns. All columns are filled, so there is no need to remove any.

All existing instances are labeled, describing the cancer diagnosis as either benign or malignant. Among the 569 records, there are 357 (62.74%) cases of benign cancer and 212 (37.26%) cases of malignant cancer, and we should expect a similar proportion of labels when testing the model.

Although the dataset contains 30 columns, they are all related, as they all store information about the same 10 attributes.

The matrix contains previously loaded data. There is a correlation between many of them (the more red the color, the higher the correlation), such as:
- The `mean_radius` column with the `mean_perimeter` (1), `mean_area` (0.99), and `worst_radius` (0.97) columns, as well as `worst_perimeter` (0.97).

In the case of `mean_perimeter` and `mean_area`, this is likely the result of very similar information contained in the columns (tumor size). In further analysis, only one of the three columns should be selected. This correlation is also noticeable between the remaining columns listed above and is due to the fact that the "worst" columns are a subset of the "mean" columns. This is a reason to reject the "worst" columns and use the "mean" columns. Unfortunately, it was not possible to remove both types of correlation, as the large number of errors during feature removal prevented this.

## Tools Used
To create the best possible model, I will use various tools provided by the Scikit-learn library. These include:
1. Tools for evaluating model quality such as:
    - Prediction probability
    - Accuracy score
    - Confusion matrix
    - Cross-validation
2. Polynomial features

## Simulation Studies
In the initial stage, I checked whether the data required standardization. To do this, I analyzed the information regarding the first patient and calculated the mean values and standard errors for all features across all cases.

Standardization was necessary, for which I used `StandardScaler`.

The initial stage of building was to split the loaded data into disjoint sets (train and test). The ratio of these sets is 9:1.

Next, I used logistic regression, which in the SciKitLearn library is an uncomplicated task thanks to a special class dedicated to this operation. After creating an instance of this class, I used the `fit()` method, providing training data (both data and corresponding labels). This process resulted in determining the decision boundary for the provided data.

## Model Testing
I verified the effectiveness of the model by presenting predicted values for four randomly selected instances from the test data and then comparing them with the corresponding labels. In each case, the model accurately predicted the outcomes.

## Model Evaluation
Now, I move on to evaluating the model. For this purpose, I use the accuracy score and the confusion matrix. To estimate accuracy, I use the `accuracy_score()` function, which evaluates the correctness of predictions for all test data. The model showed a consistency level of nearly 98%.

The confusion matrix for 57 test instances showed 1 incorrect prediction by the model. All these errors involved incorrectly assigning the "1" label. This result is consistent with the accuracy score (56/57).

I also checked the model using cross-validation for different data split amounts. The results of analyzing different algorithm parameters are very similar, likely due to the dataset size. The best results were obtained with a split into 2 sets. In many cases, during multiple algorithm runs, this split achieved the highest scores.

Additionally, I tested the model for overfitting. For this, I used polynomial features, transforming the data from linear to polynomial, and then analyzing the mean squared error and variance score for different polynomial degrees.

Modifying the polynomial degree from 1 to 6 had minimal impact on the model. This may be due to the visible area on the graph where objects of both classes are densely packed. Clearly defining a decision boundary that improves the model's precision would require using a higher-order polynomial. However, I did not opt for this solution due to the relatively high cost of this approach compared to the minimal improvement in the model and the concern of overfitting.

## Conclusions
The model was constructed correctly, as confirmed by the model evaluation tools. It assigns the correct labels to over 90% of the test data. During the work, certain difficulties were noticeable related to creating the model based on the used dataset, mainly due to the small number of instances. As a result, different model creation attempts could lead to slightly different results for the same operations. However, these differences were not significant, and the model's effectiveness always remained around 98%.

The most promising strategy for improving the model's quality was to remove unnecessary features, which, due to strong correlations among them, only complicated the process of determining the appropriate decision boundary. This could potentially have been achieved by eliminating certain data at the analysis stage or using recursive feature elimination. Unfortunately, both methods generated numerous errors, so they were not used.
```
