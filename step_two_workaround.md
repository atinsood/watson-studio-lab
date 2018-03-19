# Creating an experiment

## Linking your experiment with IBM Watson Machine Learning

1. Now that you have a project, the next step is to create an experiment. Go to `Projects` tab and select the project that you had created from the dropdown. Once in the project page, you should be able to see various tabs including `Overview` , `Assets` and so on. Select the `Assets` tab and go to the `Experiments` section towards the bottom of the page and click on `New Experiment`.

2.In order for you to create an instance, you will need to create a Watson Machine Learning instance as well. [IBM WML](https://www.ibm.com/cloud/machine-learning) is a comprehenive solution that is your gateway to various machine and deep learning technologies.

3.If you have existing Waton Machine Learning instance then you should be able to click on the `Reload` button and associate that account and skip step 4.

4a.If not, click on the `Associate a Machine Learning service instance` link and follow the steps to create an instance.

4b. Select the `Lite` plan and click on create. Once created close the new tab that had opened up and come back to the [experiment assistant](https://dataplatform.ibm.com/ml/experiments/new-experiment?context=analytics)

5.You should be able to click on the `Reload` button for the Machine Learning instance and your instance for Machine Learning should show up for selection.

## Linking your experiment with IBM Cloud Object Storage

6.Next step is to hook the cloud object storage instance that you had created in the last step. Click on `Select` button for `Cloud Object Storage bucket for storing training source and results files`  and you will be redirected to a page asking you to connect your cloud object storage account with the experiment.

7.Click onthe tab that says `New connection` and select the cloud object storage instance that you had created earlier.8.

8.You will need a training/data bucket to hold your training data and a results bucket to hold your results. 

9.Select the `New` radio button for creating `Bucket containing training data` and `Bucket for storing training results`.

** NOTE: Bucket name is restricted to lowercase letters from a to z, numbers, or dashes, between 3 and 64 characters in length.**

10.The bucket names are globally unique, so be careful in picking up names for your train/results bucket. You can add a suffix of your username to make them unique.

11.This will create the test and training buckets for you and will show you links to the buckets. On the left panel, look for `Cloud Object Storage buckets for storing training source and results files`

12.Click on the link to the source bucket and it will open a new tab. You will need to upload the training data set for mnist to the source bucket. You can find the dataset [here] and drag and drop these files to the source bucket.

13.Close the tab once you have the data uploaded and come back to the IBM Watson Studio tab.

## Creating a training definition

11.Next step it to go ahead and create a training definition which will be used to specify details about your training. Follow the instructions [here]()