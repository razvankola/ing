Arhitecture Overview:

CI: The pipeline is reponsible only for build-related tasks:
- Checkout source code
- Build the applicatiom using Maven
- Produce a versioned petclinic.jar
- Publich the JAR as a pipeline artifact

Configuration: Ansible
Ansible is used to configure and manage runtime environment ( OS packages, Users & permissions, Services - systemd, Monitoring stack)

Playbook structure:
ansible/ 
├── site.yml 
├── group_vars/ 
│ └── all.yml 
└── roles/ 
    ├── common/ 
    ├── app/ 
    ├── monitoring/


Common role is responsible for baseline system configuration such as openjdk-17-jdk, maven, curl,wget, ca-certificates installation
App role is responsible for running the application not building it -> Copy petclinic.jar from CI artifact location and creating a systemd unit, reload systemd and enable+start the service
Monitoring role is responsible for configuring the observability components, installed as systemd services. 
