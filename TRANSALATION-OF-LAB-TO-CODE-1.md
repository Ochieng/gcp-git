# Getting Started with VPC Networking
## Objectives:
    *Explore the default VPC network
    *Create an auto mode network with firewall rules
    *Create VM instances using Compute Engine
    *Explore the connectivity for VM instances

### Steps: 
1). Create an auto mode VPC network with Firewall rules for mynetwork

    --------------------------------------------------------------


gcloud compute networks create myvpcnetwork --subnet-mode=auto 

gcloud compute firewall-rules create mynetwork-allow-icmp myvpcnetwork  --direction=INGRESS --source-ranges=0.0.0.0/0 --action=ALLOW --rules=icmp

gcloud compute firewall-rules create mynetwork-allow-internal myvpcnetwork --direction=INGRESS--source-ranges=10.128.0.0/9 --action=ALLOW --rules=all

gcloud compute firewall-rules create mynetwork-allow-rdp myvpcnetwork  --direction=INGRESS --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:3389

gcloud compute firewall-rules create mynetwork-allow-ssh myvpcnetwork --direction=INGRESS --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:22


2). Create a VM instance in us-central1

    -----------------------------------
    
gcloud compute instances create mynet-us-vm --zone=us-central1-c --subnet=mynetwork --image=debian-10 --boot-disk-size=10GB --boot-disk-device-name=mynet-us-vm

3). Create a VM instance in europe-west1

    ---------------------------------------
    
gcloud compute instances create mynet-eu-vm --zone=us-west1-c --subnet=mynetwork   --image=debian-10 --boot-disk-device-name=mynet-eu-vm


 ### Explore the connectivity for VM instances
 
    -------------------------------------------------

4). Removing the allow-icmp firewall rule and try to ping the internal and external IP address of mynet-eu-vm.

    ----------------------------------------------------------------------------------------------------------

Remove the allow-icmp firewall rules then ping to the mynet-us-vm (10.128.0.2) internal IP address
ping -c 3 10.128.0.2
Result=ping succeeds


Remove the allow-icmp firewall rules then ping to the mynet-us-vm (35.239.34.82) external IP address
ping -c 3 35.239.34.82 US external IP Address:
Result=ping fails


5). Remove the allow-internal firewall rules

    ----------------------------------------
    
Remove the allow-internal firewall rule and try to ping the internal IP address of mynet-eu-vm(10.132.0.2).

Then test connectivity to mynet-eu-vm's internal IP:
ping -c 3 10.132.0.2
Result=Connection times out

6). Remove the allow-ssh firewall rules

    -----------------------------------
    
Remove the allow-ssh firewall rule and try to SSH to mynet-us-vm.

For mynet-us-vm(35.239.34.82), click SSH to launch a terminal and connect.

Result= "unable to connect to the VM on port 22". Reason: because the allow-ssh firewall rule has been deleted.








