# SageMaker and Red Hat OpenShift on AWS (ROSA) Example
This is an example showcasing one simple way to connect SageMaker and ROSA.
- hyperparameter tuning, training, and detecting bias using AWS Sage Maker
- containerizing and deploying to Red Hat OpenShift on AWS for scablable serving/inferencing

In terms of maturity we call this MLOps level 0. It is great for learning and demo but not recommended for production.
If you are interested in more advanced MLOps reference architectures integrating AWS ML and OpenShift, [please see here]().

## How this flow works from a people point-of-view
You can see in the diagram below (in this example) major activities are grouped to key capablity provided by cloud services. Data Prep and Modeling & Training happens in AWS SageMaker while Deployment & Integration plus Ops & Monitoring happens using ROSA. Obviously these major activities will break down into more discrete tasks, but hopefully this helps to orient you on the overview flow.
![people_process](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa/blob/main/.screens/people_process.png)


## Why MLOps maturity level 0?
Simply put, there isn't enough automation and not enough repeatability in the full flow to be at a higher maturity level. We do have a basic level of MLOps here though, we have the ability for a data scientist to retrain a model and even have that trigger the rebuild, containerization, and re-deployment of our prediction service. But it would be nice to leverage a feature store, a data prep pipeline, automated retraing, and feedback loop to get production data back into the training cycle. 

Granted MLOps maturity is a bit subjective we definitely can do a lot to improve what is being shown here.

## Running the example
This example showcases a fraud detection prediction service. It trains on synthetically generated auto insurance claims that indicate if a claim was fradulent or not - a binary classification task. The data has lots of things like number or ppl injured, claim amount, customer gender, the state, if a police report is available, etc... You'll need an AWS account with the ability to use SageMaker and also a ROSA cluster where the service will run.

Follow the guide below to recreate the environment and try this for yourself.

### Model tuning and training in your SageMaker account
1. Open the SageMaker console and [create a new notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-prepare.html) (defaults are fine)
2. Once the notebook is available click the linke to Open JupyterHub
3. Click the git icon on the left bar and clone [this repo](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa.git)
4. Open the .ipynb from the file explorer and choose the conda_python3 kernel from the popup
5. Run the notebook

### Deploying as an API into your ROSA cluster
1. Open your ROSA developer webconsole
2. Create a new project
2. Click +Add, and choose Import from Git
3. Paste in the details of [this repo](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa.git)
4. It will auto populate some sane defaults, click Create
5. Go to Builds, select the BuildConfig that was created, goto the Environment tab and add `MODEL_PATH` pointing to a preferred built model in S3 from your SageMaker training. 
6. TBD setup automatic build triggers and event bridge

#### Environment variables
The s2i builder expects some env vars, they are defaulted in the `.s2i/environment` file. You should override them in your CI/CD trigger or manually in your build config. The current set is:
* MODEL_PATH - where to find your xgboost model (expected in `.tar.gz` format). Note: the build will accept s3: or http: paths.

### Using OpenAPI docs to test the model
navigate to your exposed `ROUTE_URL/docs` and you should see a page like the screen shot below. It automatcially shows the model features and allows you to input test data to try the prediction service. You can find some example data to test with in the [test folder of this repo](https://github.com/redhat-na-ssa/openshiftexamples-sagemaker-train-xgboost-rosa/blob/main/test/example_features.json)

![fastapi_try](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa/blob/main/.screens/fastapi_try.png)



### Watching metrics in ROSA
There are several important metrics you might want to observe and be alerted around related to operationalizing a machine learning service like this. We can leverage the built in components of ROSA to set this up.
 - TBD how to setup, Click +Add, and choose YAML paste in this URL...
 - TODO screenshot
 
