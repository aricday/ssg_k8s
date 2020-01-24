# Brief Setup for your GCP
Repo to enable GCP Kubernetes Engine cluster with ssg_v9.4

## Create a project, record your project id
1) Enable API access for Kubernetes engine and create a service account for it
2) Download the json key of your service account, and put it to your local gcp working folder
[Reference doc] (https://cloud.google.com/kubernetes-engine/docs/quickstart)

## Create kubernete clusters, sample commands are:
` gcloud container clusters create gatewaydemo --no-enable-autorepair --num-nodes=1 `

` gcloud container clusters get-credentials gatewaydemo `

## Prepare license file:
* Add your gateway license file to /ssg94/add-on/license folder.
` echo "LICENSE=\"$(gzip -c add-ons/license/license.xml | base64)\""  `

* Add the base64 encoded license file at [gmK8S.yml](gmK8S.yml#L75) to enable the gateway.


## Create kubernetes services:
` kubectl create -f gmK8S.yml `

## Check services are up
` kubectl get deployments `

` kubectl get pods `

` kubectl get services `

` kubectl describe pods api-gateway `

## Delete the kubernetes services:

` kubectl delete -f ssg94.yml `

## Build and Tag your docker images with bundles and licenses
* Add your gateway license file to /ssg94/add-on/license folder.
* Add your GMU bundles to /ssg94/add-on/bundles folder if you have any assets to bootstrap.

#### Run "docker-compose -f docker-compose.yml build" to build a custom container
    ` docker-compose -f docker-compose.yml build `
    
1) Tag your image w/ format “gcr.io/{your_project_id}/ssg94”

        docker tag {image_id} {new_image_tag}

2) Tag mysql5.7 following the same format above.

## Push the new docker images to google cloud, sample command:
` gcloud docker -- push gcr.io/{your_project_id}/ssg94 `

` gcloud container images list `

## Create kubernetes services using the built custom image w/license:
` kubectl create -f ssg94.yml `

## Check services are up
` kubectl get deployments `

` kubectl get pods `

` kubectl get services `

` kubectl describe pods api-gateway `

## Delete the kubernetes services:

` kubectl delete -f ssg94.yml `

## Delete cluster:
` gcloud container clusters delete gatewaydemo `


# Next Step
Navigate to the [SSL_K8S README](ssl_k8s) to get started with SSL.
