Voicemail Web Service
Overview
The Voicemail Web Service project is a Flask-based REST API that allows users to send, receive, and manage voicemail messages. This project is containerized using Docker and deployed on AWS using Kubernetes (EKS).

Table of Contents
Overview
Features
Prerequisites
Setup
Docker Setup
Kubernetes Deployment
Accessing the Service
Endpoints

Send Voicemails: Users can record and send voicemails.
Retrieve Voicemails: Users can fetch and listen to received voicemails.
Delete Voicemails: Allows users to delete specific voicemails.
Prerequisites
To set up and run this project, ensure you have the following installed:

Python 3.x and Flask
Docker
kubectl and eksctl for Kubernetes setup on AWS EKS
AWS CLI for configuring AWS credentials
Setup
Clone the repository

bash
Copy code
git clone https://github.com/yourusername/voicemail-web-service.git
cd voicemail-web-service
Install Python dependencies

bash
Copy code
pip install -r requirements.txt
Run Flask Application Locally

bash
Copy code
python3 app.py
Verify the API is working (Default port is 5000)

bash
Copy code
curl http://127.0.0.1:5000/
Docker Setup
Build Docker Image

bash
Copy code
docker build -t voicemail-service:latest .
Run Docker Container

bash
Copy code
docker run -p 5000:5000 voicemail-service:latest
Push Docker Image to a Repository (Docker Hub or Amazon ECR)

Docker Hub:

code
docker tag voicemail-service:latest your-dockerhub-username/voicemail-service:latest
docker push your-dockerhub-username/voicemail-service:latest

Amazon ECR:

code
aws ecr create-repository --repository-name voicemail-service
aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-2.amazonaws.com
docker tag voicemail-service:latest <aws_account_id>.dkr.ecr.us-east-2.amazonaws.com/voicemail-service:latest
docker push <aws_account_id>.dkr.ecr.us-east-2.amazonaws.com/voicemail-service:latest
Kubernetes Deployment
Create an EKS Cluster

code
eksctl create cluster --name voicemail-cluster --region us-east-2 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed
Deploy to Kubernetes Update the image field in voicemail-deployment.yaml to point to your Docker image repository, then apply the file:

code 

kubectl apply -f voicemail-deployment.yaml
Expose the Service Check the service for an external IP:

 code
kubectl get services
Accessing the Service
Once the service is running, use the external IP provided by the LoadBalancer to access the Voicemail API:

 code
http://<external-ip>:80
Endpoints
POST /voicemail - Record and send a voicemail.
GET /voicemails - Retrieve all voicemails.
DELETE /voicemail/{id} - Delete a specific voicemail.
