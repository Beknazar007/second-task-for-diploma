# CI/CD Pipeline for Build and Deploy Service 1

This CI/CD pipeline automates the build and deployment process for Service 1 in CloudWorks Solutions' codebase. The pipeline is triggered on pull requests to the `development` and `main` branches, specifically when changes are made to files within the `service-1` directory. Additionally, it can be manually triggered through workflow dispatch.

## Workflow Structure

The pipeline consists of several jobs responsible for building the application, creating Docker images, and deploying them to Kubernetes clusters in different environments (development, QA, and production).

### Jobs

1. **build**: Builds Service 1, creates a Docker image, and pushes it to Docker Hub.
2. **deploy_dev**: Deploys Service 1 to the development environment.
3. **deploy_qa**: Deploys Service 1 to the QA environment.
4. **deploy_prod**: Deploys Service 1 to the production environment.

## Workflow Steps

### 1. Build Job

- **Checkout code**: Clones the repository's codebase using the `actions/checkout` action.
- **Set up JDK 17**: Configures the Java Development Kit (JDK) version 17 using the `actions/setup-java` action.
- **Build and Deploy with Maven**: Builds Service 1 using Maven, fetches the project version, and deploys the artifacts to a Maven repository.
- **Login to Docker Hub**: Authenticates with Docker Hub using Docker Hub credentials stored as GitHub Secrets.
- **Build and tag Docker image**: Builds a Docker image for Service 1, tags it with the GitHub SHA, and pushes it to Docker Hub.
- **Deploy to Kubernetes**: Deploys the Docker image to a Kubernetes cluster in the production environment.

### 2. Deployment Jobs (dev, qa, prod)

- **Configure AWS credentials**: Authenticates with AWS using AWS credentials stored as GitHub Secrets.
- **Update Kubernetes deployment**: Updates the Kubernetes deployment with the newly built Docker image, ensuring Service 1 is up-to-date.
- **Rollout status check**: Checks the status of the Kubernetes deployment to ensure the rollout is successful.

## Environment Specificity

- **Development Environment (dev)**: Used for testing and development purposes. The `deploy_dev` job is triggered when changes are made on the `development` branch and Service 1 is updated.
- **QA Environment (qa)**: Used for quality assurance testing.
- **Production Environment (prod)**: The live environment where Service 1 is publicly available.

## Trigger Conditions

- The pipeline is triggered on pull requests to the `development` and `main` branches, specifically when changes are made to files within the `service-1` directory.

## Security Considerations

- Ensure that sensitive information such as Docker Hub and AWS credentials are securely stored as GitHub Secrets.
- Review and restrict access to GitHub Secrets to authorized personnel.




# CI/CD Pipeline for Version Update or New release

This CI/CD pipeline automates the process of updating the version of specified services in CloudWorks Solutions' codebase and pushing the changes to a designated release branch in the GitHub repository. The pipeline is triggered manually through workflow dispatch and allows users to specify the release version and select which services to update.

## Workflow Structure

The pipeline consists of a single job that updates the versions of selected services and pushes the changes to a release branch.

### Job

1. **update-versions-and-push**: Updates the versions of specified services and pushes the changes to a designated release branch.

## Workflow Inputs

The workflow dispatch accepts the following inputs:

- **release_version**: The release version to update to. Default is set to '1.0.0-RELEASE'.
- **service-1**: Boolean input to specify whether to update Service 1. Default is set to 'true'.
- **service-2**: Boolean input to specify whether to update Service 2. Default is set to 'false'.
- **service-3**: Boolean input to specify whether to update Service 3. Default is set to 'false'.

## Workflow Steps

1. **Checkout code**: Clones the repository's codebase using the `actions/checkout` action.
2. **Update versions**: Updates the versions of selected services to the specified release version. The selected services are determined based on the provided inputs.
   - Creates a new release branch using the specified release version.
   - Iterates through the selected services, updates their versions, commits the changes, and pushes them to the release branch.
   - Uses Maven to update the versions of services' projects.
   - Commits the changes and pushes them to the release branch.

## Security Considerations

- Ensure that sensitive information such as GitHub Personal Access Tokens (PAT) used for pushing changes is securely stored as GitHub Secrets.
- Review and restrict access to GitHub Secrets to authorized personnel.
