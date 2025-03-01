[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/ao4Zt6N9)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=17383848&assignment_repo_type=AssignmentRepo)
# Mobile Computing README template

Template with examples
<!-- PROJECT LOGO -->
<img src="resources/images/FRA-UAS_Logo_rgb.jpg" width="150"/>

<h1 align="center">MobCom-Giga-Force</h1>
<p align="center">
    <strong>Roaming-AMF/UPF/SMF-Traffic</strong>
    <br>
    Group-4
    <br>
    Members-matriculation number
    <br>
    Syeda Ayesha Asad-1512083
    <br>
    Rubi Kiran-1504617
    <br>
    Chaitra Bhandari-1504352
    <br>
    Amit Maity-1502808
</p>
<br/>

## Contents

*   [Overview](#Overview)
*   [Project Overview (5G Roaming Deployment)](#ProjectOverview-(5G-Roaming-Deployment))
*   [Project Network Architecture](#Project-Network-Architecture)
*   [Prerequisites](#Prerequisites)
*   [Deployment Instructions](Deployment-Instructions)
*   [Roaming Deployment](#RoamingDeployment)
    * [Used Technologies](#UsedTechnologies)
    * [Testing/Verification](#Testing/Verification)
    * [Logging](#logging)
*   [Conclusion](#conclusion)
*   [Sources](#sources)


## Overview
With the rapid advancement of mobile communications, 5G networks have become a key enabler of high-speed, low-latency, and ultra-reliable connectivity.
The Mobile Computing Project focuses on designing, implementing, and analyzing a comprehensive 5G Core Network architecture. It also focuses on the implementation of a 5G Core Network (5GC) using Open5GS, along with a 5G Radio Access Network (RAN) and User Equipment (UE) simulation usingPacketrusher. Additionally, the project includes managing and orchestrating 5G network functions (NFs) using Docker and Docker Compose to ensure network scalability, reliability, and fault tolerance. This collaborative project integrates various essential components of the 5G ecosystem, offering hands-on experience in modern mobile network technologies. 

## Project Overview (5G Roaming Deployment)

This project implements a 5G roaming architecture with two separate 5G Core Networks (CNs): a Home 5G CN (PLMN: 262/01) and a Visited 5G CN (PLMN: 262/02). Each CN includes key network functions such as NRF, AMF, SMF, and UPF. The two networks are interconnected via a roaming interface, allowing seamless mobility for users between them. The deployment is designed to work with the Open5GS roaming feature, utilizing PacketRusher for the 5G Radio Access Network (gNB and UE). The architecture enables interoperability between two distinct 5G Core Networks (home and a visited network) while leveraging a shared MongoDB database exposed via TCP port 27017 for user authentication. The 5G RAN consists of multiple gNBs based on Packetrusher, supporting multiple 5G UEs. This setup enables testing of roaming scenarios, performance, and scalability within a containerized environment using Docker and Docker Compose.

## Project Network Architecture
                                                        +-----------------------+  
                                                        |      Home 5G CN       |  
                                                        |  (PLMN: 262/01)       |  
                                                        |                       |  
                                                        | NRF, AMF, SMF, UPF    |  
                                                        +-----------------------+  
                                                                 |  
                                                         (Roaming Interface)  
                                                                 |  
                                                        +-----------------------+  
                                                        |    Visited 5G CN      |  
                                                        |  (PLMN: 262/02)       |  
                                                        | NRF, AMF, SMF, UPF    |  
                                                        +-----------------------+  
                                                                 |  
                                                         ----------------------  
                                                        |    5G RAN (gNBs)    |  
                                                        | Packetrusher gNBs   |  
                                                         ----------------------  
                                                                 |  
                                                         Multiple 5G UEs                                                 

This configuration focuses on implementing a minimal set of essential network functions to establish a functioning 5G roaming setup. Authentication is handled within the home network database, ensuring that user credentials remain within the home PLMN while the visited network enforces roaming policies based on PCF configurations.

Home Network (PLMN 262 01)

* Network Repository Function (NRF) – Service registration and discovery
* Authentication Server Function (AUSF) – User authentication
* Unified Data Management (UDM) – Subscription data handling
* Unified Data Repository (UDR) – Storage for subscription information
* Policy Control Function (PCF) – Policy enforcement
* Session Management Function (SMF) – Manages user sessions
* User Plane Function (UPF) – Traffic routing and forwarding
* Access and Mobility Management Function (AMF) – Handles mobility and access control

Visited Network (PLMN 262 02)

* Contains a subset of 5G Core functions necessary for roaming operations
* Includes its own NRF, AMF, SMF, PCF, and UPF
* Relies on the home network for user authentication
  
### Prerequisites:

1.  Operating System: Ubuntu 20.04.
    <br/>* Other Linux distributions may work but could require additional dependencies.<br/>
         * Ensure your system is up to date (eg: sudo apt update && sudo apt upgrade -y).
         * MongoDB version 4.2
2. Docker and Docker Compose.
   <br/> * Since Open5GS will run in a containerized environment, need Docker and Docker Compose installed.
   <br/> * Install Docker.
   <br/>  * Start and enable docker.
   <br/> * Install Docker compose.
   <br/> * Verify the installation.


## Deployment Instructions:

 <br/>* Step 1: Clone the Repository ( https://github.com/Borjis131/docker-open5gs/tree/main )
 <br/>* Step 2: Setup Docker & Docker Compose 
 <br/>* Step 3: Installing MongoDB
 <br/>* Step 4: Configure the Roaming Setup
 <br/>* Step 5: Start the 5G Core & RAN & installing Open5gs
 <br/>* Step 6: Verify Running Containers/images
 <br/>* Step 7: Registering User Equipment & installing  Packetrushers
 <br/>* Step 8: Testing & Verification using-iperf3.

## Configuration Files
Modify the following .yaml files to adapt the deployment:
1. NRF Configuration (nrf.yaml).
2. AMF Configuration (amf.yaml). 
3. UPF Configuration (upf.yaml).
4. SMF Configuration (upf.yaml).

## Roaming Deployment

The roaming deployment is prepared to work with Open5GS Roaming feature and PacketRusher(gNB and UE), only exposing the MongoDB database using TCP port 27017. Two 5G Cores are deployed, one for the visited network (with PLMN 262 02) and one for the home network (with PLMN 262 01). This deploys the all the Network Functions. One database is used, for the home network user authentication. So, in this case, the UE with IMSI 262011234567891. The Roaming agreement is present on the visited network PCF (v-pcf) configuration file, under the policy section. That is because the visited network does not have any information for the user apart from that.

The communication between the two 5G Cores is done via the SEPP Network Functions.

![image](https://github.com/user-attachments/assets/4faf022a-8015-4fe1-a3b4-d263b8747433)

Even though the two SBI sections are separated in the diagram, only one docker network is created so in reality everything is interconnected.

### Used Technologies
1. Open5GS (5G Core Network).
2. Packetrusher was installed from GitHub.
3. Traffic Generation Tool used iperf3.
4. Implementation of a management and orchestration solution for Docker containers and 5G NFs.

### Testing/Verification

1. First the reachability of the server is checked.
   
   ![´Ping](https://github.com/user-attachments/assets/cd28cec1-83ac-4003-a080-2dce2ee1edb4)

   
2. Run the iperf3 client from another machine to establish a connection with the UPF server using the ( iperf3 -s ) command.

3. Start the iperf3 server on the UPF machine to listen for incoming connections.
   
   ![Iperf](https://github.com/user-attachments/assets/5f9d54aa-c497-4c40-83bc-5934af8f8f78)

4. Testing and verification of UPF Server and Client using iperf3 for traffic throughput generation.

   ![iperf3@UPF](https://github.com/user-attachments/assets/d0895082-11c8-4a74-a69b-50b46258b4cd)

5. Check the output for bandwidth, latency, and packet loss to verify network performance.

   ![Test UDP connection](https://github.com/user-attachments/assets/56f545b6-266e-4b18-bd4c-c1e324432bbf)

### Logging

1. The following figure show the logs of UE with vrf interface.

![WhatsApp Image 2025-03-01 at 21 54 02_3926b7cf](https://github.com/user-attachments/assets/c73c59f7-0213-4bd4-8283-b2559f2387ad)

2. The image shows the Open5GS WebUI, a web-based interface for managing and configuring mobile network subscribers.
3. The subscriber ID (IMSI) displayed is 262011234567890, which is used for identifying the user in the mobile network.
   
![WhatsApp Image 2025-03-01 at 21 58 13_e89a2984](https://github.com/user-attachments/assets/284ff76d-0613-446e-b4e2-72187fbfc22e)



## Building the Project( Using Docker-Compose)

1. Create the Base Open5GS Image– Run the command to build the base-open5gs image, which serves as the foundation for the 5G Core Network deployment.
2. Build and Deploy the 5GC Network-(docker compose -f compose-files/roaming/docker-compose.yaml --env-file=.env up -d.).
3. Execute the Docker Compose command to build all necessary images, configure the environment, and deploy the selected 5G Core Network architecture.
4. Verify Deployment – Ensure all containers are running properly, check logs for any errors, and confirm successful communication between the 5GC, RAN, and simulated UE.
   
## Conclusion 

The Mobile Computing Project provides a practical and in-depth exploration of 5G network architecture, implementation, and management. By leveraging Open5GS for the 5G Core Network, Packetrusher for RAN and UE simulation, and Docker & Docker Compose for orchestration, the project ensures a scalable, reliable, and fault-tolerant network. This hands-on approach enables a deeper understanding of modern mobile networks, preparing participants to tackle real-world challenges in 5G deployment and innovation. Ultimately, the project contributes to advancing next-generation communication systems by integrating key components of the 5G ecosystem.
   
## Sources

 <br/>* https://github.com/Borjis131/docker-open5gs/tree/main
 <br/>* https://github.com/open5gs/open5gs
 <br/>* https://medium.com/@vidime.sa.buduci.rok/5g-roaming-open5gs-packet-rusher-dacb34f3497c
 <br/>* https://github.com/HewlettPackard/PacketRusher/blob/main/docker/docker-compose.yml
 <br/>* https://open5gs.org/open5gs/docs/
