# New Document# Watson Hands On Labs - IBM Watson Machine Learning

The labs cover [Watson Machine Learning][wml_service] in particular how to train a Deep Learning Model and then how to Server/Inference. [Watson Machine Learning][wml_service] Service is available on [IBM Cloud][ibmcloud]. Throughout the workshop, we will navigate through IBM Cloud, Watson Machine Learning Service, Github, and the source code of our application in order to demonstrate how deep learnings models can be trained quickly and easily using the [IBM Cloud][ibmcloud] platform.

So let’s get started. The first thing to do is to build out the shell of our application in Bluemix.

## Creating a [IBM][ibmcloud] Account

  1. Go to [https://console.ng.bluemix.net/](https://console.ng.bluemix.net/)
  2. Create a IBM ID if required.
  3. Log in with your IBM ID (the ID used to create your Bluemix account)

**Note:** The confirmation email from IBM Cloud mail take up to 1 hour.

## Setup the your local environment to interact with IBM Cloud 

1. Open Terminal window 
2. Change to `think_wml_lab` directory
3. Run `linux-osx_wmlcli_setup.sh` to install Bluemix CLI 
4. login to IBM Cloud using CLI
     `bx api https://api.ng.bluemix.net`
    
     `bx login`
     
      or:
     
     `bx login -sso  < select your personal account>`
5. Set resource target to default
     `bx target -g default`      
6. Run `linux-osx_wmlcli_setup.sh` to install ML plugin to bx CLI, create a WML instance, create a COS account and buckets
   ` Note: Enter a unique name for your bucket. i.e: think-<your_lastname> when asked`
7. This command wil create an instance of [Cloud Object Store][cos_service], [Watson Machine Learning][wml_service]. Record the output as we will need this for the next step. 
8. Export your COS access key ID and secret access key (These are your COS credentials)

   		 export AWS_ACCESS_KEY_ID=<access_keyid_from_output_from_above>
   	     export AWS_SECRET_ACCESS_KEY=<secret_key_from_above>

7. Export ML environment variables

 	     export ML_INSTANCE=<wml_instnace id>
 	     export ML_USERNAME=<wml_instance uaerid>   
 	     export ML_PASSWORD=<wml_instnace password>
  	     export ML_ENV=<wml_instance url> 
         
## Upload data files and update the tf-train.yaml file

2. Upload the training date files in sample_data folder to Cloud Object Storage
   `aws --endpoint-url=https://s3-api.us-geo.objectstorage.softlayer.net s3 sync sample_data/. s3://think-<your_lastname>`
 
2. Update `model/tf-train.yaml` file with your COS information. Update aws_access_key_id, aws_secret_access_key and bucket values


    name: training_data_reference_name
    connection:
      endpoint_url: "https://s3-api.us-geo.objectstorage.service.networklayer.com"
      aws_access_key_id: "<access_keyid_from_above>"
      aws_secret_access_key: "<access_key_from_above>"
    source:
      bucket: think-<your_lastname>
    type: s3
    
    training_results_reference:
      name: training_results_reference_name
      connection:
        endpoint_url: "https://s3-api.us-geo.objectstorage.service.networklayer.com"
        aws_access_key_id: "<access_keyid_from_above>"
        aws_secret_access_key: "<access_key_from_above>"
      target:
        bucket: think-<your_lastname>
      type: s3

  
## Submit your training job

1. Submit the training job

 ` bx ml model/train tf-train.zip model/tf-train.yaml`
 
    sample output:
        Starting to train ...
        OK
        Model-ID is 'training-kHC1ACgmg' (this will be your <training_id>)

2. Monitor training run:

  `bx ml show training-runs <training_id>`

    Sample output:
          Fetching the training run details with MODEL-ID 'training-kHC1ACgmg' ...
          ModelId                  training-kHC1ACgmg
          url                      /v3/models/training-kHC1ACgmg
          Name                     tf-mnist-showtest1
          Training definition ID   64154e3a-1397-4d95-a45c-1af73421ed87
          Command                  python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001 --trainingIters 20000
          Source bucket            think-sdiamond
          Target bucket            think-sdiamond
          Framework name           tensorflow
          Framework version        1.2
          State                    running
          Submitted_at             2018-03-17T19:52:00Z
          OK
          Show training-runs details successful

3. Monitor the logs of the training run:

  `bx ml monitor training-runs <training_id>`

    Sample output:
        Starting to fetch status and metrics messages for model-id 'training-kHC1ACgmg'
        [--LOGS]      Training with training/test data at:
        [--LOGS]        DATA_DIR: /mnt/data/think-sdiamond
        [--LOGS]        MODEL_DIR: /job/model-code
        [--LOGS]        TRAINING_JOB:
        [--LOGS]        TRAINING_COMMAND: python3 convolutional_network.py --trainImagesFile ${DATA_DIR}/train-images-idx3-ubyte.gz --trainLabelsFile ${DATA_DIR}/train-labels-idx1-ubyte.gz --testImagesFile ${DATA_DIR}/t10k-images-idx3-ubyte.gz --testLabelsFile ${DATA_DIR}/t10k-labels-idx1-ubyte.gz --learningRate 0.001 --trainingIters 20000
        [--LOGS]      Storing trained model at:
        ...

4.  Query training job status until status shows ‘completed’
   
   `bx ml show training-runs training-kHC1ACgmg`
   
5. You can list  the log file  or trained model or download the  trained model using the following aws cli
`aws --endpoint-url=https://s3-api.us-geo.objectstorage.softlayer.net s3 ls s3://think-<your_lastname>/<training_id>/learner-1/`

`aws --endpoint-url=https://s3-api.us-geo.objectstorage.softlayer.net s3 ls s3://think-<your_lastname>/<training_id>/model/`

`aws --endpoint-url=https://s3-api.us-geo.objectstorage.softlayer.net s3 cp s3://think-<your_lastname>/<training_id>/model/saved_model.pb `

## Deploy and score the model

1. Deploy and score the model

    `bx ml store training-runs <training_id>`
    
        Sample output:
          Checking if content upload is complete ...
          Checking if content upload is complete ...
          OK
          Model store successful. Model-ID is '1a39714a-65a0-44b8-9a26-a8a96dc56d66'. (this will be your <model_id>)

2. list the model that is saved in WML

   ` bx ml list models`

3. Deploy stored model to WML

    `bx ml deploy <model_id> <lastname-deployment>`
    
        Sample output:
          Deploying the model with MODEL-ID '1a39714a-65a0-44b8-9a26-a8a96dc56d66'...
          DeploymentId       a638dbd3-7046-4666-8a33-7beddb1b6bb0   (this wil be your <deployment_id>
          
          Scoring endpoint   https://ibm-watson-ml.mybluemix.net/v3/wml_instances/f702b67e-e5b1-4465-b0f7-b835ce68ec8d/published_models/1a39714a-65a0-44b8-9a26-a8a96dc56d66/deployments/a638dbd3-7046-4666-8a33-7beddb1b6bb0/online
          Name               sdiamond-deployment
          Type               tensorflow-1.2
          Runtime            None Provided
          Status             DEPLOY_SUCCESS
          Created at         2018-03-17T21:01:34.253Z
          OK
          Deploy model successful

4. Update scoring/scoring_payload.json file with your model id and deployment id


`{"modelId": "<model_id>","deploymentId": "<deployment_id>".... `


5. Score the model

    `bx ml score scoring/scoring_payload.json`

        Sample output:
          Fetching scoring results for the deployment 'a638dbd3-7046-4666-8a33-7beddb1b6bb0' ...
          {"values": [5, 4]}
          OK
          Score request successful

    
   
# Congratulations
Yahoo!!! You completed this lab!!! :bowtie:

[ibmcloud]: https://console.ng.bluemix.net/
[wml_service]: https://console.bluemix.net/catalog/services/machine-learning?taxonomyNavigation=apps
[cos_service]: https://console.bluemix.net/catalog/services/cloud-object-storage?taxonomyNavigation=apps
