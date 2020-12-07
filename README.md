# Various thoughts on resubmitting this EKSCapstone project.  

There are two new screeenshots that demonstrate both linting success and failure against the Dockerfile.  These are named (sensibly enough) hadolintFail.jpg and hadolintSuccess.jpg.  
https://github.com/tacarsten/EKSCapstone/blob/main/hadolintFail.JPG
https://github.com/tacarsten/EKSCapstone/blob/main/hadolintSuccess.JPG

The EKS cluster was created using AWS Cloudformation.   The code used to accomplish this is now in the "Cloudformation" directory. I find I fight less with .json formatting over .yaml.  So it is .json throughout.  
https://github.com/tacarsten/EKSCapstone/tree/main/Cloudformation

The deployment type has been changed from "Rolling" to "Blue/Green".  Thusly, there are two new deployment yaml files, one each for blue and green. Both deployments occur within the same EKS namespace but with different application names. Other files demonstrate the technical details of how k8 namespace is used.  
https://github.com/tacarsten/EKSCapstone/blob/main/capstone-deployment-blue.yml
https://github.com/tacarsten/EKSCapstone/blob/main/capstone-deployment-green.yml

The service resource is created using service.yaml. This yaml file is used in Jenkins deploy stage. The loadbalancer service routes based on the selector found in service.yaml.
https://github.com/tacarsten/EKSCapstone/blob/main/service.yaml

The Jenkins pipeline now takes an input of the deployment type (blue or green). Based on the input, the proper deployment file is selected and used in the deploy stage. This parameter is also modified in service.yaml so that it updates the extenal endpoint so that it refers to the latest deployment.
https://github.com/tacarsten/EKSCapstone/blob/main/JenkinsReceivesParameter.jpg


The file eks-nodes-1.JPG shows the EKS nodes running as two completely seperate EC2 instances in AWS:
https://github.com/tacarsten/EKSCapstone/blob/main/eks-nodes-1.JPG

The file eks-nodes-2.JPG shows the K8 pods running on separate EC2 nodes. Note especially the "nodes" field on the far right: 
https://github.com/tacarsten/EKSCapstone/blob/main/eks-nodes-2.JPG

Lastly, the K8 deployment is actually live right now.
This can be verified with the following URL:
http://a0bff960d5b47429fb4b28894d3308f2-126931079.us-west-2.elb.amazonaws.com/hello
