#  Getting Started with Compute Engine
## Objectives

    *Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

    *Create a Compute Engine virtual machine using the gcloud command-line interface.

    *Connect between the two instances.

 ## Steps:   
1.Setting zone environmental varaibles:

gcloud config set compute/zone us-central1-a

gcloud compute zones list | grep us-central1 (To see the zones)

2. Create a virtual machines

gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

3. Creating 2nd VM

gcloud config set compute/zone us-central1-b (set a new environmental variable different from the first zone)
gcloud compute zones list | grep us-central1 (To see the zones again)

gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"


4. exit (To exit the Cloud Shell)


5. Connect between VM instances

ssh into my-vm-2


ping -c 4 my-vm-1

6. Installing nginx

ssh my-vm-1
sudo apt-get update (to update the local repository)
sudo apt-get install nginx-light -y 

7. add a custom message to the home page of the web server

sudo nano /var/www/html/index.nginx-debian.html (to open the home page for editing in nano editor and adding Hi from David)

To confirm that the web server is serving my new page. At the command prompt on my-vm-1, I executed curl http://localhost/

Result: Hi from David

8. exit

9. I execute 
curl http://my-vm-1/


