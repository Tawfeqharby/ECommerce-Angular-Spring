### AevaPay Project

## First phase 

1- First to create your docker files will hte following:

    # Builing the base image 
    FROM  node:16.15.2-alpine  AS  appbuild
    WORKDIR  /app
    COPY  package.json  ./app
    RUN  npm  install
    COPY  .  ./app
    RUN  npm  run  test
    RUN  npm  run  build
    RUN  npm  run  lint
    # Build Stage 2
    # Building the final image based of the base image
    FROM  node:16.15.2-alpine
    WORKDIR  /app
    COPY  package.json  ./
    COPY  --from=appbuild  /app/dist  ./dist
    EXPOSE  4002
    CMD  npm  start


2- Create the environment variable file for each environment (DEV-UAT-PROD). so you will have dev.env , uat.env and prod.env .
each file contain the variables in each environment.
3- Create the  docker composer file  with following:

```
version: '3.4'
services:
backend:
   image: backend
   build:
   context: .
       dockerfile: ./Dockerfile
   ports:
       - 3000:3000
   volumes:
       - ./:/app
       - /app/node_modules
 ```
4- Run the docker composer command based on the env file that you did before with the following command:
```
docker-compose up --env-file dev.env -d 
```

5- Based on the following Github action Pipeline :
```
name: Build Docker Images
on:
   pull_request:
      branches:
         - dev
         - uat
         - main
jobs:
    build_docker_images:
    runs-on: ubuntu-latest

   steps:

    -
      name: Checkout
      uses: actions/checkout@v2
 
    -
      name: Docker meta
      id: docker_meta
      uses: crazy-max/ghaction-docker-meta@v1
      with:
        images: ghcr.io/awesomecompany/mlflow
        tag-sha: true

    -
      name: Set up QEMU
      uses: docker/setup-qemu-action@v1

    -
      name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v1

    -
      name: Login to GitHub Container Registry
      uses: docker/login-action@v1
      with:
        registry: ghcr.io
        username: ${{ github.repository_owner }}
        password: ${{ secrets.CR_PAT }}

    -

      name: Build and push MLFlow image
      id: mlflow
      uses: docker/build-push-action@v2
      with:
        context: ./
        file: ./mlflow/Dockerfile
        push: true
        tags: ${{ steps.docker_meta.outputs.tags }}
        
   ```

So the Pipeline is :
1- make checkout step and based on the Triger that run when push happens  on the branch.
2- Build the docker image, and inside the docker image we make all the phases to build and test and lint.
3- we provide which Registry for example Docker Hub with credential.
4- Push the image in your private registry.
5- You can use it after using docker compose or in Kubernetes using Deployment file. or We can package it to Helm charts and install the chart in our Kubernetes cluster.


## Second phase ##

Design our infrastructure that host the application:
1- I will choose EKS service in Amazon.
    - you will find the design in the the same git repo.
    - It is EKS design.
 Here is the list of used service:
 1- EKS service: Managed kubernetes Cluster.
 2- RDS: for MYSQL database.
 3- All basic networking in AWS.
 4- Elastic cashe : for Redis memory database.
 4- Prometheus and Grafana for monitor our application.
 5- Inside the cluster we will use 
     - Nginx Ingress Controller.
     - Deployments.
     - Service ClusterIP.
     - PODs.
6- We can use ArgoCD for continuous deployment.
