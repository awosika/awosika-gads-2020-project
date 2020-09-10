#getting started with compute engine

Overview

In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

Objectives
In this lab, you will learn how to perform the following tasks:

Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

Create a Compute Engine virtual machine using the gcloud command-line interface.

Connect between the two instances.

Task 1: Sign in to gcp web console.

Task 2a: Create a virtual machine using the GCP Console

web console instructions to create a compute engine virtual machine 

On the Navigation menu (Navigation menu), click Compute Engine > VM instances.
Click Create.
On the Create an Instance page, for Name, type my-vm-1
For Region and Zone, select the region and zone assigned by Qwiklabs.
For Machine type, accept the default.
For Boot disk, if the Image shown is not Debian GNU/Linux 9 (stretch), click Change and select Debian GNU/Linux 9 (stretch).
Leave the defaults for Identity and API access unmodified.
For Firewall, click Allow HTTP traffic.
Leave all other defaults unmodified.
To create and launch the VM, click Create.

Task2b:Translating from web console to command line

activate cloud shell

To display a list of all the zones in the region to which Qwiklabs assigned you enter this command:
gcloud compute zones list

To set your default zone to the zone of your choice, enter this command:
gcloud config set compute/zone followed by the zone you chose

for example gcloud config set compute/zone us-central1-a

To create a VM instance called my-vm-1 in the zone of your choice, execute this command:

gcloud compute instances create my-vm-1 --machine-type=n1-standard-1 --subnet=default --tags=http-server --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 

For firewall, run this commannd in cloud shell:
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

The command above will create VM instances as required in the web console'

Note: The VM can take about two minutes to launch and be fully available for use.

Task 3: Create a virtual machine using the gcloud command line

activate cloud shell

To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command gcloud compute zones list | grep followed by the region that Qwiklabs or your instructor assigned you to.

your completed command should look like this:
gcloud compute zones list | grep us-central1

Choose a zone from that list other than the zone to which Qwiklabs assigned you. For example, if Qwiklabs assigned you to region us-central1 and zone us-central1-a you might choose zone us-central1-b.

To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.

The completed command should look like this:
gcloud config set compute/zone us-central1-b

To create a VM instance called my-vm-2 in that zone, execute this command:

gcloud compute instances create my-vm-2 --machine-type=n1-standard-1 --subnet=default --tags=http-server --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-2 

Note: The VM can take about two minutes to launch and be fully available for use.

Task 4: Connect between VM instances

1a. In the Navigation menu (Navigation menu), click Compute Engine > VM instances.

You will see the two VM instances you created, each in a different zone.

Notice that the Internal IP addresses of these two instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.

2a.To open a command prompt on the my-vm-2 instance, click SSH in its row in the VM instances list.

3.Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

ping my-vm-1

Translating from the web console instruction above to command line 

1b.To see the two instances created, each in a different zone, execute this command:

gcloud compute instances list

Notice that the Internal IP addresses of these two instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.

2b. in the cloud shell, to ssh into my-vm-2, execute this command:

gcloud compute ssh my-vm-2 

3.Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

ping my-vm-1

Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

4.Press Ctrl+C to abort the ping command.

5.Use the ssh command to open a command prompt on my-vm-1:

ssh my-vm-1

If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

6.At the command prompt on my-vm-1, install the Nginx web server:

sudo apt-get install nginx-light -y

7.Use the nano text editor to add a custom message to the home page of the web server:

sudo nano /var/www/html/index.nginx-debian.html

8.Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

Hi from YOUR_NAME

9.Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

10.Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

curl http://localhost/

The response will be the HTML source of the web server's home page, including your line of custom text.

11.To exit the command prompt on my-vm-1, execute this command:

exit

You will return to the command prompt on my-vm-2

12.To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

curl http://my-vm-1/

The response will again be the HTML source of the web server's home page, including your line of custom text.

13a.In the Navigation menu (Navigation menu), click Compute Engine > VM instances.

Translating 13a to command line

to exit command prompt on my-vm-2, execute this command:

exit

you will return to cloud shell

13b. execute this command:

gcloud compute instances list

14.Copy the External IP address for my-vm-1 and paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text.

Note: If you forgot to click Allow HTTP traffic when you created the my-vm-1 VM instance, your attempt to reach your web server's home page will fail. You can add a firewall rule to allow inbound traffic to your instances, although this topic is out of scope for this course.

Congratulations!
In this lab, you created virtual machine (VM) instances in two different zones and connected to them using ping, ssh, and HTTP.

End your lab
When you have completed your lab, click End Lab. Qwiklabs removes the resources you’ve used and cleans the account for you.
