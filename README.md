# IT_Helpdesk-Chatbot using Dialogflow
IT_Helpdesk Chatbot helps to provide IT services for the end-users. 

Wouldn't it be awesome to have an accurate estimate of how long it will take for tech support to resolve your issue? In this project we will train a simple machine learning model for predicting helpdesk response time using BigQuery Machine Learning. We will then build a simple chatbot using Dialogflow and learn how to integrate trained BigQuery ML model with helpdesk chatbot. The final solution will provide an estimate of response time to users at the moment a request is generated!

1. Train a Model using BigQuery Machine Learning
2. Deploy a simple Dialogflow application
3. Use an inline code editor within Dialogflow for deploying a Node.js fulfillment script that integrates BigQuery
4. Testing chatbot

![Capture](https://user-images.githubusercontent.com/56398068/67155491-59abfe80-f32e-11e9-880e-ecbafb7503c0.JPG)


### Training a Model Using BigQuery Machine Learning

Upload Helpdesk Data to BigQuery
Go to the Google Cloud Platform Console and verify your project is selected at the top.

Select BigQuery from the navigation menu in Google Cloud Console.

![image](https://user-images.githubusercontent.com/56398068/67155613-44d06a80-f330-11e9-93d2-543091bd8325.png)

Select your Project ID on the left sidebar, then create a new dataset called helpdesk. Leave Default selected as the location and click Create dataset.

In the sidebar, select the new helpdesk dataset you just created, and select Create Table.

Use the following parameters to create a new table. Leave the defaults for all other fields.

Create table from: Google Cloud Storage

Select file from GCS bucket: gs://solutions-public-assets/smartenup-helpdesk/ml/issues.csv

File format: CSV

Destination table issues

Auto detect: Check box for Schema and input parameters

Advanced > Header rows to skip: 1

Click Create Table. This will trigger a job that loads the source data into a new BigQuery table. It will take about 30 seconds for the job to complete, and you can view it by selecting Job History from the sidebar on the left.

In Query editor, execute the following query and examine the data.

SELECT * FROM `helpdesk.issues` LIMIT 1000
In the next query, we will use the data fields category & resolutiontime to build a machine learning model that predicts how long it will take to resolve an issue. The model type is a simple linear regression, and the trained model will be named predict_eta_v0 in our helpdesk dataset. The query takes about 1 minute to complete.

CREATE OR REPLACE MODEL `helpdesk.predict_eta_v0` 
OPTIONS(model_type='linear_reg') AS
SELECT
 category,
 resolutiontime as label
FROM
  `helpdesk.issues`
Run the following query to evaluate the machine learning model you just created. The metrics generated by this query tell us how well the model is likely to perform.

SELECT
  *
FROM
  ML.EVALUATE(MODEL `helpdesk.predict_eta_v0`)


Using the evaluation metrics, we can see that the model doesn't perform very well. When the r2_score and explained_variance metrics are close to 0, our algorithm is having difficulty distinguishing the signal in our data from the noise. We are going to use a few more fields during training and see if there is an improvement: seniority, experience & type. The final trained model will be named predict_eta. Run the query below.

CREATE OR REPLACE MODEL `helpdesk.predict_eta` 
OPTIONS(model_type='linear_reg') AS
SELECT
 seniority,
 experience,
 category,
 type,
 resolutiontime as label
FROM
  `helpdesk.issues`
Now, run the following query to evaluate the updated machine learning model you just created.

SELECT
  *
FROM
  ML.EVALUATE(MODEL `helpdesk.predict_eta`)
After adding the additional fields during training, we can see that our model has improved. When the metrics r2_score and explained_variance are close to 1, there is evidence to suggest that our model is capturing a strong linear relationship. We can also see that our *_error metrics are lower than before, which means our model will likely perform better.



Now we can execute a query to get a prediction of resolution time for a given scenario:

WITH pred_table AS (
SELECT
  5 as seniority,
  '3-Advanced' as experience,
  'Billing' as category,
  'Request' as type
)
SELECT
  *
FROM
  ML.PREDICT(MODEL `helpdesk.predict_eta`,
    TABLE pred_table)


When seniority is 5, experience is 3-Advanced, category is Billing, and type is Request, our model is saying that the average response time is 3.74 days.
