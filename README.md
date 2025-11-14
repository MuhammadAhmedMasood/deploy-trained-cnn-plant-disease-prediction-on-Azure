# 🌿 Plant Disease Prediction — Azure Deployment

This repository contains all the files and configuration required to deploy a trained CNN-based Plant Disease Prediction model on Microsoft Azure.
The deployment is done using Azure Machine Learning, Managed Online Endpoints, and a fully specified Conda environment.

# 📁 Repository Structure
.
├── deploy_trained_cnn_plant_disease_prediction-on_azure.py
├── config.json
├── conda-env.yml
├── score.py
├── class_indices.json
├── sample-request.json
├── test_apple_black_rot.JPG
└── README.md

# Main Files

deploy_trained_cnn_plant_disease_prediction-on_azure.py
Main script used to deploy the trained CNN model.
Originally written in Google Colab and exported as a .py file.
It installs all required libraries, loads Azure config, creates the environment, endpoint, deployment, and runs a sample test request.

config.json
Contains Azure credentials:

subscription_id

resource_group

workspace

location

Users must fill these values using their own Azure ML workspace details.

conda-env.yml
Conda environment configuration used by the Azure ML deployment.
This file defines dependencies required for scoring and inference.

score.py
The scoring script used by Azure ML to process inference requests, load the model, perform prediction, and return results.

class_indices.json
Maps class names to numerical indices used in model training.

sample-request.json
Sample request file used to test the deployed endpoint after deployment.

test_apple_black_rot.JPG
Example image for local or cloud testing.

# 🚀 Deployment Overview

The deployment follows Azure ML's Managed Online Endpoint workflow.

1. Load Azure Configuration

The script reads from config.json and initializes the Azure ML workspace using:

from azure.ai.ml import MLClient

2. Create an Online Endpoint

A new endpoint is generated using:

from azure.ai.ml.entities import ManagedOnlineEndpoint


This endpoint acts as the API entry point for predictions.

3. Define Environment Using Conda

The deployment uses a custom environment defined in conda-env.yml.
Azure creates this environment automatically when deploying the model.

4. Create a Deployment (Blue Deployment)

A deployment named blue is created using:

ManagedOnlineDeployment(...)


The trained CNN model, scoring script, class index file, and environment are included in this deployment.

5. Invoke Endpoint for Testing

A simple base64 image test is performed after deployment.
Example code used:

import base64, json

with open('test_apple_black_rot.JPG', 'rb') as image_file:
    image_data = image_file.read()

image_base64 = base64.b64encode(image_data).decode('utf-8')

json_request = json.dumps({'data: image_base64'})

with open('sample-request.json', 'w') as json_file:
    json_file.write(json_request)

response = ml_client.online_endpoints.invoke(
    endpoint_name=endpoint_name,
    deployment_name="blue",
    request_file="sample-request.json"
)


This verifies that the endpoint, environment, and scoring pipeline work correctly.

# 🔗 Model & Training Details

The model used in this deployment is trained separately in my other repository:
👉 plant-diseases-prediction-using-CNN

(Replace link with your actual repo)

That repository contains:

Full CNN training code

Dataset structure

Preprocessing methods

Model saving and metrics

# 🧪 Testing

A sample image (test_apple_black_rot.JPG) and a sample-request.json are included for quick validation of the endpoint.

# 📌 Notes for Users

Update config.json with your Azure subscription, resource group, workspace name, and region.

Ensure your Azure ML workspace is configured properly.

Use the included conda-env.yml to create the environment for deployment.

Modify paths in the Python script if needed for your directory structure.
