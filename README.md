# QAC Final Project
Final group project following the QAC Final Project Brief (DevOps) due 10th July 2020.

## Index
1. [Brief](#brief)
    - [Project Proposal](#pp)
2. [Trello Board](#trello)

## Brief <a name="brief"></a>
As specified in the project brief, the following applications are to be deployed:
- A front-end version of the PetClinic WebApp coded in angular js (<a href="https://github.com/spring-petclinic/spring-petclinic-angular">Link</a>)
- A back-end, restful API version of the PetClinic WebApp coded in java (<a href="https://github.com/spring-petclinic/spring-petclinic-rest">Link</a>)

Workflows, both deployment and development, must be automated with the following considerations in mind:
- Which tools and technologies, covered over the training period, will be used
- Multiple environments to facilitate testing
- Automated building and continuous deployment based on VCS modifications
- A record of total costs incurred

### Project Proposal <a name="pp"></a>
Our proposal focused on fulfilling the project brief by using the following architecture:
- Infrastructure as Code (IaC) deployed with Terraform
- Configuration Management using Ansible
- Utilising AWS CodePipeline, acting as a CI/CD server via a webhook to this repository
- An AWS EKS Cluster that will run the app
- Monitoring the project by using AWS services such as CloudWatch, CloudTrail and X-Ray

![](images/initial_architecture.PNG)

The above image displays the initial architecture discussed during the first team stand-up. Note, this diagram is not the overall deployment, but the application itself.

An EKS Cluster, consisting of a manager node and two worker nodes, sits inside a subnet, inside a VPC, within the AWS cloud. The Kube (Kubernetes) Manager encompasses a single pod in which three containers are housed: NGINX, front-end and back-end. 

Once the cluster is deployed, the replicas are load-balanced across the worker nodes. NGINX is configured so that vulnerable ports (4200, 9966) are not open to the public, especially those who intend to be malicious. Only port 80 is accessible to the open internet for that reason - this is achieved through an Internet Gateway that is attached to a Route Table associated with the EC2 instances.

## Trello Board <a name="trello"></a>
In terms of project tracking, we used a kanban-style Trello Board. Agile methodology was carried out where possible, in line with the project brief. Multiple sprints were conducted, as well as daily scrums.

## Technologies <a name="technologies"></a>
* The Spring Pet Clinic application is a spring boot application we ran using maven. 
* RDS MySQL database to persist data entered on the website. 
* Ansible to provision VMs.
* Jenkins to automate the building process.
* Docker to containerise the application and docker swarm to deploy the application.
* Terraform to provision AWS resources.
* Trello to track and manage the project.

# Deployment

## CI Pipeline <a name="CI Pipeline"></a>

## MySQL <a name="mysql"></a>
A RDS MySQL database was set up on AWS in order to persist data from the website. This required the application-mysql.properties file to be modified so that the first three lines are uncommented and to include the endpoint for the database, username and password. In order to protect this sensitive information we entered the export command with the values for these varibles in the .bashrc and then used variable substitution in the file. 
### Issues
03/07/20 - The mysql scripts were not executing when the application was ran. Adding 'spring.datasource.initialization-mode=always' to application-mysql.properties resolved the issue. 

## Docker <a name="docker"></a>
The front and back end applications are containerised using docker utilising apline images to reduce the memory usage. Initially it was intended to use kubernetes to deploy the application however after encountering issues we decided to use docker swarm instead. The front end application communicates with the back end through the instruction of the environment.ts, pulling the database information to display on the site and also enabling CRUD functionality. We utilised DockerHub's team repository functionality so that we could all have access from our personal accounts in order to push and retrieve images. This function is free for teams of up to 3 people and $9 a month for teams of any higher number. We decided to keep the cost down by enabling the only docker developers access to the repository. 

## Terraform <a name="terraform"></a>
Terraform was used to provision the AWS resources; EC2 instances including master and swarm worker nodes, an internet gateway, the RDS instance, route table, security groups, subnets and VPC. 

## Ansible <a name="ansible"></a>
Ansible was used to provision the VMs with docker and set up the master and nodes as part of a swarm. 

## Jenkins <a name="jenkins"></a>
Jenkins was used to provision the manager node with docker and ansible, and deploy ansible to run the scripts.

# Billing 
| Date | Spend ($) | Resources |
| --- | ---:| --- |
01/07/20 | 0.5 | 2 t2.micro instances and RDS |
02/07/20 | 0.13 | 2 t2.micro instances and RDS | 
03/07/20 | 0.50 | 4 t2.micro, RDS and t2.small |
04/07/20 | 1.34 | 4 t2.micro, RDS and t2.small |
05/07/20 | 1.79 | 1 t2.micro, RDS, 2 t2.small and 1 t2.medium |
06/07/20 | 3.89 | 2 t2.micro, RDS, 2 t2.small and 2 t2.medium, EKS |
07/07/20 | 5.68 | 2 t2.micro, RDS, 2 t2.small and 2 t2.medium, EKS |
08/07/20 | 8.48 |  2 t2.micro, RDS and 2 t2.medium ||

For this project we had a budget of £20. Initially we tried to stay within the free tier usage that AWS offers, however the apps required a higher memory and CPU usage than what the free tier instances offered. We gradually increased the size of the instances which in turn incurred a higher cost. In addition, the EKS also increased the charges, after not being sucessful with Kubernetes we decided not to use this service. 
# Risk Tracking
## Initial Risk Assessment

| Number |Date | Risk | Response Strategy | Outcome | Likelihood | Impact | Proximity |
| --- |---  | ---   | ------------------ | ------- | ---------| --------| ---------|
1 | 01/07/20 | Cost exceeds budget | Set up billing alerts and routinely check costs | The project is completed within budget with only necessary costs spent | Low | Low | Once AWS resources are used | 
2 | 01/07/20 | Project not completed on time | Use trello for project tracking and managing | Time is managed effectively and project is completed on time | Low | High | Immediate |
3 | 01/07/20 | Service failure | Implement testing in order to ensure services work as expected | All systems run as expected | Medium | High | Development Stage |
4 | 01/07/20 | COVID 19 | Adhere to government guidelines and distribute work between healthy team members | Project is completed on time and meets the MVP | Low | Medium | Immediate |
5 | 01/07/20 | General illness/absence | Work is distributed between healthy team members | Project is completed on time and meets the MVP | Low | Medium | Immediate |
6 | 02/07/20 | Diminished communication due to working remotely | Utilise trello and teams to plan tasks and hold frequent meetings | Project is completed on time and meets the MVP | Low | Medium | Immediate |
7 | 02/07/20 | Link failure | Use the terminal to monitor service responses | Links are working and delivering expected data | Low | Medium | Development stage |
8 | 02/07/20 | Secret data stolen | Use environment variables | Sensitive data is not exposed on github and only known within the development team | Low | High | Development stage |
9 | 02/07/20 | Developers knowledge not sufficient to complete the project | Review materials and research any unknown areas, contact other team members and post issues on Trello | Developers have complete understanding of the technologies used and this is reflected in all aspects of the project | Medium | High | Development stage |
10 | 02/07/20  | Man in the middle attack | Limit IP access to the machines, make use of VPCs, route tables and security groups | Only authorised access to the machines allowed | Low | High | Development Stage |

![](images/initialriskmatrix.png)

### Analysis
| Number | Analysis | 
|--- | ---|
1 | We had been given a budget of £20 by the Academy to complete the project. We started off by using the smallest instances on the free tier however after running the app and other software we found we did not have enough memory or CPU. Therefore, we decided to increase the size of our instances which also increased our costs. We made use of the AWS billing dashboard which helped us track our costs. After decreasing the size of the images to alpine images we were able to keep the memory and CPU usage down which kept our costs at a reduced amount as well. Another option we explored was to only copy the necessary files into the docker container and run it using a java -jar command rather than maven spring boot, however we felt that we did not have enough time to utilise this strategy. |
2 | We utilised resources such as daily scrums, meetings and Trello to keep track of our progress and manage our time effectively. We spent a lot of time working with Kubernetes which towards the end of the sprint we decided to swap out for Docker Swarm as we were confident we could get that working and we were conscious of not meeting the deadline |
3 | Service Failure | After struggling to get Kubernetes to work, we decided to utilise Docker Swarm instead so that we could meet the MVP and deadline. We spent a lot of time trouble shooting and making notes of processes that were sucessful should we need to pass them on. |
4 | We conducted daily scrums in which we checked for any COVID related symptoms or worries. None of the team members were ill at any point |
5 | We conducted daily scrums in which we checked for any illness related symptoms and where team members could communicate any required absences. None of the team members had any illnesses or took any absences. |
6 | We conducted daily scrums and frequent meetings, utilising Teams screen share and instant messaging functionality to keep in contact throughout the project. We also used Trello to keep track of tasks. This was not an issue for us. |
7 | The frontend had trouble communicating with the backend so we had to use the IP address instead of the container name. |
8 | Our information was not exposed as we made use of environment variables. |
9 | We communicated well to help eachother solve any issues we were facing. We also spent a lot of time researching any areas we weren't familiar with and any areas we were stuck on until we came to a solution. |
10 | We made use of security measures to prevent any cyber attacks. |

# Security 
## IAM 
We set up IAM users and gave specific permission policies to the developers depending on their roles in the project. The IAM users had password policies, multi-factor authentication and security credentials in order to keep accounts secure.
## CloudTrail
Cloud trail provided a history of each users activity on the AWS resources so it was easy to keep track of activity on the account. We opted out of creating a cloud trail as this could have incurred extra cost and the team had good communication throughout the project. 
## CloudWatch