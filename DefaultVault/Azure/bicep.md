Great! Here's the modular Bicep deployment setup as requested. This will include:

---

### ✅ `main.bicep` – Entry Point

```bicep
param location string = resourceGroup().location
param adminUsername string = 'azureuser'

module networkModule 'network.bicep' = {
  name: 'networkDeployment'
  params: {
    location: location
  }
}

module storageModule 'storage.bicep' = {
  name: 'storageDeployment'
  params: {
    location: location
  }
}

module keyVaultModule 'keyvault.bicep' = {
  name: 'keyVaultDeployment'
  params: {
    location: location
  }
}

module vmModule 'vm.bicep' = {
  name: 'vmDeployment'
  params: {
    location: location
    adminUsername: adminUsername
    nicId: networkModule.outputs.nicId
    storageAccountName: storageModule.outputs.storageAccountName
    keyVaultSecretUri: keyVaultModule.outputs.secretUri
  }
}
```

---

### ✅ `network.bicep` – Network, Subnets, NSG, Public IP, NIC

```bicep
param location string

resource vnet 'Microsoft.Network/virtualNetworks@2023-05-01' = {
  name: 'myVnet'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'subnet1'
        properties: {
          addressPrefix: '10.0.1.0/24'
        }
      }
      {
        name: 'subnet2'
        properties: {
          addressPrefix: '10.0.2.0/24'
        }
      }
    ]
  }
}

resource nsg 'Microsoft.Network/networkSecurityGroups@2023-05-01' = {
  name: 'myNSG'
  location: location
  properties: {
    securityRules: [
      {
        name: 'Allow-RDP'
        properties: {
          priority: 1000
          direction: 'Inbound'
          access: 'Allow'
          protocol: 'Tcp'
          sourcePortRange: '*'
          destinationPortRange: '3389'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
        }
      }
    ]
  }
}

resource publicIp 'Microsoft.Network/publicIPAddresses@2023-05-01' = {
  name: 'myPublicIP'
  location: location
  properties: {
    publicIPAllocationMethod: 'Dynamic'
  }
}

resource nic 'Microsoft.Network/networkInterfaces@2023-05-01' = {
  name: 'myNIC'
  location: location
  properties: {
    ipConfigurations: [
      {
        name: 'ipconfig1'
        properties: {
          subnet: {
            id: vnet.properties.subnets[0].id
          }
          privateIPAllocationMethod: 'Dynamic'
          publicIPAddress: {
            id: publicIp.id
          }
        }
      }
    ]
    networkSecurityGroup: {
      id: nsg.id
    }
  }
}

output nicId string = nic.id
```

---

### ✅ `storage.bicep` – Storage Account

```bicep
param location string

resource storage 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: uniqueString(resourceGroup().id, 'storageacct')
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    allowBlobPublicAccess: false
    deleteRetentionPolicy: {
      enabled: false
    }
    allowPermanentDelete: true
  }
}

output storageAccountName string = storage.name
```

---

### ✅ `keyvault.bicep` – Key Vault + Secure Password Secret

```bicep
param location string

var secretName = 'vmAdminPassword'
var password = uniqueString(resourceGroup().id, utcNow()) // can be replaced with more complex generation

resource keyVault 'Microsoft.KeyVault/vaults@2023-02-01' = {
  name: 'myKeyVault'
  location: location
  properties: {
    sku: {
      name: 'standard'
      family: 'A'
    }
    tenantId: subscription().tenantId
    accessPolicies: [] // Can be customized for deployment identity
    enableSoftDelete: true
    enablePurgeProtection: true
  }
}

resource secret 'Microsoft.KeyVault/vaults/secrets@2023-02-01' = {
  name: '${keyVault.name}/${secretName}'
  properties: {
    value: password
  }
  dependsOn: [
    keyVault
  ]
}

output secretUri string = 'https://${keyVault.name}.vault.azure.net/secrets/${secretName}/${secret.properties.version}'
```

---

### ✅ `vm.bicep` – VM Deployment Using Modules

```bicep
param location string
param adminUsername string
param nicId string
param keyVaultSecretUri string
param storageAccountName string

var adminPassword = reference(keyVaultSecretUri, '2015-06-01').secretValue

resource vm 'Microsoft.Compute/virtualMachines@2023-03-01' = {
  name: 'myVM'
  location: location
  properties: {
    hardwareProfile: {
      vmSize: 'Standard_DS1_v2'
    }
    osProfile: {
      computerName: 'myVM'
      adminUsername: adminUsername
      adminPassword: adminPassword
    }
    storageProfile: {
      imageReference: {
        publisher: 'MicrosoftWindowsServer'
        offer: 'WindowsServer'
        sku: '2019-Datacenter'
        version: 'latest'
      }
      osDisk: {
        createOption: 'FromImage'
      }
    }
    diagnosticsProfile: {
      bootDiagnostics: {
        enabled: true
        storageUri: 'https://${storageAccountName}.blob.core.windows.net/'
      }
    }
    networkProfile: {
      networkInterfaces: [
        {
          id: nicId
        }
      ]
    }
  }
}
```

---

Let me know if you want to:

- Add outputs for other resources
    
- Modify NSG rules
    
- Add private DNS, VM extensions, or tagging
    
- Convert this into a deployment-ready `.bicepparam` parameter file
    

I can help package this too!