**🚀 End-to-End debugging guide for quick checks and references **

**Case 1: Ansible commands:**

# Check Ansible installation and version
ansible --version

# Dry run to see what would be executed
ansible-playbook -i inventory playbooks/final_play.yml --connection=local --check

# Run with verbose output for debugging
ansible-playbook -i inventory playbooks/final_play.yml --connection=local -vvv

# Check specific task execution
ansible-playbook -i inventory playbooks/final_play.yml --connection=local --start-at-task="TASK_NAME"

# Verify installed components
docker --version
kubectl version --client
helm version
jenkins --version

**Case 2: Docker commands**

# Check Docker daemon status
sudo systemctl status docker
sudo journalctl -u docker -f

# Verify Docker can run containers
docker run hello-world

# Check Docker images and containers
docker images
docker ps -a

# Inspect specific container logs
docker logs <container_id>

# Check Docker storage driver
docker info | grep Storage

**Case 3: Kubernetes commands**

# Detailed node information
kubectl describe nodes

# Check cluster component status
kubectl get componentstatuses

# Verify kubelet service
sudo systemctl status kubelet

# Check cluster configuration
kubectl config view

# Examine pod issues in detail
kubectl describe pods -n kube-system
kubectl logs <pod-name> -n kube-system

# Verify CNI networking
kubectl get pods -n kube-system | grep cni

**Case 4: Helm commands**

# Check Helm installation and version
helm version

# Dry run to see what would be deployed
helm upgrade lightweight-app . --dry-run

# Check release history
helm history lightweight-app

# Rollback if needed
helm rollback lightweight-app <revision>

# Verify all Kubernetes resources
kubectl get all -l app=node-lightweight-app

# Detailed pod debugging
kubectl describe pods -l app=node-lightweight-app
kubectl logs -l app=node-lightweight-app --tail=50

**Case 5: Application deployment verification**

# Detailed pod inspection
kubectl describe pods -l app=node-lightweight-app

# Check pod logs
kubectl logs -l app=node-lightweight-app --tail=100

# Verify service endpoints
kubectl get endpoints
kubectl describe service node-lightweight-app

# Check ingress configuration
kubectl describe ingress node-lightweight-app

# Test application connectivity
kubectl port-forward service/node-lightweight-app 3000:3000 &
curl http://localhost:3000/health

# Verify HPA status
kubectl get hpa
kubectl describe hpa node-lightweight-app

**Case 6: Jenkins verification commands**

# Check Jenkins service status
sudo systemctl status jenkins
sudo systemctl restart jenkins

# View Jenkins logs in real-time
sudo tail -f /var/log/jenkins/jenkins.log

# Check Jenkins port binding
sudo netstat -tlnp | grep 8081

# Verify Jenkins home directory
sudo ls -la /var/lib/jenkins/

# Check Jenkins process
ps aux | grep jenkins

**Case 7: Vault integration commands**

# Check Vault service status
sudo systemctl status vault

# Verify Vault connectivity
vault status

# Check Vault logs
sudo journalctl -u vault -f

# List available secrets engines
vault secrets list

**Case 8: Quick health check command**

# Overall system health
kubectl get nodes,po,svc,ingress
docker ps
helm list
sudo systemctl status docker kubelet jenkins

# Application specific health
kubectl get pods -l app=node-lightweight-app
kubectl logs -l app=node-lightweight-app --tail=10
curl -s http://localhost:3000/health || echo "Health check failed"
