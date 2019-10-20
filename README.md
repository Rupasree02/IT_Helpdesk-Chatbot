# IT_Helpdesk-Chatbot
IT_Helpdesk Chatbot helps to provide IT services for the end-users. 

Wouldn't it be awesome to have an accurate estimate of how long it will take for tech support to resolve your issue? In this project we will train a simple machine learning model for predicting helpdesk response time using BigQuery Machine Learning. We will then build a simple chatbot using Dialogflow and learn how to integrate trained BigQuery ML model with helpdesk chatbot. The final solution will provide an estimate of response time to users at the moment a request is generated!

1. Train a Model using BigQuery Machine Learning
2. Deploy a simple Dialogflow application
3. Use an inline code editor within Dialogflow for deploying a Node.js fulfillment script that integrates BigQuery
4. Testing chatbot

### Training a Model Using BigQuery Machine Learning

Upload Helpdesk Data to BigQuery.

Go to the Google Cloud Platform Console and verify your project is selected at the top.

Select your Project ID on the left sidebar, then create a new dataset called helpdesk. Leave Default selected as the location and click Create dataset.

Use the following parameters to create a new table. Leave the defaults for all other fields.

Create table from: Google Cloud Storage
Select file from GCS bucket: gs://solutions-public-assets/smartenup-helpdesk/ml/issues.csv
File format: CSV
Destination table issues
Auto detect: Check box for Schema and input parameters
Advanced > Header rows to skip: 1

Click Create Table. This will trigger a job that loads the source data into a new BigQuery table. It will take about 30 seconds for the job to complete, and you can view it by selecting Job History from the sidebar on the left.
