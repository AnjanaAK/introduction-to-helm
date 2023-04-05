# introduction-to-helm


## Installing Helm

https://helm.sh/docs/intro/install/


## Check the kubernetes cluster context helm is connecting to


    kubectl config get-contexts


## Search for helm charts in Artifact hub

https://artifacthub.io/


## Search for helm charts using Helm CLI


    helm search hub ingress-nginx

    helm search repo ingress-nginx


## Install Nginx ingress controller

Add repo:

    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

    helm repo update

    helm search repo ingress-nginx


Install chart:

    helm install [RELEASE_NAME] [REPO_NAME]/[CHART_NAME]  [FLAGS]

    helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-basic --create-namespace


List the installed chart:

    helm ls -n ingress-basic 

View the kubernetes resources created:

    kubectl get all -n ingress-basic
    kubectl get svc -n ingress-basic
    kubectl describe cm ingress-nginx-controller -n ingress-basic


## Upgrade the release using `--set` flag


    kubectl get pods -n ingress-basic

    helm upgrade ingress-nginx ingress-nginx/ingress-nginx -n ingress-basic --set controller.replicaCount=2

    kubectl get pods -n ingress-basic



## Rollback the release back to the previous version


    helm rollback ingress-nginx 1 -n ingress-basic 


Check the history of the release:


    helm history ingress-nginx -n ingress-basic



## Uninstall ingress controller


    helm uninstall ingress-nginx -n ingress-basic



## Install the helm chart with custom values file


    helm upgrade ingress-nginx ingress-nginx/ingress-nginx \
        --install \
        --namespace ingress-basic \
        --create-namespace \
        --values values-ic.yaml


Some other flags that can be used with `helm install` :

        --wait       
        --debug
        --timeout 10m0s


## Deploy aspnetapp using yaml files


    kubectl create ns test

    kubectl apply -f deployment.yaml -n test
    kubectl apply -f service.yaml -n test
    kubectl apply -f ingress.yaml -n test


## Commands that'll be useful while creating your own helm chart


Create a helm chart:

     helm create aspnetapp
     
     
Examine a helm chart for possible issues:

     helm lint ./aspnetapp


Render the helm chart templates locally and save it to a yaml file: 

    helm template aspnetapp --debug > test.yaml


Install the created chart:

    helm install aspnetapp ./aspnetapp -n test 


## Notes

Helm install resources to the k8s cluster in a particular order:

https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package 



Docker image used as `dmctest.azurecr.io/aspnetapp:latest` for the demo:

    mcr.microsoft.com/dotnet/samples:aspnetapp



Built-in objects of helm : 
https://helm.sh/docs/chart_template_guide/builtin_objects/     


Template function list :
https://helm.sh/docs/chart_template_guide/function_list/
