**Purpose** : End-to-end CI/CD pipeline demonstrating Automation using various tools in DevOps world

**Scope** : Major tools and technologies used: 
            - Ansible
            - Helm
            - Kubernetes
            - Docker
            - Jenkins
            - Linux Ubuntu headless
            - Vault
            - Anlytical thinking and debugging skills

**Objective** : Automate, secure, and deploy a small web application in a headless Ubuntu environment with Jenkins. 

**Git repository details** : Below are the respective folder links on git for each of these technologies evaluation :
                             Jenkins: https://github.com/anubha2709/devops-pipeline-demo/tree/main/jenkins                             
                             Helm charts and kubernetes: https://github.com/anubha2709/devops-pipeline-demo/tree/main/kubernetes
                             Ansible playbooks: https://github.com/anubha2709/devops-pipeline-demo/tree/main/playbook
                             Dockerfile + Node.js application code: https://github.com/anubha2709/devops-pipeline-demo/tree/main/application

**Setup/Usage Instructions** : 
    1. From terminal, use the credentials provided by email for security reasons to connect to the Ubuntu machine.
    2. SSH to the machine : ssh devopsuser@given.ip.address.fromemail and when prompted provide the password. 
    3. To check the code details hands on, use the following commands: 
                                        - Clone the repository
                                                - git clone https://github.com/anubha2709/devops-pipeline-demo.git
                                                - cd devops-pipeline-demo
    4. To run the Ansible playbook manually, use : ansible-playbook -i inventory playbooks/final_play.yml --connection=local
    5. To run the Helm configuration manually, use : helm upgrade lightweight-app .
    6. Access Jenkins ui page and logs in server: "http://98.95.27.246:8081" . The access might be restricted within organization policies. Please open in developer mode in           case it doesn load otherwise. 

**Setup Details** : 
    1. **Ubuntu headless** : Provisioned through AWS EC2 instance and uses a 24.04 Ubuntu version
    2. **Ansible version used** : Installed compatible ansible [core 2.19.4] version
    3. **Tasks handled by Ansible** : 
                        - Environment update, cleanup any existing repo 
                        - Installations: Docker, prerequisite packages, kubelet, kubeadm, kubectl, Helm, Jenkins
                        - Disable Swap
                        - Creation of non-root user with sudo privilges and ssh access : devopsuser
                        - CNI deployment for networking
                        - UFW configuration
                        - NGINX test container deployment
    4. **Docker containerization details** : Source file for the lightweight web app, Dockerfile created with multi-stage build optimization and Health check endpoint enabled ,                                             .dockerignore file is provided for reference.
    5. **Helm Charts and Kubernetes Configuration** : The latest Helm version(v3.19.0) is installed. Helm here is used for the following configurations with Kubernetes: 
                                                    - deployment.yaml : Specifies the number of replicas and container configuration using values from the Helm chart. It pulls                                                                         the latest image, exposes the port, and injects environment variables from configmap.yaml
                                                    - Chart.yaml : Defines metadata for lightweight-app
                                                    - ingress.yaml : Defines a Kubernetes Ingress resource for the node-lightweight-app, routing external HTTP traffic to the                                                                        app's service on a specified host and port. It activates only if .Values.ingress.enabled is true, and                                                                           dynamically includes annotations and configuration from the Helm values file.
                                                    - service.yaml : Kubernetes Service that exposes the node-lightweight-app on a specified port and type, routing traffic to                                                                       matching pods based on the app label.
                                                    - configmap.yaml : Creates a ConfigMap that sets the NODE_ENV variable using a value from the chart configuration.
                                                    - values.yaml : Sets up a Node.js app with 2 replicas, using the latest image and exposing it via a ClusterIP service on                                                                        port 3000. It enables ingress routing and horizontal pod autoscaling between 2 and 5 replicas based on 50%                                                                      CPU utilization, with the environment set to production.
                                                    - hpa.yaml : Creates a HorizontalPodAutoscaler for the node-lightweight-app deployment, scaling the number of pods based on                                                                  CPU usage. It adjusts replicas between defined minimum and maximum thresholds when CPU utilization crosses the                                                                  specified target percentage.
     6. **Secrets management** : Vault is used in this solution to store the secrets for DockerHub, Jenkins keys, and vault secret and role id which helps integrate securely with                               Jenkins
     7. **Jenkins / CI/CD explanation** : Below are the tasks handled by Jenkins: 
                             - Clone source code from git repository main branch
                             - Build Docker image
                             - Test Docker image against health check endpoint
                             - Push securely to DockerHub using Vault secrets
                             - Deploy to Kubernetes

**Assumptions and Limitations** : While the implementation process, following assumptions/strategic decisions were made : 
                                    - Used AWS EC2 instance with Ubuntu 24.04 version
                                    - With AWS resource contraint, implementation is optimized to work in available memory capacity
                                    - Vault tokens and Jenkins credentials are short-lived for demo purposes
                                    - Application has no external database dependencies for simplicity
                                    - Kubernetes cluster runs as single node; no high-availability configurations
                                    - Vault uses local file storage rather than cloud storage backend
                                    - Uses latest tags for simplicity rather than immutable versioning




Document version: 
Last Updated: November 2024
Maintainer: DevOps Team



    
                             
