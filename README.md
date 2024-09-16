# Static Site Deployment using Kubernetes (Testing)

This repository contains Kubernetes manifests for deploying a static website using Caddy web server.

## Prerequisites

- Kubernetes cluster
- `kubectl` command-line tool
- Docker

## Deployment

1. Clone the repository:

  ```
  git clone https://github.com/cvelens/k8-manifests.git
  cd k8-manifests
  ```

2. Clone the repository:
```
kubectl apply -f namespace.yaml
```

3. Create the secret for pulling the Docker image:
```
kubectl apply -f secret.yaml
```

4. Deploy the static site pod:
```
kubectl apply -f pod.yaml
```

5. Create the service to expose the static site:
```
kubectl apply -f service.yaml
```

6. Access the static site

### Note down the `NodePort` value and access the site using `http://<node-ip>:<node-port>` in your web browser.

## Manifests
* `namespace.yaml`: Creates a namespace named `static-site` for deploying the static site resources.
* `secret.yaml`: Creates a secret named `docker` to store the Docker registry credentials for pulling the container image.
* `pod.yaml`: Defines a pod named `caddy` running the Caddy web server container with the static site. It includes startup, liveness, and readiness probes.
* `service.yaml`: Creates a service named `static-site-service` to expose the static site pod using a `NodePort`.

## Probes
* **Startup Probe**: Checks if the static site is available at `/index.html` on port 8080. It has a failure threshold of 3 and runs every 5 seconds.
* **Liveness Probe**: Checks if the static site is up and running at `/index.html` on port 8080. It has an initial delay of 3 seconds and runs every 3 seconds.
* **Readiness Probe**: Checks if the static site is ready to serve traffic at `/index.html` on port 8080. It has an initial delay of 3 seconds and runs every 3 seconds.

> Note: The `secret.yaml` file contains base64 encoded Docker registry credentials. Make sure to update the credentials with your own before deploying.

## GitHub Actions Workflow

This repository includes a GitHub Actions workflow for linting YAML files. The workflow is defined in `.github/workflows/lint.yml` and performs the following steps:

1. Triggers the workflow on pull requests to the `main` branch or manually using `workflow_dispatch`.
2. Checks out the repository using `actions/checkout@v2`.
3. Sets up Python using `actions/setup-python@v2`.
4. Installs the `yamllint` package using `pip`.
5. Lints the YAML files in the repository using `yamllint`.

The workflow ensures that the YAML files in the repository adhere to the specified linting rules and helps maintain code quality.