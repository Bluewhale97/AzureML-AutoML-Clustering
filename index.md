## Introduction

Clustering is a type of unsupervised machine learning on which we do not predict the label from featrues and we can separate items into clusters based purely on the characteristics. 

In this article we also should create the resource, the compute instances and targets. I have the articles that are very specifically talking about how to step by step create resources and machine learning instances. The respositories names are: AzureML-Startup and AzureML-AutoMLStartup, welcome to review them.

Now we can use Microsoft Azure Machine Learning designer to create clustering models without writing code.



## 1. Prerequisition

In Azure we can perform all of machine learning assignments. Before implementing the assignments, we should build up the platforms, which means that we need to create an Azure Machine Learnig workspace, create compute resources and compute targets. 

There are several configurations that should be set in advance.

To create an Azure machine learning workspace:

We need to sign into the Azure portal and create a resource, please configure for the following information:

![image](https://user-images.githubusercontent.com/71245576/115499085-c6ae9600-a23c-11eb-8d05-6bd4d886f925.png)

It could take a few minutes.

Then we need to create compute resources. To train and deploy models using Azure Machine Learning designer, you need compute on which to run the training process, and to test the trained model after deploying it.

There are several compute resource that you can create:

![image](https://user-images.githubusercontent.com/71245576/115499174-f8bff800-a23c-11eb-9553-8b97d1a0b458.png)

On the Compute Instances tab, add a new compute instance with the following settings, and you will use this as the workstation from which to test the models.

![image](https://user-images.githubusercontent.com/71245576/115499250-1beaa780-a23d-11eb-9307-3f3fed64764b.png)

While the compute instance is being created, switch to the Compute Clusters tab, and add a new compute cluster with the following settings. You'll use this to train a machine learning model:

![image](https://user-images.githubusercontent.com/71245576/115499291-33c22b80-a23d-11eb-8c52-1472bf1b7d73.png)

## 2. Loading the data

In Azure Machine Learning studio, view the Datasets page. Datasets represent specific data files or tables that you plan to work with in Azure ML

Create a dataset from web files, the dataset is from https://aka.ms/penguin-data.

Now set the basic info:

![image](https://user-images.githubusercontent.com/71245576/115637037-4a1fc400-a2dd-11eb-900d-d4a3adfe1a4e.png)

Settings and preview:

![image](https://user-images.githubusercontent.com/71245576/115637120-6f143700-a2dd-11eb-82a3-eebd6c684d49.png)

In Schema setting, include all columns other than Path and review it:

![image](https://user-images.githubusercontent.com/71245576/115637189-8e12c900-a2dd-11eb-90e1-1fd4cb5b7732.png)

It took a few second, now you can see it has completed successfully:

![image](https://user-images.githubusercontent.com/71245576/115637276-c1555800-a2dd-11eb-9599-dbdb148b297b.png)

Create a new pipeline named Penguin training, drag dataset penguin-data to the canvas:

![image](https://user-images.githubusercontent.com/71245576/115637338-ed70d900-a2dd-11eb-8005-42b647b334e5.png)

On the settings pane, visualize the data result:

![image](https://user-images.githubusercontent.com/71245576/115637378-05485d00-a2de-11eb-9b41-5cbb289bcd43.png)

## 3. Data transformations

To cluster the penguin observations, we're going to use only the measurements; so we'll discard the species column. We also need to remove rows where values are missing, and normalize the numeric measurement values so they're on a similar scale.

In the pane on the left, expand the Data Transformation section, which contains a wide range of modules you can use to transform data before model training

To cluster the penguin observations, we're going to use only the measurements - we'll ignore the species column. So, drag a Select Columns in Dataset module to the canvas, below the penguin-data module and connect the output at the bottom of the penguin-data module to the input at the top of the Select Columns in Dataset module, like this:

![image](https://user-images.githubusercontent.com/71245576/115637526-59534180-a2de-11eb-8432-548025bc501a.png)

Select the Select Columns in Dataset module, and in its Settings pane on the right, select Edit column. Then in the Select columns window, select By name and use the + links to select the column names CulmenLength, CulmenDepth, FlipperLength, and BodyMass; like this

![image](https://user-images.githubusercontent.com/71245576/115637600-8b64a380-a2de-11eb-862f-d0d25bc0f13d.png)

Select the Clean Missing Data module, and in the settings pane on the right, click Edit column. Then in the Select columns window, select With rules and include All columns; like this:

![image](https://user-images.githubusercontent.com/71245576/115637761-f7470c00-a2de-11eb-8f51-7d9806fb3940.png)

Choose mode on the settings of missing data module:

![image](https://user-images.githubusercontent.com/71245576/115637813-12b21700-a2df-11eb-8b4a-36313b9e9b59.png)

Select the Normalize Data module, and in its Settings pane on the right, set the Transformation method to MinMax and select Edit column. Then in the Select columns window, select With rules and include All columns; like this:

![image](https://user-images.githubusercontent.com/71245576/115637876-370df380-a2df-11eb-9070-f6ce4e0ef2c8.png)

Now your pipeline is like this:

![image](https://user-images.githubusercontent.com/71245576/115638150-cfa47380-a2df-11eb-84ef-a407408c9e9f.png)

Submit to run it:

![image](https://user-images.githubusercontent.com/71245576/115639142-daf89e80-a2e1-11eb-84dc-f768cd698a60.png)

View the transformed data on the setting panes, you should click visualize to visualize it:

![image](https://user-images.githubusercontent.com/71245576/115639170-ef3c9b80-a2e1-11eb-8f70-1c813b1a18b1.png)

## 4. Machine learning pipeline

After you've used data transformations to prepare the data, you can use it to train a machine learning model.

In the pane on the left, in the Data Transformations section, drag a Split Data module onto the canvas under the Normalize Data module. Then connect the left output of the Normalize Data module to the input of the Split Data module. And configure Split Data module settings:

![image](https://user-images.githubusercontent.com/71245576/115639333-4f334200-a2e2-11eb-95fb-7e6a85add5a9.png)

Drag a Train Clustering Model module to the canvas, under the Split Data module. Then connect the Result dataset1 (left) output of the Split Data module to the Dataset (right) input of the Train Clustering Model module. The clustering model should assign clusters to the data items by using all of the features you selected from the original dataset. Select the Train Clustering Model module and in its settings pane, on the Parameters tab, select Edit Columns and use the With rules option to include all columns; like this:

![image](https://user-images.githubusercontent.com/71245576/115639463-a33e2680-a2e2-11eb-9b74-a0b1aa296749.png)

Now your pipeline could be like this:

![image](https://user-images.githubusercontent.com/71245576/115639500-ba7d1400-a2e2-11eb-8895-86a43dade73e.png)

Expand the Machine Learning Algorithms section, and under Clustering, drag a K-Means Clustering module to the canvas, to the left of the penguin-data dataset and above the Train Clustering Model module. Then connect its output to the Untrained model (left) input of the Train Clustering Model module.

Set the centroids(the number of clusters) as 3:

![image](https://user-images.githubusercontent.com/71245576/115639585-ff08af80-a2e2-11eb-89ff-f0c94ff354bf.png)

Expand the Model Scoring & Evaluation section and drag an Assign Data to Clusters module to the canvas, below the Train Clustering Model module. Then connect the Trained model (left) output of the Train Clustering Model module to the Trained model (left) input of the Assign Data to Clusters module; and connect the Results dataset2 (right) output of the Split Data module to the Dataset (right) input of the Assign Data to Clusters module.

![image](https://user-images.githubusercontent.com/71245576/115639679-324b3e80-a2e3-11eb-9b37-853a732cee36.png)

Submit to run. When the experiment run has finished, select the Assign Data to Clusters module and in its settings pane, on the Outputs + Logs tab, under Data outputs in the Results dataset section, use the Visualize icon to view the results.

![image](https://user-images.githubusercontent.com/71245576/115640406-dbdeff80-a2e4-11eb-8630-9e469915d26d.png)

## 5. Evaluating a clustering model

Evaluating a clustering model is made difficult by the fact that there are no previously known true values for the cluster assignments. A successful clustering model is one that achieves a good level of separation between the items in each cluster, so we need metrics to help us measure that separation.

Now drag an Evaluate Model module to the canvas, under the Assign Data to Clusters module, and connect the output of the Assign Data to Clusters module to the Scored dataset (left) input of the Evaluate Model module.

Our pipeline is like this:

![image](https://user-images.githubusercontent.com/71245576/115640486-1052bb80-a2e5-11eb-8f9d-9338cc78b981.png)

Submit to run, you can use the Visualize icon to view the performance metrics. These metrics can help data scientists assess how well the model separates the clusters. They include a row of metrics for each cluster, and a summary row for a combined evaluation.

![image](https://user-images.githubusercontent.com/71245576/115640797-c6b6a080-a2e5-11eb-8d1c-6191d7d85b7a.png)

## 6. Creating an inference pipeline

In the Create inference pipeline drop-down list, click Real-time inference pipeline. After a few seconds, a new version of your pipeline named Train Penguin Clustering-real time inference will be opened. It contains a web service input for new data to be submitted, and a web service output to return results.

Replace the penguin-data dataset with an Enter Data Manually module that does not include the Species column. The data are below:

```excel
CulmenLength,CulmenDepth,FlipperLength,BodyMass
39.1,18.7,181,3750
49.1,14.8,220,5150
46.6,17.8,193,3800
```
Connect the outputs from both the Web Service Input and Enter Data Manually modules to the Dataset (right) input of the first Apply Transformation module.

Remove the Select Columns in Dataset module, which is now redundant

Remove the Evaluate Model module.

Now our pipeline should be like this:

![image](https://user-images.githubusercontent.com/71245576/115641205-9b808100-a2e6-11eb-838c-e10b8436d473.png)

Submit it as a new experiment to run it. When it has finished, visualize the results daatset:

![image](https://user-images.githubusercontent.com/71245576/115642857-b274a280-a2e9-11eb-8904-c37bea6051ad.png)

## 7. Deploying a predictive service

After you've created and tested an inference pipeline for real-time inferencing, you can publish it as a service for client applications to use.

Select Deploy, and deploy a new real-time endpoint, using the following settings:

![image](https://user-images.githubusercontent.com/71245576/115642984-eea80300-a2e9-11eb-84a8-3410bbe8157c.png)

Wait for the web service to be deployed - this can take several minutes. The deployment status is shown at the top left of the designer interface.

Create a new file and open a notebook, paste the following code, noting that please replace with your endpoint and the primary key information:

```python
endpoint = 'YOUR_ENDPOINT' #Replace with your endpoint
key = 'YOUR_KEY' #Replace with your key

import urllib.request
import json
import os

data = {
    "Inputs": {
        "WebServiceInput0":
        [
            {
                    'CulmenLength': 49.1,
                    'CulmenDepth': 4.8,
                    'FlipperLength': 1220,
                    'BodyMass': 5150,
            },
        ],
    },
    "GlobalParameters":  {
    }
}

body = str.encode(json.dumps(data))


headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ key)}

req = urllib.request.Request(endpoint, body, headers)

try:
    response = urllib.request.urlopen(req)
    result = response.read()
    json_result = json.loads(result)
    output = json_result["Results"]["WebServiceOutput0"][0]
    print('Cluster: {}'.format(output["Assignments"]))

except urllib.error.HTTPError as error:
    print("The request failed with status code: " + str(error.code))

    # Print the headers to help debug
    print(error.info())
    print(json.loads(error.read().decode("utf8", 'ignore')))
```

Run it and take the prediction:

The cluster that predicted is 1.

![image](https://user-images.githubusercontent.com/71245576/115643508-e603fc80-a2ea-11eb-9683-4ead91d3c856.png)

## Reference

Create no-code predictive models with Azure Machine Learning, retrieved from https://docs.microsoft.com/en-us/learn/paths/create-no-code-predictive-models-azure-machine-learning/.
