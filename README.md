# Finance-Domain-Expert-
Fine-tuning and Deploying Meta Llama 2 7B Model for Finance with Amazon SageMaker
# Introduction
In the financial sector, leveraging the power of large language models (LLMs) like Meta Llama 2 7B can significantly enhance tasks such as market analysis, investment predictions, and risk management. These models, initially trained on extensive datasets, require fine-tuning on domain-specific data to optimize their performance for specialized tasks. This project documents the journey of fine-tuning Meta Llama 2 7B on a financial dataset, deploying it using Amazon SageMaker, and evaluating its text generation capabilities.
# Table of Contents
Setup
Select Dataset
Fine-tune the Model
Deploy the Fine-tuned Model
Test the Fine-tuned Model
Cleanup
# Setup
Install Required Libraries
I ensured that the environment had the latest versions of the SageMaker and datasets libraries.
Authenticate AWS Services
Authenticated the session to access AWS services using the execution role associated with your SageMaker notebook instance.
import sagemaker, boto3, json
from sagemaker.session import Session

sagemaker_session = Session()
aws_role = sagemaker_session.get_caller_identity_arn()
aws_region = boto3.Session().region_name
sess = sagemaker.Session()
Test the Fine-tuned Model
Testing the fine-tuned model involves sending domain-specific queries to the endpoint and evaluating its responses.

# Define Response Function
Define a function to parse and print the model’s responses clearly.


# Select Dataset
For fine-tuning, choose a dataset that is specific to the financial domain. Ensure that your dataset is stored in an S3 bucket. Here’s an example path for a financial dataset.

Financial Domain: s3://your-bucket-name/training-datasets/finance
# Fine-tune the Model
Fine-tuning involves training the model on the financial dataset to tailor its knowledge for better performance in financial tasks.

Set Model and Estimator Parameters
First, define the model ID and version. Then, create an estimator with the required environment settings and hyperparameters.
# Specify the Dataset Path

Fill in the code below with the S3 path to your financial training dataset.
# Deploy the Fine-tuned Model
Once fine-tuning is complete, deploy the model to an endpoint for inference.

Test the Fine-tuned Model
Testing the fine-tuned model involves sending domain-specific queries to the endpoint and evaluating its responses.

Define Response Function
Define a function to parse and print the model’s responses clearly.

python
Copy code
def print_response(payload, response):
    print(payload["inputs"])
    print(f"> {response}")
    print("\n==================================\n")
Send a Query to the Model
Invoke the fine-tuned model with a financial domain query to evaluate its performance.


payload = {
    "inputs": "The investment tests performed indicate",
    "parameters": {
        "max_new_tokens": 64,
        "top_p": 0.9,
        "temperature": 0.6,
        "return_full_text": False,
    },
}
    response = finetuned_predictor.predict(payload, custom_attributes="accept_eula=true")
    print_response(payload, response)
    
# Cleanup
To avoid unnecessary costs, delete the model deployment after completing the evaluation.
# Delete the SageMaker endpoint and the attached resources
finetuned_predictor.delete_model()
finetuned_predictor.delete_endpoint()
Verify Deletion
Ensure the endpoint is deleted by checking the SageMaker dashboard under 'Inference' > 'Endpoints'. If the endpoint still exists, manually delete it via the dashboard.

# Conclusion
In this project, we embarked on the journey of fine-tuning Meta Llama 2 7B to enhance its performance in the financial domain. From setting up the environment to selecting a dataset, fine-tuning the model, deploying it, and finally evaluating its performance, each step was meticulously detailed to ensure a comprehensive understanding. This process not only enhances the model’s capabilities in financial tasks but also showcases the power of transfer learning in adapting large language models for specialized applications.

