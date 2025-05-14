## 📁 .github/workflows/deploy-to-aks.yml

```
name: Deploy to AKS

on:
  push:
    branches: [ main ]

env:
  AZURE_CONTAINER_REGISTRY: myacr.azurecr.io
  IMAGE_NAME: myapp
  RESOURCE_GROUP: myRG
  CLUSTER_NAME: myAKSCluster

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2

    - name: Login to ACR
      uses: docker/login-action@v2
      with:
        registry: ${{ env.AZURE_CONTAINER_REGISTRY }}
        username: ${{ secrets.ACR_USERNAME }}
        password: ${{ secrets.ACR_PASSWORD }}

    - name: Build and Push
      run: |
        docker build -t $AZURE_CONTAINER_REGISTRY/$IMAGE_NAME:latest .
        docker push $AZURE_CONTAINER_REGISTRY/$IMAGE_NAME:latest

    - name: Set AKS context
      uses: azure/aks-set-context@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        resource-group: ${{ env.RESOURCE_GROUP }}
        cluster-name: ${{ env.CLUSTER_NAME }}

    - name: Deploy to AKS
      run: |
        kubectl apply -f k8s/

```
Make sure to:

Store AZURE_CREDENTIALS in GitHub Secrets (use az ad sp create-for-rbac to generate).

Define ACR_USERNAME and ACR_PASSWORD if not using managed identity.

