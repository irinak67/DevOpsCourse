# DevOps Course project

## Exercise 1: Docker Locally

	### Build & Test Locally:
		docker-compose up -d
		docker ps
		# Visit http://localhost:80 and http://localhost:5000/api/notes

	### Push Images to Docker Hub:
		# Backend
		docker build -t kirina67/my-backend:latest backend/
		docker push kirina67/my-backend:latest

		# Frontend
		docker build -t kirina67/my-frontend-custom:latest frontend/
		docker push kirina67/my-frontend-custom:latest

## Exercise 2: Manual Deployment to Azure VM

	### Provision VM:
		Ubuntu 24.04 Standard B1s in RG devOps-course_group
	    Public IP: 52.159.236.159

	### SSH into VM:
		ssh azureuser@52.159.236.159

	### Install Docker:
		sudo apt update
		sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
		sudo systemctl enable --now docker
		sudo usermod -aG docker $USER && newgrp docker

	### Open Firewall Ports:
		az vm open-port --resource-group devOps-course_group --name devOps-course --port 8080
		az vm open-port --resource-group devOps-course_group --name devOps-course --port 5000

	### Create Docker Network:
		docker network create app-net

	### Deploy Backend:
		docker pull kirina67/my-backend:latest
		docker run -d --name backend --network app-net -p 5000:5000 kirina67/my-backend:latest

	### Deploy Frontend:
		docker pull kirina67/my-frontend-custom:latest
		docker run -d --name frontend --network app-net -p 8080:80 kirina67/my-frontend-custom:latest

	### Verify Containers:
		docker ps

	### Verify in Browser:
		1. Frontend UI: http://52.159.236.159/
		2. Backend API: http://52.159.236.159:5000/api/notes

