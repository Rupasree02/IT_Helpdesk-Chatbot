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

