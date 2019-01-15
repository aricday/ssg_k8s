# Brief Setup for your GCP
Repo to enable GCP Kubernetes Engine cluster with ssg_v9.4

## Create a project, record your project id
1) Enable API access for Kubernetes engine and create a service account for it
2) Download the json key of your service account, and put it to your local gcp working folder
[Reference doc] (https://cloud.google.com/kubernetes-engine/docs/quickstart)

## Build and Tag your docker images
* Add your gateway license file to /ssg94/add-on/license folder.
* Add your GMU bundles to /ssg94/add-on/bundles folder if you have any assets to bootstrap.

#### Run "docker-compose -f docker-compose.yml build" to build a custom container
1) Tag your image use a name following this format “gcr.io/{your_project_id}/ssg94”

        docker tag {image_id} {new_image_tag}

2) Tag mysql5.7 following the same format above.

## Push the two docker images to google cloud, sample command:
` gcloud docker -- push gcr.io/{your_project_id}/ssg94 `

` gcloud container images list `

## Create kubernete clusters, sample commands are:
` gcloud container clusters create gatewaydemo --no-enable-autorepair `

` gcloud container clusters get-credentials gatewaydemo `

## Create kubernetes services using the SSG yaml file:
` kubectl create -f ssg94.yml `

## Check services are up
` kubectl get deployments `

` kubectl get pods `

` kubectl get services `

` kubectl describe pods api-gateway `
