## Deployment Steps

### Step 0: Project Directory Structure

Before we proceed with the deployment steps, here is the directory structure of the project:

```plaintext
Techdome/
├── backend/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── (other backend files)
├── frontend/
│   ├── Dockerfile
│   └── (frontend files)
└── kubernetes/
    ├── backend.yaml
    ├── frontend.yaml
    ├── mern-namespace.yaml
    ├── mongo.yaml
    └── readme.md 

```

### Step 1: Initial Local Deployment with Docker Compose for Database

The first step involved setting up the application locally by deploying the **database** using Docker Compose. This allowed us to quickly spin up the database container, test connectivity, and ensure that the database was functioning correctly before proceeding with other services.

#### Breakdown:
- The **database container** was configured and started using Docker Compose.

    ```bash
    docker-compose -f ./backend/docker-compose.yml up -d db
    ```

- Local connections were established to verify the database's connectivity.

- The database was tested thoroughly for any issues before moving forward.

---

### Step 2: Dockerizing Frontend and Backend, and Integrating into Docker Compose

Once the local deployment with the database was successful, the next step was to containerize the **frontend** and **backend** services. Each service was packaged into Docker images, ensuring they were properly configured to run independently.

#### Breakdown:
- A **Dockerfile** for both **frontend** and **backend** was created to specify the necessary build steps and dependencies.

    ```bash
    docker build -t backend-distroless:v2 ./backend
    docker build -t frontend-distroless:v2 ./frontend
    ```

- Docker images for both services were built using these Dockerfiles.

    ```bash
    docker images
    ```

- A single **docker-compose.yml** file was created to manage the deployment of all services (frontend, backend, and database) in a unified environment.

    ```bash
    docker-compose -f ./backend/docker-compose.yml up -d
    ```

- The services were tested to ensure they were correctly communicating and interacting within the Docker network.

---

### Step 3: Kubernetes Deployment

After successfully containerizing the frontend, backend, and database, the final step involved deploying the application on a **Kubernetes cluster**. This allowed the application to scale, ensuring high availability and simplified management of the services.

#### Breakdown:
- The Kubernetes configuration was set up for all services, ensuring they were properly defined as **deployments** and **services**.

    ```bash
    kubectl apply -f ./kubernetes/mern-namespace.yaml
    ```

- The frontend, backend, and database services were deployed to the Kubernetes cluster.

    ```bash
    kubectl apply -f ./kubernetes/frontend.yaml
    kubectl apply -f ./kubernetes/backend.yaml
    kubectl apply -f ./kubernetes/mongo.yaml
    ```

- **Port forwarding** was configured to enable local access to the services running in Kubernetes.

    ```bash
    kubectl port-forward svc/frontend 3000:3000 -n mern-stack
    kubectl port-forward svc/backend 5000:5000 -n mern-stack
    ```

- The deployment was verified by accessing the frontend, backend, and database through the forwarded ports.

    Access the services locally:
    - Frontend: [http://localhost:8080](http://localhost:8080)
    - Backend: [http://localhost:5000](http://localhost:5000)
    - Database: `localhost:3306` (for local DB clients)

---
