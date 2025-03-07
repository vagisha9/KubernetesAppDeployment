
Overview

This document provides an overview of the deployment process for analytics application, detailing the technologies and tools used, as well as guidance on how to deploy changes effectively.

Technologies and Tools

1. Version Control System

Github

We use Git for version control to manage changes to the source code. The repository is hosted on GitHub.

2. Continuous Integration (CI)

AWS CodeBuild: We utilize AWS CodeBuild to automate the build process. CodeBuild compiles the source code, creates a docker image of the analytics application. We have ECR repository hosted on AWS. Image is stored in that reporsitory.The build is trigerred everytime when pull reuqest is merged.

Build Specification: The build process is defined in a buildspec.yml file, which specifies the build commands associated with building dockerfile and environment settings.

3. Containerization

Docker: Our application is containerized using Docker. This ensures consistency across different environments (development, staging, production).

Dockerfile: The Dockerfile defines the environment and instructions for building the Docker image, including dependencies and configuration.

4. Database Configuration

Database needs to be configured locally for this project. Apply yaml configurations in following order:
kubectl apply -f pvc.yaml
kubectl apply -f pv.yaml
kubectl apply -f postgresql-deployment.yaml

Deploy service to expose the database using:
kubectl apply -f postgresql-service.yaml

Run the seed files to insert data into user and tokens tables present in DB folder using:

export DB_PASSWORD=mypassword
PGPASSWORD="$DB_PASSWORD" psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < <FILE_NAME.sql>

Application Deployment

Add database configuration for host,name,username and password in configmap file
Deploy configmap and coworking yaml configurations to deploy the app

5. Verification of pods and services

Validate pods for database and application should be created and running

Validate application and database service using command:
kubectl get svc

Monitoring tool - AWS CloudWatch can be used to track application performance and health



Releasing New Builds

Steps for Releasing Changes:

- Create a Pull Request: Once development is complete, create a pull request to merge changes into the main branch.
- Code Review: Conduct a code review with team members to ensure code quality and adherence to standards.
- Merge Changes: After approval, merge the changes into the main branch.
- Trigger CI/CD Pipeline: The merge will trigger the CI/CD pipeline, initiating the build and deployment process.
Monitor Deployment: After deployment, monitor the application for any issues and verify that the new features are functioning correctly.
