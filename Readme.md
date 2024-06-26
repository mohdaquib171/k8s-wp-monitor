# Setting up a production-grade WordPress app on Kubernetes.
## Part 1: Run a Production Grade WordPress App on Kubernetes

### Step 1: Set Up Your EC2 Instance
1. **Update and Install Necessary Packages**:
   ```sh
    sudo apt-get update
    sudo apt-get install -y docker.io
    sudo systemctl start docker
    sudo systemctl enable docker
    sudo usermod -aG docker $USER
   ```
   Log out and log back in to apply the Docker group changes.

2. **Install Kubernetes Tools**:
	1. Update the apt package index and install packages needed to use the Kubernetes apt repository:
		```sh
	   sudo apt-get update
	   sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
		```
	
	2. Download the public signing key for the Kubernetes package repositories. The same signing key is used for all repositories so you can disregard the version in the URL:
		```sh
		curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
		sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg 
		```

	3. Add the appropriate Kubernetes apt repository. If you want to use Kubernetes version different than v1.30, replace v1.30 with the desired minor version in the command below:
		```sh
		echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
		sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   
		```
	4. Update apt package index, then install kubectl:
		```sh
		sudo apt-get update
		sudo apt-get install -y kubectl
		```

3.  **Install, Start and Verify Minikube**:
	1. Install 
		```sh
		curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
		sudo dpkg -i minikube_latest_amd64.deb
                OR
		curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
		sudo install minikube-linux-amd64 /usr/local/bin/minikube
		```
	
	2. Start Minikube:
		```
		minikube start
		   OR
		minikube start --driver=docker
		```
	
	3. Verify Minikube:
		```
		kubectl get nodes
		```

### Step 2: Create Dockerfiles
1. **WordPress Dockerfile**:
   Create a file named `Dockerfile.wordpress`:

2. **MySQL Dockerfile**:
   Create a file named `Dockerfile.mysql`:

3. **Dockerfile for Nginx with OpenResty and Lua**:
    Create a file named `Dockerfile.nginx`:

### Step 3: Build and Push Docker Images to Docker Hub
```sh
docker build -t mohdaquib/wordpress:latest -f Dockerfile.wordpress .
docker build -t mohdaquib/mysql:5.7 -f Dockerfile.mysql .
docker build -t mohdaquib/nginx:latest -f Dockerfile.nginx .

docker login

docker push mohdaquib/wordpress:latest
docker push mohdaquib/mysql:5.7
docker push mohdaquib/nginx:latest
```


### Step 4: Create K8s Manifests

1. **Create Kubernetes Deployment and Service for MySQL**:
   Create a file named `mysql-deployment.yaml`:
 
2. **Create Kubernetes Deployment and Service for WordPress**:
   Create a file named `wordpress-deployment.yaml`:

3. **Create Kubernetes Deployment and Service for Nginx**:
   Create a file named `nginx-deployment.yaml`:


### Step 6: Use Helm Charts for Deployment

1. **Install Helm**:
   ```sh
   sudo snap install helm --classic
   ```

2. **Create Helm Repository for WordPress**:
   ```sh
   helm create wordpress
   ```

3. **Install WordPress Using Helm**:
   ```sh
   helm install my-release ./wordpress
   ```

### Step 7: Clean Up with Helm
1. **Uninstall the Helm Release**:
   ```sh
   helm delete my-release
   ```


---