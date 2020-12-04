# Various thoughts on the resubmission of the EKSCapstone project.  

There are two new screeenshots that demonstrate both linting success and failure against the Dockerfile.  There are named (sensibly enough) hadolintFail.jpg and hadolintSuccess.jpg.  
https://github.com/tacarsten/EKSCapstone/blob/main/hadolintFail.JPG
https://github.com/tacarsten/EKSCapstone/blob/main/hadolintSuccess.JPG

The EKS cluster was created using AWS Cloudformation.   The code used to accomplish this is now in the "Cloudformation" directory.
https://github.com/tacarsten/EKSCapstone/tree/main/Cloudformation

The deployment type has been changed from "Rolling" to "Blue/Green".  Thusly, there are two new deployment yaml files, one each for blue and green. Both deployments occur within the same EKS namespace but with different application names. 

The service resource is created using service.yaml. This yaml file is used in Jenkins deploy stage. The loadbalancer service routes based on the selector in service.yaml.

The Jenkins pipeline now takes an input of the deployment type (blue or green). Based on the input, the proper deployment file is selected and used in the deploy stage. This parameter is also modified in service.yaml so that it updates the extenal endpoint so that it refers to the latest deployment.

The file blue-green-deployment.jpg shows the blue/green pods running under the capstone namespace. This image also shows the description of service which uses the selector to forward traffic to apps.
