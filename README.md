# telco-churn-aws-sagemaker
End-to-end Machine Learning project to predict customer churn using Python, XGBoost, Amazon S3 and Amazon SageMaker.

The project covers the complete Machine Learning lifecycle:

Data preprocessing → S3 → SageMaker Training → Model Evaluation → Model Deployment → Real-Time Inference

Project Overview

Customer churn is an important business problem in the telecommunications industry.

The objective of this project is to predict which customers are more likely to leave the company, allowing the business to identify high-risk customers and potentially implement retention strategies.

The model was trained using XGBoost through an Amazon SageMaker Training Job and subsequently deployed as a real-time SageMaker Endpoint.

Architecture

Amazon S3

↓

Data Preprocessing

↓

Processed Train / Validation / Test

↓

SageMaker Training Job

↓

XGBoost

↓

Model Artifact

↓

SageMaker Model

↓

SageMaker Endpoint

↓

Real-Time Inference

Technologies

Programming & Machine Learning

Python
Pandas
NumPy
Scikit-learn
XGBoost
Jupyter Notebook

AWS

Amazon S3
Amazon SageMaker Studio
SageMaker Training Jobs
SageMaker Model
SageMaker Endpoint
AWS IAM
Amazon ECR
Dataset

The project uses the Telco Customer Churn dataset.

The dataset contains customer information including:

Gender
Senior Citizen
Partner
Dependents
Tenure
Phone Service
Internet Service
Contract
Payment Method
Monthly Charges
Total Charges
Churn
Target Variable

The target variable is:

Churn

It was encoded as:

0 = No Churn

1 = Churn

Data Preprocessing

The dataset was processed using Python and Pandas.

The preprocessing workflow included:

Data inspection
Data cleaning
Missing value handling
Data type conversion
Feature transformation
Categorical variable encoding
Target encoding
Train / validation / test split

The resulting datasets were uploaded to Amazon S3.

Amazon S3 structure:

Raw/

WA_Fn-UseC_-Telco-Customer-Churn.csv

Processed/

train.csv

validation.csv

test.csv

Amazon S3

Amazon S3 was used as the storage layer for the Machine Learning workflow.

The processed datasets were stored in S3 and consumed by the SageMaker Training Job.

Example:

s3://<bucket>/Processed/train.csv

s3://<bucket>/Processed/validation.csv

s3://<bucket>/Processed/test.csv

The trained model artifact was stored in S3:

s3://<bucket>/Models/<training-job>/output/model.tar.gz

SageMaker Training

The model was trained using an Amazon SageMaker Training Job.

Algorithm:

XGBoost

XGBoost Version:

1.7-1

The Training Job consumed the datasets stored in Amazon S3.

Training workflow:

train.csv

↓

SageMaker Training Job

↓

XGBoost

↓

model.tar.gz

The resulting model artifact was automatically stored in Amazon S3.

Model Evaluation

The model was evaluated using the test dataset.

Performance:

ROC-AUC: 0.8491

Accuracy: 78.81%

Precision: 56.01%

Recall: 66.95%

F1-Score: 60.99%

Classification Report
          precision    recall  f1-score   support

0 0.88 0.83 0.85 1058

1 0.56 0.67 0.61 348

accuracy 0.79 1406

macro avg 0.72 0.75 0.73 1406

weighted avg 0.80 0.79 0.79 1406

Confusion Matrix
             Predicted

             0       1

Actual 0 875 183

Actual 1 115 233

The model correctly identified 233 of 348 customers who actually churned.

This resulted in a 66.95% recall for the Churn class.

Business Interpretation

For a churn prediction problem, Recall is an important metric because missing a customer who is actually going to churn can represent a potential business opportunity lost.

The model achieved:

Recall = 66.95%

This means that the model identified approximately two-thirds of the customers who actually churned.

A telecommunications company could use these predictions to prioritize customers for potential retention strategies.

Possible business actions could include:

Personalized offers
Customer retention campaigns
Contract upgrades
Loyalty programs
Targeted communications
Customer support intervention
Model Deployment

After training and evaluation, the model was deployed using Amazon SageMaker.

The model artifact:

model.tar.gz

was used to create a SageMaker Model and deploy it as a real-time endpoint.

Deployment workflow:

Amazon S3

↓

model.tar.gz

↓

SageMaker Model

↓

SageMaker Endpoint

↓

XGBoost Inference

Endpoint Configuration

Instance Type:

ml.m5.large

Endpoint status:

InService

Endpoint name:

telco-churn-xgboost-endpoint

Real-Time Inference

The deployed endpoint was tested from a separate Jupyter notebook.

The inference workflow was:

Customer Data

↓

CSV Payload

↓

SageMaker Runtime

↓

SageMaker Endpoint

↓

XGBoost

↓

Churn Probability

Example:

import boto3

runtime = boto3.client(
"sagemaker-runtime",
region_name="us-east-2"
)

response = runtime.invoke_endpoint(
EndpointName="telco-churn-xgboost-endpoint",
ContentType="text/csv",
Body=payload
)

result = response["Body"].read().decode("utf-8")

print(result)

The endpoint returns the predicted probability.

The probability can be converted into a binary prediction:

probability = float(result)

prediction = int(probability >= 0.5)

print("Probability:", probability)

print("Prediction:", prediction)

Where:

0 = No Churn

1 = Churn

AWS IAM

AWS IAM was used to provide the required permissions for SageMaker.

The SageMaker execution role was used to access Amazon S3 and execute the required SageMaker operations.

Required permissions included access to:

Amazon S3
SageMaker Training Jobs
SageMaker Models
SageMaker Endpoints

No AWS credentials are stored in this repository.

Security

Sensitive AWS information is intentionally excluded from the repository.

Do not commit:

.aws/

.env

credentials

*.pem

*.key

AWS Access Keys and Secret Access Keys should never be hard-coded into notebooks or source code.

Project Workflow
Load Dataset
Data Preprocessing
Train / Validation / Test Split
Upload Data to Amazon S3
Create SageMaker Training Job
Train XGBoost Model
Evaluate Model
Store Model Artifact in S3
Create SageMaker Model
Deploy SageMaker Endpoint
Real-Time Inference
Key Learnings

This project provided practical experience with:

End-to-end Machine Learning workflows
Binary classification
XGBoost
Data preprocessing
Model evaluation
ROC-AUC
Precision and Recall
Confusion Matrix
Amazon S3
SageMaker Training Jobs
SageMaker Models
SageMaker Endpoints
Real-time inference
AWS IAM
Cloud-based Machine Learning deployment
Future Improvements

Potential improvements for future versions include:

Hyperparameter optimization
SageMaker Automatic Model Tuning
SageMaker Pipelines
SageMaker Model Registry
MLflow experiment tracking
SHAP model explainability
Feature importance analysis
Data drift monitoring
Model monitoring
Automated retraining
CI/CD pipeline
FastAPI REST API
Docker containerization
Project Results

Machine Learning:

Algorithm: XGBoost

ROC-AUC: 0.8491

Accuracy: 78.81%

Precision: 56.01%

Recall: 66.95%

F1-Score: 60.99%

AWS:

Amazon S3

Amazon SageMaker

AWS IAM

Amazon ECR

Deployment:

SageMaker Training Job

↓

SageMaker Model

↓

SageMaker Endpoint

↓

Real-Time Inference

Author

Julio C. Valderrabano

Data Scientist | Machine Learning | Python | SQL | AWS

GitHub:

https://github.com/jcvhrocker-glitch
