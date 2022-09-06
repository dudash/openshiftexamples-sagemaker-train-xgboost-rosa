# SageMaker Studio and Red Hat OpenShift on AWS (ROSA) Example
This is an example showcasing one simple way to connect SageMaker and ROSA.
- hyperparameter tuning, training, and detecting bias using AWS Sage Maker Studio
- containerizing and deploying to Red Hat OpenShift on AWS for scablable serving/inferencing

In terms of maturity we call this MLOps level 0. It is great for learning and demo but not recommended for production.
If you are interested in more advanced MLOps reference architectures integrating AWS ML and OpenShift, [please see here]().

## How this flow works from a people point-of-view
You can see in the diagram below (in this example) major activities are grouped to key capablity provided by cloud services. Data Prep and Modeling & Training happens in AWS SageMaker while Deployment & Integration plus Ops & Monitoring happens using ROSA. Obviously these major activities will break down into more discrete tasks, but hopefully this helps to orient you on the overview flow.
![people_process](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa/blob/main/.screens/people_process.png)


## Why MLOps maturity level 0?
Coming soon

## Running the example
Follow the guide below to try this yourself.

### Model tuning and training in your SageMaker Studio
TBD importing the notebook

### Deploying as an API into your ROSA cluster
TBD setup triggers or manual deploy steps

#### Environment variables
The s2i builder expects some env vars, they are defaulted in the `.s2i/environment` file. You should override them in your CI/CD trigger or manually in your build config. The current set is:
* MODEL_PATH - where to find your xgboost model (expected in `.tar.gz` format). Note: the build will accept s3: or http: paths.

### Using OpenAPI docs to test the model
navigate to your exposed `ROUTE_URL/docs` and you should see a page like the screen shot below. It automatcially shows the model features and allows you to input test data to try the prediction service.

![fastapi_try](https://github.com/dudash/openshiftexamples-sagemaker-train-xgboost-rosa/blob/main/.screens/fastapi_try.png)



### Watching metrics in ROSA
TBD
