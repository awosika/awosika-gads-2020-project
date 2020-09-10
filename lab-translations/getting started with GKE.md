
Getting started with Kubernetes Engine

Overview
In this lab, you create a Google Kubernetes Engine cluster containing several containers, each containing a web server. You place a load balancer in front of the cluster and view its contents.

Objectives
In this lab, you learn how to perform the following tasks:

Provision a Kubernetes cluster using Kubernetes Engine.

Deploy and manage Docker containers using kubectl.

Task 1: Sign in to the Google Cloud Platform (GCP) Console
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

1. Make sure you signed into Qwiklabs using an incognito window.

2. Note the lab's access time and make sure you can finish in that time block.

3. When ready, click start_lab.

4. Note your lab credentials. You will use them to sign in to Cloud Platform Console.

5. Click Open Google Console.

6. Click Use another account and copy/paste credentials for this lab into the prompts.

7. Accept the terms and skip the recovery resource page.

Task 2: Confirm that needed APIs are enabled

1. Make a note of the name of your GCP project. This value is shown in the top bar of the Google Cloud Platform Console. It will be of the form qwiklabs-gcp- followed by hexadecimal numbers.

2. In the GCP Console, on the Navigation menu (Navigation menu), click APIs & Services.

3. Scroll down in the list of enabled APIs, and confirm that both of these APIs are enabled:

Kubernetes Engine API
Container Registry API

If either API is missing, click Enable APIs and Services at the top. Search for the above APIs by name and enable each for your current project. (You noted the name of your GCP project above.)

Translating Task 2 to command line

1a. Take note of the name of your GCP project. This can be seen in the top bar of Google cloud platform console or on the left pane just under the qwiklabs credentials (username and password) used to sign into the Google cloud platform console

2a. Activate Cloud shell or use your installed Google SDk

3a. run this command in the cloud shell to enable Kubernetes Engine Api:
gcloud services enable container.googleapis.com

note: you will get "finished successfully" message if Kubernetes Engine Api was not enabled prior to executiing the above command, else it will update with any message

3b. run this command in the cloud shell to enable Container Registry API:
gcloud services enable containerregistry.googleapis.com


Task 3: Start a Kubernetes Engine cluster

1. In GCP console, on the top right toolbar, click the Open Cloud Shell button.

2. Click Continue.

Note: step 1 and step 2 are not neccessary, you are already in cloud shell

3. For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:

export MY_ZONE=

followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:

export MY_ZONE=us-central1-a

4. Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

5. After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

kubectl version

The gcloud container clusters create command automatically authenticated kubectl for you.

6.View your running nodes in the GCP Console. On the Navigation menu (Navigation menu), click Compute Engine > VM Instances.

translating step 6 to command line:
6a. to view your running nodes in cloud shell, execute this command:

gcloud compute instances list

Task 4: Run and deploy a container

1. From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)

kubectl create deploy nginx --image=nginx:1.17.10

Note: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.

2. View the pod running the nginx container:

kubectl get pods

3. Expose the nginx container to the Internet:

kubectl expose deployment nginx --port 80 --type LoadBalancer

Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

4. View the new service:

kubectl get services

You can use the displayed external IP address to test and contact the nginx container remotely.

It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.

5. Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

6. Scale up the number of pods running on your service:

kubectl scale deployment nginx --replicas 3

Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

7. Confirm that Kubernetes has updated the number of pods:

kubectl get pods

8. Confirm that your external IP address has not changed:

kubectl get services

9. Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.

Congratulations!
In this lab, you configured a Kubernetes cluster in Kubernetes Engine. You populated the cluster with several pods containing an application, exposed the application, and scaled the application.








