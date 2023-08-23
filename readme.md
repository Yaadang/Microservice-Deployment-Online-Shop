# Online Shop Microservices Demo

## Overview

This project demonstrates an intricate understanding of microservices deployment using Kubernetes. While Kubernetes' native `kubectl` command offers direct control, using Helm in conjunction with Helmfile streamlines the deployment process and makes it more maintainable.

## Repository Structure and Detailed Functionality

- **config-microservices.yaml**: A consolidated file that contains the bulk configurations for every microservice. This would traditionally be applied using `kubectl apply -f config-microservices.yaml`. While this approach is straightforward, it lacks flexibility, reusability, and modularity.

- **charts/microservices/templates/**: This directory contains Helm templates for the services and deployments:
  - **deployment.yaml**: A Helm template that generalizes the deployment structure for the microservices. It allows for dynamic assignment of values, offering flexibility for varying microservice requirements.
  - **service.yaml**: Similar to the deployment template, this Helm template abstracts the service definition, enabling the same structure to be reused for multiple microservices with varying configurations.

- **helmfile.yaml**: This file leverages the Helm templates from `charts/microservices/templates/`. Helmfile uses this to systematically deploy Helm charts, ensuring consistent deployments. By referencing dynamic values, Helmfile can deploy multiple microservices, each with its unique configurations, without duplicating any YAML code. 

## Helm & Helmfile vs. Kubectl Approach

**Helm & Helmfile**:
- **Modularity**: Helm templates allow the same YAML structure to be reused, reducing redundancy and potential errors.
- **Flexibility**: Dynamic values in Helm templates can be overridden for each microservice, ensuring customization without altering the core templates.
- **Consistency**: Helmfile ensures a consistent deployment process. If you've used the same Helmfile and Helm chart across environments, you're guaranteed the deployment process remains the same, only the values change.

**Kubectl**:
- **Direct Control**: You're interacting directly with the cluster, making it a transparent process.
- **Simplicity**: A single monolithic file can be both a boon and a bane. It's easy to visualize everything in one place but can become unwieldy as the project grows.
- **Less Overhead**: Without the need for templating engines or additional tools, it's simpler but comes at the cost of scalability and maintainability.

## Deployment Process Using Helm & Helmfile

1. **Review Configuration**: Before deployment, ensure the values you wish to use for each microservice are correctly defined.
  
2. **Helmfile Sync**: This command, `helmfile sync`, will deploy all Helm releases defined in the `helmfile.yaml`. It references the Helm templates and dynamically fills in the values for each microservice:
   ```bash
   helmfile sync
   ```

## Monitor Deployment:

```bash
kubectl get pods -w
```
Accessing the Application after successful deployment:

  Fetch the IP address assigned to the load balancer:

  ```bash
  kubectl get svc | grep frontend
  ```
Access the application using the IP address from your browser.
