## A Simple Notes app for the Cloud application Development Module: Group project

GitHub Actions(CI/CD) Status: <br>
![Build Status](https://github.com/Chamal1120/flask-notes-app/actions/workflows/test.yml/badge.svg)
![Build Status](https://github.com/Chamal1120/flask-notes-app/actions/workflows/ci.yml/badge.svg)

##

## Preview

<image src="previews/preview1.webp"/>

## Table of Contents

- [Prerequisites](#prerequisites)
- [Run locally](#Run-locally)
- [Run locally with docker](#Run-locally-with-docker)
- [Test the k8s deployment Locally](#Test-the-k8s-deployment-locally)
- [Troubleshooting](#troubleshooting)

## Prerequisites

#### Common
- [Git](https://git-scm.com/)

#### For local development
- [uv](https://docs.astral.sh/uv/) 
- [sqlite3](https://www.sqlite.org/)
- [nodejs](https://nodejs.org/en)
- [pnpm](https://pnpm.io/)

#### For running docker image
- [Docker](https://www.docker.com/get-started)

#### For testing K8s locally
- [Docker](https://www.docker.com/get-started)
- [Minikube](https://minikube.sigs.k8s.io/docs/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Run and Develop locally

1. clone the repository to your local machine:

```bash
git clone https://github.com/Chamal1120/flask-notes-app.git
cd flask-notes-app
```

2. Install the dependancies:

```bash
uv sync
pnpm i
```

3. Enable the flask debug mode to get hot reload working.

For Unix
```bash
export FLASK_DEBUG=1 # Set the variable to 1 in the current terminal session
```

For Windows
```powershell
$env:FLASK_DEBUG=1 # Set the variable to 1 in the current powershell session
```

_NOTE: If you want to make it global (means you don't like to set it everytime before you run) add it to your shell configuration._

4. Run the app:

```bash
uv run flask run
```

5. Now the app is accessible at `localhost:5000`.

#### NOTE: If you are adding custom stylings, themes or color pallets to tailwind, you need to rebuild the tailwind css file.

The following command will start a service that listens for changes in the main css file and builds the tailwind css file.

```bash
npx tailwindcss -i ./src/input.css -o ./static/styles.css --watch
```

## Run locally with docker

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

## Test the k8s deployment (locally)

1. clone the repository to your local machine (if you already haven't):

```bash
git clone https://github.com/Chamal1120/flask-notes-app.git
cd flask-notes-app
```

2. Add a k8s secret for the flask app (Otherwise it will use the default one which dangerous in production).

```bash
kubectl create secret generic flask-notes-secret --from-literal=SECRET_KEY=<your_secure_key>
```

> [!TIP]
> You can use python to create a secure key
> ```bash
> python -c "import secrets; print(secrets.token_hex(32))"
> ```

3. Start Minikube If not running yet:
```bash
minikube start
```

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

## Summery Preview

<image src="previews/preview2.webp"/>

## Troubleshooting

1. Minikube is failing to start: Try following two commands and restart your pc. Then try again.

```bash
minikube delete
rm -rf ~/.minikube # Only for Unix-like systems
```

2. "Connection Refused" Error: If you're unable to access the app, ensure that Minikube is running, the tunnel is up, and the correct IP is being used. You can check the service by running:

```bash
kubectl get svc flask-notes-service
```

3. Pods Not Running: If your pods are not starting or are stuck in a crash loop, check the logs for errors:

```bash
kubectl logs <pod-name>
```

4. `db_path` not found issue in pod's logs: Try stoping and starting minikube again. Sometimes minikube fails to enable volume paritiioning plugins. 

```bash
minikube stop
minikube start
```

5. Minikube Tunnel Issues: If the tunnel is not working, try restarting Minikube:

```bash
minikube stop
minikube start
minikube tunnel
```
