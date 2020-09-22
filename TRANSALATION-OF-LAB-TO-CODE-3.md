#  Virtual Private Networks (VPN) 
## Objectives

    *Create VPN gateways in each network

    *Create VPN tunnels between the gateways

    *Verify VPN connectivity


## Steps:
1. Create the VPN gateways and tunnels in us-central1 named vpn-1-static-ip

gcloud compute  target-vpn-gateways create "vpn-1" --region "us-central1" --network "vpn-network-1"

gcloud compute  forwarding-rules create "vpn-1-rule-esp" --region "us-central1" --address "34.70.138.155" --target-vpn-gateway "vpn-1"

gcloud compute  forwarding-rules create "vpn-1-rule-udp500" --region "us-central1" --address "34.70.138.155" --ip-protocol "UDP" --ports "500" --target-vpn-gateway "vpn-1"

gcloud compute  forwarding-rules create "vpn-1-rule-udp4500" --region "us-central1" --address "34.70.138.155" --ip-protocol "UDP" --ports "4500" --target-vpn-gateway "vpn-1"



2. Create the vpn-1 gateway with the following properties: Name=vpn-1, Network=vpn-network-1, Region=us-central1, IP address=vpn-1-static-ip

gcloud compute  vpn-tunnels create "tunnel1to2" --region "us-central1" --peer-address "34.76.136.144" --shared-secret "gcprocks" --local-traffic-selector "0.0.0.0/0" --target-vpn-gateway "vpn-1"

3. Creating a tunnel1to2: Name=tunnel1to2, Remote peer IP address=34.76.136.144, IKE pre-shared key=gcprocks, Routing options-Route-based, Remote network IP ranges=10.1.3.0/24:

gcloud compute  routes create "tunnel1to2-route-1" --network "vpn-network-1" --next-hop-vpn-tunnel "tunnel1to2" --next-hop-vpn-tunnel-region "us-central1" --destination-range "10.1.3.0/24"

4. Create the vpn-2 gateway with the following properties: Name=vpn-2, Network=vpn-network-2, Region=europe-west1, IP address=34.76.136.144:
5. Create the VPN gateways in europe-west1 named vpn-2-static-ip
gcloud compute  target-vpn-gateways create "vpn-2" --region "europe-west1" --network "vpn-network-2"

gcloud compute  forwarding-rules create "vpn-2-rule-esp" --region "europe-west1" --address "34.76.136.144" --target-vpn-gateway "vpn-2"

gcloud compute  forwarding-rules create "vpn-2-rule-udp500" --region "europe-west1" --address "34.76.136.144" --ip-protocol "UDP" --ports "500" --target-vpn-gateway "vpn-2"

gcloud compute  forwarding-rules create "vpn-2-rule-udp4500" --region "europe-west1" --address "34.76.136.144" --ip-protocol "UDP" --ports "4500" --target-vpn-gateway "vpn-2"

6. Create tunnels tunnel2to1: Name=tunnel2to1, Remote peer IP address=34.70.138.155, IKE pre-shared key=gcprocks,
Routing options=Route-based, Remote network IP ranges=10.5.4.0/24:
gcloud compute  vpn-tunnels create "tunnel2to1" --region "europe-west1" --peer-address "34.70.138.155" --shared-secret "gcprocks" --ike-version "2" --local-traffic-selector "0.0.0.0/0" --target-vpn-gateway "vpn-2"

gcloud compute  routes create "tunnel2to1-route-1" --network "vpn-network-2" --next-hop-vpn-tunnel "tunnel2to1" --next-hop-vpn-tunnel-region "europe-west1" --destination-range "10.5.4.0/24"

7. Verify server-1 to server-2 connectivity:
From server-1 ping server-2 IP address:

ping -c 3 34.76.136.144

Result: Response from server-2

8. Verify server-2 to server-1 connectivity:
From server-2 ping server1 IP address:

ping -c 3 34.70.138.155
Result: Response from server-1



