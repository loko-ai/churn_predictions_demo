<html><p><a href="https://loko-ai.com/" target="_blank" rel="noopener"> <img style="vertical-align: middle;" src="https://user-images.githubusercontent.com/30443495/196493267-c328669c-10af-4670-bbfa-e3029e7fb874.png" width="8%" align="left" /> </a></p>
<h1>Churn Predictions Demo</h1><br></html>

A simple example of training/predicting a classification model using the **Predictor** component.

The aim of the example is to predict whether customers are going to leave a mobile operator or not.

From the **Projects** tab, click on **Import from git** and copy and paste the URL of the current page 
(i.e. https://github.com/loko-ai/churn_predictions_demo):
<p align="center"><img src="https://user-images.githubusercontent.com/30443495/230620716-c67e5e58-b71e-4817-9213-39a7e0e9cddd.gif" width="80%" /></p>

### STEP1: Model training

First of all, you have to create a new predictor from the **Predictors** tab using an *auto* model and *auto* transformer:
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231456937-c88df6c7-8330-4b6d-9ce8-d6286a5cd3bd.gif" width="80%" />
</p>

You can then go back to your project and run it.

In order to start the project remember to press the play button on the right of the project's name:

<img src="https://user-images.githubusercontent.com/30443495/231462416-11fc90d1-045f-45c6-be4c-b5b0664f493c.gif" width="80%" />

The **churn** dataset is already split into a train and test dataset and has the following features:

`State:` the US state in which the customer resides, indicated by a two-letter abbreviation; for example, OH or NJ

`Account Length:` the number of days that this account has been active

`Area Code:` the three-digit area code of the corresponding customer’s phone number

`Phone:` the remaining seven-digit phone number

`Int’l Plan:` whether the customer has an international calling plan: yes/no

`VMail Plan:` whether the customer has a voice mail feature: yes/no

`VMail Message:` the average number of voice mail messages per month

`Day Mins:` the total number of calling minutes used during the day

`Day Calls:` the total number of calls placed during the day

`Day Charge:` the billed cost of daytime calls

`Eve Mins, Eve Calls, Eve Charge:` the billed cost for calls placed during the evening

`Night Mins, Night Calls, Night Charge:` the billed cost for calls placed during nighttime

`Intl Mins, Intl Calls, Intl Charge:` the billed cost for international calls

`CustServ Calls:` the number of calls placed to Customer Service

`Churn?:` whether the customer left the service: true/false

You can fit the churn model:

<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231460882-1119571a-de00-42d3-a838-18e8246238a5.png" width="80%" />
</p>

The **CSV Reader** component reads the `churn_train` dataset, the **Selector** component exclude the features: `Day Charge`, 
`Eve Charge`, `Night Charge`, `Intl Charge`, `Phone`. The **Function** component, named "2Object", transforms the 
numerical feature `Area Code` to categorical. Finally, all data are sent to the **Predictor** component and used for 
the model training. Ones the model is fitted, you can open the **Predictors** tab and visualize the generated 
report:
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231480367-4f7aed74-18f2-42d8-ab64-9bfcda92e6cc.gif" width="80%" />
</p>


### STEP2: Model prediction

In the second pipeline, `churn_test` dataset is preprocessed in almost the same way, this time you have to exclude also
the `Churn?` variable, and it's predicted by the `churn` model.

<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231482085-d739567d-468d-44a6-a28f-b4d58e34ee9d.png" width="80%" />
</p>

The output shows each input object and its predicted label:

<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231485560-02e9d29e-a3dd-48fa-8092-25d929ff4dee.png" width="80%" />
</p>

### STEP3: Expose service

Finally, you can expose a service to obtain the churn predictions.

<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231498243-866d0241-0a46-4ad5-a860-405dec5ce2fc.png" width="80%" />
</p>

Use **Route** and **Response** components at the beginning and at the end of the flow producing the output. 
In the **Route** configuration name the service predict and copy the complete url 
(i.e. http://localhost:9999/routes/orchestrator/endpoints/churn_predictions_demo/predict). In the **Response** component 
set the *Response Type* to json.

The **Function** component extracts the body from the received request, preprocessing is applied and the **Predictor** 
component predicts the output.

You can test it using:

```
curl -d "{\"State\":\"NC\",\"Account Length\":125,\"Area Code\":786,\"Phone\":\"758-8060\",\"Int'l Plan\":\"yes\",\"VMail Plan\":\"no\",\"VMail Message\":0,\"Day Mins\":7.364831620290008,\"Day Calls\":2,\"Day Charge\":4.443728993845921,\"Eve Mins\":3.4439530293579668,\"Eve Calls\":0,\"Eve Charge\":6.914081165830165,\"Night Mins\":4.5034706152237955,\"Night Calls\":300,\"Night Charge\":6.78182020513275,\"Intl Mins\":4.0490777782370255,\"Intl Calls\":7,\"Intl Charge\":0.20388610691303732,\"CustServ Calls\":9,\"Churn?\":\"False.\"}" -X POST http://localhost:9999/routes/orchestrator/endpoints/churn_predictions_demo/predict
```

### STEP4: Test service

You can test the predict service directly in your flow using the **HTTP Request** component:

<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/231505219-498a72b6-b784-47a6-a675-c4bf2577e063.png" width="80%" />
</p>

In this case you have to change http://localhost:9999 to http://gateway:8080 since the request will be executed in one 
of the containers inside the loko network (i.e. 
http://gateway:8080/routes/orchestrator/endpoints/churn_predictions_demo/predict).

The **Sampler** component samples only 2 rows from the `churn_test` dataset which are predicted by the *predict* service. 