<html><p><a href="https://loko-ai.com/" target="_blank" rel="noopener"> <img style="vertical-align: middle;" src="https://user-images.githubusercontent.com/30443495/196493267-c328669c-10af-4670-bbfa-e3029e7fb874.png" width="8%" align="left" /> </a></p>
<h1>Churn Predictions Demo</h1><br></html>

A simple example of training/predicting a classification model using the **Predictor** component.

The aim of the example is to predict whether customers are going to leave a mobile operator or not.

First of all, we have to create a new predictor from the **Predictors** tab using an *auto* model and *auto* transformer:
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/216644093-6ae902cd-c2e0-4b14-89c1-b6263f959d7e.png" width="80%" />
</p>

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

Let's go back to the churn predictions workflow:
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/216658256-3ad44211-4abc-4895-89ae-99820827628e.png" width="80%" />
</p>

The first pipeline reads the `churn_train` dataset, the **Selector** component exclude the features: `Day Charge`, 
`Eve Charge`, `Night Charge`, `Intl Charge`, `Phone`. The **Function** component, named "2Object", transforms the 
numerical feature `Area Code` to categorical. Finally, all data are sent to the **Predictor** component and used for 
the model training. Ones the model is fitted, you can open the **Predictors** tab and visualize the generated 
report:
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/216663690-b9dcc59d-86a4-4880-9123-bbf32417fcb6.png" width="80%" />
</p>

Everything is now ready. In the last pipeline, `churn_test` dataset uses the same preprocessing flow and is predicted by the `churn` model. 
<p align="center">
<img src="https://user-images.githubusercontent.com/30443495/216666900-d532b9a7-6808-4a02-8688-a22c0774b50c.png" width="80%" />
</p>
