 ## main.bicep — Deploy AKS with Azure CNI, ACR integration, and monitoring

--------------------------------------------------------------------------------------
```
param location string = resourceGroup().location
param aksName string = 'myAKSCluster'
param dnsPrefix string = 'myaks'
param agentCount int = 3
param kubernetesVersion string = '1.29.2'
param acrName string

resource acr 'Microsoft.ContainerRegistry/registries@2023-01-01-preview' existing = {
  name: acrName
}

resource aks 'Microsoft.ContainerService/managedClusters@2023-01-01' = {
  name: aksName
  location: location
  identity: {
    type: 'SystemAssigned'
  }
  properties: {
    dnsPrefix: dnsPrefix
    kubernetesVersion: kubernetesVersion
    enableRBAC: true
    agentPoolProfiles: [
      {
        name: 'nodepool'
        count: agentCount
        vmSize: 'Standard_DS2_v2'
        type: 'VirtualMachineScaleSets'
        mode: 'System'
        osType: 'Linux'
      }
    ]
    linuxProfile: {
      adminUsername: 'azureuser'
      ssh: {
        publicKeys: [
          {
            keyData: 'ssh-rsa YOUR_PUBLIC_KEY'
          }
        ]
      }
    }
    addonProfiles: {
      kubeDashboard: {
        enabled: false
      }
      omsagent: {
        enabled: true
      }
    }
    networkProfile: {
      networkPlugin: 'azure'
      loadBalancerSku: 'standard'
    }
  }
}

resource roleAssignment 'Microsoft.Authorization/roleAssignments@2020-10-01-preview' = {
  name: guid(aks.identity.principalId, acr.id, 'acrpull')
  scope: acr
  properties: {
    principalId: aks.identity.principalId
    roleDefinitionId: subscriptionResourceId('Microsoft.Authorization/roleDefinitions', '7f951dda-4ed3-4680-a7ca-43fe172d538d') // AcrPull
    principalType: 'ServicePrincipal'
  }
}
```
##  Deploy the Bicep template:
az deployment group create \
  --resource-group myRG \
  --template-file main.bicep \
  --parameters acrName=myacr
