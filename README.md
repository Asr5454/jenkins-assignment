# Jenkins Assignment 2

## Description

This project demonstrates a Jenkins pipeline that:
- Pulls code from GitHub
- Builds and pushes a Docker image to Docker Hub
- Runs and verifies the container
- Cleans up resources
- Uses a Shared Library for Docker logic

## Structure
- `Jenkinsfile`: Main pipeline logic
- Shared Library: `dockerUtils.groovy` (in separate repo)

## Prerequisites
- Jenkins installed
- Docker Desktop running
- Docker credentials added to Jenkins (ID: `dockerhub`)
- Shared Library configured in Jenkins (Name: `jenkins-shared-lib`)

