# Project Structure

. aws-pole-inference
├── api-source                    <!--Contains the API source code.-->
│   ├── api                       <!--To implement Micro Service-->
│   ├── arcanumai                 <!--Arcanum class-->
│   ├── clients                   <!--Other code-->
│   ├── pole_invocations          <!--Model infer code-->
└── docker                        <!--Docker-->
└── resource                      <!--Terraform code-->
└── docs                          
├── script                        <!--Service start scripts-->
│   ├── bitbucket-pipelines.yml   <!--Git pipeline-->
│   ├── Makefile                  <!--To run locally-->
│   ├── pyproject.toml            <!--Python-poetry config file to orchestrate project and it's dependencies-->

# API URL
- Please see the following format of the API url:
ike-dev-118525968.ap-southeast-2.elb.amazonaws.com/api/api/invocations/{collection_id}/{image_name).jpeg

- For eg, if the {collection_id} has value yhkTh1YEJl and {image_name} has value jfJFlqMHtE_medium, the API URL will be:
ike-dev-118525968.ap-southeast-2.elb.amazonaws.com/api/api/invocations/yhkTh1YEJl/jfJFlqMHtE_medium.jpeg

# To run the IKE GPS in the local, we can run the following commands in the directory where the IKE GPS is located. Please make sure that the docker is installed and running in your local system.

- make build-dev <!--To build the docker image of the service, please use this command.->
- make run-dev <!--To run the docker image that we created, please use this command.->

# To trigger the pipeline.

- To trigger the pipeline, from the bit bucket, go to the repository of the IKE GPS service. There we can see an option of adding tags.
- Click on the "+" button and it will open up the option of adding tags.
- In the tag name have a format of tag name like this "release-<version-number>", for eg: "release-0.0.3". Click on the button that says "Create tag". This will create the tag for us. 


# 1. Project Structure:

The project structure of the IKE GPS is as follows:

```html
. aws-pole-inference
├── api-source                    #Contains the API source code.
│   ├── api                       #To implement Micro Service
│   ├── arcanumai                 #Arcanum class
│   ├── clients                   #Other code
│   ├── pole_invocations          #Model infer code
└── docker                        #Docker
└── resource                      #Terraform code
└── docs                          
├── script                        #Service start scripts
│   ├── bitbucket-pipelines.yml   #Git pipeline
│   ├── Makefile                  #To run locally
│   ├── pyproject.toml            #Python-poetry config file to orchestrate project and it's dependencies
```

# 2. Processed check logic

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/646d4be5-32c7-4945-8cc3-f8f1bca0b964/Untitled.png)

### 1. Check Bill-result

- In the first step, we will grab the image. It will select the image from the arguments provided in `image_name` and the `container_id`.

### 2. Check Processed?

- In the second step, we will check if the image has been already processed or not. If it has been processed, then we return to the billing result.
- If it hasn't been processed we will continue with the process of detecting the pole

### 3. Download Image

- Here we will download the image for the detection process from the IKE Office API.

### 4. Pole Detection process

- In this stage, the pole detection process occurs. The events that happen as the result of this process are described in the further steps.

### 5. Detect Successful?

- If the pole detection process is not successful we will return the result.
- If the pole detection process is successful we will proceed with the further steps.
- For either of the results(successful or not successful) we will save the result in the Dynamo DB.

### 6. Save Detect Result

- After the pole detection process is successful, we will save the result in the S3 bucket.
- We will also continue ahead with the process.

### 7. Hitting Classification

- After saving the successful result of the detection of the pole in the pole detection process in S3, we will proceed further with the classification of the result.

### 8. Save Billing Result

- After the classification, the result will be saved. After saving the results we will return the result and go back to Dynamo DB.

# 3. How to trigger pipeline:

To trigger the pipeline, from the bit-bucket, go to the repository of the IKE GPS service. There we can see an option of adding tags:

 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb9f063d-173a-4b00-8a2f-b2ca0024a4a8/Untitled.png)

Click on the "+" button and it will open up the option of adding tags:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ebfba73-d04b-4004-8b90-6e34a1dfec18/Untitled.png)

In the tag name have a format of tag name like this "release-<version-number>", for eg: "release-0.0.3". Click on the button that says "Create tag". This will create the tag for us. After this the pipeline will trigger. Please see the screenshot of successfully triggering the pipeline below:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad7657ef-8878-4d12-aa36-b1ac05793e4e/Untitled.png)

# 4. To run it in your local system:

 To run the IKE GPS in the local, we can run the following commands in the directory where the IKE GPS is located. Please make sure that the docker is installed and running in your local system.

```html
make build-dev
```

This command will build the docker image of our service.

```html
make run-dev
```

This command will be useful in running the docker image we created in the previous step.

Please see the output of the above commands here:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f044465-2642-4672-bfb2-03e47c09fb9f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3215c891-d452-4d94-bed0-329472ee6765/Untitled.png)

The API URL will have a format as shown below:

`ike-dev-118525968.ap-southeast-2.elb.amazonaws.com/api/api/invocations/{collection_id}/{image_name).jpeg`

For eg, if the {collection_id} has value **yhkTh1YEJl** and {image_name} has value **jfJFlqMHtE_medium**, the API URL will be:
`ike-dev-118525968.ap-southeast-2.elb.amazonaws.com/api/api/invocations/yhkTh1YEJl/jfJFlqMHtE_medium.jpeg`

We can use this API URL in the postman API. For eg:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce5debff-35f1-4f13-ae52-014769da903b/Untitled.png)

# 5. Model Update

### 1. Update the model file:

1. Replace the model file **"Pole.h5"** to the file you want in the directory: Please see the file structure below for the location of the model file in the "aws-pole-inference" directory: 

```html
. aws-pole-inference
├── api-source
│   ├── api
│   ├── arcanumai
│   ├── clients
│   ├── pole_invocations
│   |   ├── static
│   |   |   ├── models
│   |   |   |   ├── **Pole.h5**
```

1. After adding the file, please commit the changes to the GIT.
2. After that, please add a new tag to trigger the pipeline. Please see the steps above in **3. How to trigger pipeline** to add a new tag and trigger the pipeline.

### 2. Update the model structure:

- To inherit the abstract methods of the base class please follow the following steps:
    1. The base class location:
    
    ```html
    . aws-pole-inference
    ├── api-source
    │   ├── api
    │   ├── arcanumai
    │   |   ├── **InferBase.py**
    ```
    

1. The class is as follows:

```
from abc import (ABC, abstractmethod)

class ModelInferBase(ABC):
    @abstractmethod
    def set_args(self, **kwargs):
        pass

    @abstractmethod
    def load_model(self, **kwargs):
        pass

    @abstractmethod
    def infer(self, **kwargs):
        pass
```

1. Please use this class to inherit the abstract methods. It will be preferable to have a class in the **"pole-invocations"** folder.

1. Please test the class to see if we get a suitable output.

1. After testing the class, please commit the changes to the GIT.

1. After that, please add a new tag to trigger the pipeline. Please see the steps above in **3. How to trigger pipeline** to add a new tag and trigger the pipeline.