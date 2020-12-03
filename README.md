# eks-capstone

linting-1.JPG and linting-2.JPG images show failed liniting for Dockerfile.

EKS cluster is created using Cloudformation and the cloudformation scripts are under the Cloudformation directory.

There are two deployment yaml files created. One each for blue and green deployment. Both deployments happen under the same EKS namespace but with different app names. 

The service resource is created using service.yaml. This yaml file is used in Jenkins deploy stage. Loadbalancer service routes based on the selector in service.yaml.

Jenkins pipeline takes input of the deployment type i.e. blue or green. Based on the input deployment file is selected and used in the deploy stage. This parameter is also modified in service.yaml so that it updates the extenal endpoint to point to latest deployment.

blue-green-deployment.JPG image shows the blue/green pods running under the capstone namespace. Image also shows the description of service which uses selector to forward traffic to apps.
