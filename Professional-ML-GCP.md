# Professional Machine Learning Engineer - GCP

https://medium.com/@furcifer/cracking-the-gcp-certified-professional-machine-learning-engineer-exam-in-a-week-966890073adf
https://sathishvj.medium.com/notes-from-my-google-cloud-professional-machine-learning-engineer-certification-exam-2110998db0f5

## Performance Metrics
- Accuracy
- F-score
- Recall
- Precision
- PR curves
- ROC
- AUC
- Confusion Matrix
- Micro-Averaging vs. Macro-Averaging - https://www.evidentlyai.com/classification-metrics/multi-class-metrics

## Loss Functions
- Cross-entropy
- Mean squared error
- Loss or log loss

| Loss Function                | Type                   | Formula / Definition                                           | When to Use | Key Characteristics | Comparison |
|------------------------------|------------------------|----------------------------------------------------------------|-------------|---------------------|------------|
| **Mean Squared Error (MSE)** | Regression            | \( MSE = \frac{1}{n} \sum (y_i - \hat{y}_i)^2 \)               | When large errors should be penalized more | Sensitive to outliers, amplifies large errors | Unlike MAE, squares errors, making it more sensitive to outliers |
| **Mean Absolute Error (MAE)** | Regression           | \( MAE = \frac{1}{n} \sum |y_i - \hat{y}_i| \)                | When all errors should be treated equally | Less sensitive to outliers, linear error weighting | Unlike MSE, does not emphasize larger errors |
| **Mean Squared Log Error (MSLE)** | Regression      | \( MSLE = \frac{1}{n} \sum (\log(1 + y_i) - \log(1 + \hat{y}_i))^2 \) | When predicting positive values and want relative errors | Penalizes under-predictions more | Unlike MSE, does not penalize large errors as much |
| **Huber Loss**               | Regression            | Quadratic for small errors, linear for large ones              | When robustness to outliers is needed | Hybrid of MSE and MAE | Unlike MSE, reduces sensitivity to large errors |
| **Log-Cosh Loss**            | Regression            | \( \sum \log(\cosh(y_i - \hat{y}_i)) \)                        | When smooth, robust loss is needed | Similar to MSE for small errors, MAE for large errors | Unlike Huber, does not need a threshold |
| **Binary Cross-Entropy (BCE)** or **Log Loss**| Binary Classification | \( -\frac{1}{n} \sum [ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) ] \) | When output is probability (0-1) | Measures classification probability accuracy | Unlike MSE, handles probability-based classification better |
| **Categorical Cross-Entropy (CCE)** | Multi-Class Classification | \( -\sum y_{ij} \log(\hat{y}_{ij}) \)                          | When classifying multiple classes | Works well with softmax activation | Unlike BCE, handles multi-class classification |
| **Sparse Categorical Cross-Entropy** | Multi-Class Classification | Similar to CCE but with integer labels                        | When labels are sparse (not one-hot encoded) | Optimized for sparse labels | Same as CCE but works directly with class indices |
| **Hinge Loss**               | Classification (SVM)  | \( \sum \max(0, 1 - y_i \hat{y}_i) \)                          | When using Support Vector Machines | Encourages confident classification with margin | Unlike cross-entropy, does not output probabilities |
| **KL Divergence**            | Probabilistic Models  | \( \sum P(x) \log \frac{P(x)}{Q(x)} \)                         | When comparing probability distributions | Measures how much one distribution differs from another | Unlike BCE, compares entire probability distributions |

## Activation Functions

## Bias vs. Variance: 
How to diagnose and address underfitting and overfitting in your models.

## Feature Engineering and Selection: 
- Selecting the right features and transforming raw data into a format that is better suited for ML models. 

- How to deal with missing data — do you remove it? do you compute it? In which scenarios would you take different approaches?
- Data Normalization — what is this and why would you use it?
- Transforming Data — numerical and categorical
- Feature crossing
- Binning vs Scrubing
- One-hot encoding, one-hot encoding hashed
-



## Model Evaluation: Besides performance metrics, familiarize yourself with different model evaluation techniques like K-fold cross-validation, train/test split, and bootstrapping.

## Ensemble Methods: 
- Bagging
- Boosting
- Stacking
- How these techniques can improve model performance by combining multiple models.

## Machine Learning Algorithms: 
- Linear regression
- Logistic regression
- Decision trees
- SVMs (Support Vector Machines)

## Deep Learning
- Epochs
- Learning Rate
- Dropout
- Batch normalization

### Example:
Suppose we have a dataset of **10,000** images, and we want to train our model with the following parameters:
- **Batch size**: 1000
= Number of **iterations**: 10
- Number of **epochs**: 50
- **Learning rate**: 0.01

In this scenario, we have **10 iterations per epoch** (**10,000** images / **1000** batch size). 
Over the course of **50 epochs**, our model will go through **500 iterations**. The **learning rate** of **0.01** is chosen to ensure smooth weight updates

https://www.geeksforgeeks.org/how-to-choose-batch-size-and-number-of-epochs-when-fitting-a-model/

## Vertex AI: 
- Pipelines
- AutoML
- Custom Training
- Endpoints
- Batch Prediction

- Kubeflow pipelines
- AI Platform
- Pub/Sub, Dataflow and pipelines, Dataprep 
- BigQuery 
- Data cleaning and validation: Dataprep, Dataflow, Dataproc

- Pipeline frameworks: Kubeflow vs. TFX

- Vertex AI pipeline - https://www.youtube.com/watch?v=H2kvI4zesPI&t=2s

- Vertex AI Model Monitoring
  - **Training-serving skew** occurs when the feature data distribution in production deviates from the feature data distribution used to train the model. If the original training data is available, you can enable skew detection to monitor your models for training-serving skew.
  - **Prediction drift** occurs when feature data distribution in production changes significantly over time. If the original training data isn’t available, you can enable drift detection to monitor the input data for changes over time.


- Getting Started with Kubeflow Pipelines (Hello World) - https://www.youtube.com/watch?v=JWjj4tttE04&t=2s
- Kubeflow Components/Pipelines (Hands-On Vertex AI Example) - https://www.youtube.com/watch?v=-spzhqAhz0Q
- Simplifying MLOps with Kubeflow (Metadata and Artifacts) in GCP - https://www.youtube.com/watch?v=UFzz4IskDoI
- DAGs: Kubeflow vs. Airflow (One Use Case, Built Twice) - https://www.youtube.com/watch?v=Yy6GJqYkY7Q&t=184s
- OpenAI Whisper Pipelines (Vertex AI, Kubeflow, JAX, GPUs,...) - https://www.youtube.com/watch?v=-mWwAUrN4F0
- Claude 3 Opus in ML Pipelines (Python & Kubeflow Example) - https://www.youtube.com/watch?v=VEjlxzvEV88&t=706s
- Using PaLM 2 LLM API in Data/ML Pipelines (Kubeflow Example) - https://www.youtube.com/watch?v=p5aaHOCFmjc
- Google Search API in Data/AI/ML Pipelines - https://www.youtube.com/watch?v=JWjj4tttE04https://www.youtube.com/watch?v=JWjj4tttE04
