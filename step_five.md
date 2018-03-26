# Saving and deploying the model

## Saving the model

Once your training has completed and you have the model from [training](step_four.md) , now its time to save and deploy the model.

1. You can save the model by clicking on the 3 vertical dots next to your model and click `Save model`

![save_model_1](images/step_five/save_model_1.png)

2. Follow the steps and you will get a notification on the top saying `Model successfully saved. View model details here` You can follow the link.

![save_model_3](images/step_five/save_model_3.png)

3. Alternatively, you can go to the assets tabs of your project to to view all your saved models.

![save_model_4](images/step_five/save_model_4.png)

## Deploying and inferencing from the model

Once the model, the next step is to deploy or host the model.

1. Go to the saved model and you will see the   link to deploy the model `Add deployment`

![deploy_model_1](images/step_five/deploy_model_1.png)


2. Follow the steps and you will get to the screen of deploying model. The status of deploying model will change from `INITIALIZING` to `DEPLOY_SUCCESS`

![deploy_model_3](images/step_five/deploy_model_3.png)

3. Click on the name of the saved deployment and you will be directed to deployment details. You can click on the `Implementation` tab to get details on how to get to the model.

![deploy_model_4](images/step_five/deploy_model_4.png)

4. The above gives details on how your model is hosted and how you can get to your model using a REST endpoint and/or how to query this model using different clients. 


5. You can also quickly test your model by using the `Test` tab. For the mnist example, copy the contents of [this](scoring/wml_demo_5.json) file which is a tensor representation of the number.

![test_deploy_model_1](images/step_five/test_deploy_model_1.png)

### Further reading

1. [Overview: Deploying and scoring a deep learning model](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_model_deploy_score_ovr.html?context=analytics)
2. [Deploying and scroing models using CLI](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_tensorflow_deploy_score.html?context=analytics&linkInPage=true)