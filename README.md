## A Simple Notes app for the Cloud application Development Module: Group project

GitHub Actions(CI/CD) Status: <br>
![Build Status](https://github.com/Chamal1120/flask-notes-app/actions/workflows/ci.yml/badge.svg)

##

### Table of Contents

- [Prerequisites](#prerequisites)
- [Run locally](#Run-locally)
- [Run locally with docker](#Run-locally-with-docker)
- [Test the k8s deployment Locally](#Test-the-k8s-deployment-locally)
- [Troubleshooting](#troubleshooting)

### Prerequisites

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/get-started)
- [Minikube](https://minikube.sigs.k8s.io/docs/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Run locally

> NOTE: You need to install [uv](https://docs.astral.sh/uv/) package manager for local testing without docker.

1. clone the repository to your local machine:

```bash
git clone https://github.com/Chamal1120/flask-notes-app.git
cd flask-notes-app
```

2. Install dependancies:

```bash
uv sync
```

3. Run the app:

```bash
uv run flask run
```

4. Now the app is accessible at `localhost:5000`.

### Run locally with docker

1. clone the repository to your local machine (if you already haven't):

```bash
git clone https://github.com/Chamal1120/flask-notes-app.git
cd flask-notes-app
```

2. Build the docker image:

```bash
docker build -t flask-notes-app .
```

3. Run the container:

```bash
docker run -v $(pwd)/database:/app/database -p 5000:5000 flask-notes-app
```

4. Now the app is accessible at `localhost:5000`.

### Test the k8s deployment locally

1. clone the repository to your local machine (if you already haven't):

```bash
git clone https://github.com/Chamal1120/flask-notes-app.git
cd flask-notes-app
```

2. Start Minikube If not running yet:
```bash
minikube start
```

5. Create a secret key for the flask app in the 

4. Apply the deployment.yml file that contains the app's Kubernetes configuration:

```bash
kubectl apply -f deployment.yml
```

This will create a Deployment with 3 replicas and a LoadBalancer service for external access:

5. Check if the pods and services are running correctly:

```bash
kubectl get pods
kubectl get svc flask-notes-service
```

6. Open a seperate terminal window and start Minikube tunnel to expose the service to your local machine (you should keep this running until you finish working with the app):

```bash
minikube tunnel
```

This command will set up a network route from your machine to the Kubernetes cluster.

7. Check the flask-notes-service again:

```bash
kubectl get svc flask-notes-service
```

Now you'll see an external ip is available for the service.

8. Open your browser and navigate to that external ip:

```
http://<external-ip>
```

You should now see the Flask Notes app running locally.

### Troubleshooting

1. "Connection Refused" Error: If you're unable to access the app, ensure that Minikube is running, the tunnel is up, and the correct IP is being used. You can check the service by running:

```bash
kubectl get svc flask-notes-service
```

2. Pods Not Running: If your pods are not starting or are stuck in a crash loop, check the logs for errors:

```bash
kubectl logs <pod-name>
```

3. Minikube Tunnel Issues: If the tunnel is not working, try restarting Minikube:

```bash
minikube stop
minikube start
minikube tunnel
```
