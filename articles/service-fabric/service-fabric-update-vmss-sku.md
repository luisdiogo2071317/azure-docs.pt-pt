---
title: Procedimento para atualização/migrar o SKU de PrimaryNodeType para SKUs superiores | Microsoft Docs
description: Saiba como atualização/migrar o SKU de PrimaryNodeType para SKUs superiores utilizar comandos do PowerShell e comandos da CLI.
services: service-fabric
documentationcenter: .net
author: v-rachiw
manager: navya
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2018
ms.author: v-rachiw
ms.openlocfilehash: 96956543a44b6d5d967e3bae3fd833b08baf3d6f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642738"
---
# <a name="upgrademigrate-the-sku-for-primary-node-type-to-higher-sku"></a>Atualização/migrar o SKU para o tipo de nó principal para o SKU superior

Este artigo descreve como migrar/atualização o SKU do primário tipo de nó de cluster do service fabric para SKU superior com o Azure PowerShell

## <a name="add-a-new-virtual-machine-scale-set"></a>Adicionar um novo conjunto de dimensionamento de máquina virtual

Implemente um novo conjunto de dimensionamento de máquina virtual e o Balanceador de carga. A configuração de extensão de recursos de infraestrutura de serviço (especialmente o tipo de nó) do novo conjunto de dimensionamento de máquina virtual deve ser igual ao conjunto de escala antiga que está a tentar atualizar. Certifique-se no Explorador de recursos de infraestrutura de serviço que estão disponíveis os novos nós

#### <a name="azure-powershell"></a>Azure PowerShell

O exemplo seguinte utiliza o Azure PowerShell para implementar o modelo do Resource Manager atualizado *Template* utilizando o grupo de recursos com o nome *myResourceGroup*:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile template.json -TemplateParameterFile parameters.json
```

#### <a name="azure-cli"></a>CLI do Azure

O seguinte comando utiliza o CLI de recursos de infraestrutura de serviço do Azure para implementar o modelo do Resource Manager atualizado *Template* utilizando o grupo de recursos com o nome *myResourceGroup*:

```CLI
az group deployment create --resource-group myResourceGroup --template-file template.json --parameters parameters.json
```

Consulte o seguinte exemplo para modificar o modelo json para adicionar o novo recurso de conjunto de dimensionamento de máquina virtual com o tipo de nó principal no cluster existente

```json
        {
            "apiVersion": "[variables('vmssApiVersion')]",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[parameters('vmNodeType2Name')]",
            "location": "[parameters('computeLocation')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType2Name')))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
            ],
            "properties": {
                "overprovision": "[parameters('overProvision')]",
                "upgradePolicy": {
                    "mode": "Automatic"
                },
                "virtualMachineProfile": {
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "[concat(parameters('vmNodeType2Name'),'_ServiceFabricNode')]",
                                "properties": {
                                    "type": "ServiceFabricNode",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                                    },
                                    "publisher": "Microsoft.Azure.ServiceFabric",
                                    "settings": {
                                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                                        "dataPath": "D:\\\\SvcFab",
                                        "durabilityLevel": "Bronze",
                                        "enableParallelJobs": true,
                                        "nicPrefixOverride": "[parameters('subnet0Prefix')]",
                                        "certificate": {
                                            "thumbprint": "[parameters('certificateThumbprint')]",
                                            "x509StoreName": "[parameters('certificateStoreValue')]"
                                        }
                                    },
                                    "typeHandlerVersion": "1.0"
                                }
                            },
                            {
                                "name": "[concat('VMDiagnosticsVmExt','_vmNodeType2Name')]",
                                "properties": {
                                    "type": "IaaSDiagnostics",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                        "storageAccountEndPoint": "https://core.windows.net/"
                                    },
                                    "publisher": "Microsoft.Azure.Diagnostics",
                                    "settings": {
                                        "WadCfg": {
                                            "DiagnosticMonitorConfiguration": {
                                                "overallQuotaInMB": "50000",
                                                "EtwProviders": {
                                                    "EtwEventSourceProviderConfiguration": [
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Actors",
                                                            "scheduledTransferKeywordFilter": "1",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableActorEventTable"
                                                            }
                                                        },
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Services",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                            }
                                                        }
                                                    ],
                                                    "EtwManifestProviderConfiguration": [
                                                        {
                                                            "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                            "scheduledTransferLogLevelFilter": "Information",
                                                            "scheduledTransferKeywordFilter": "4611686018427387904",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricSystemEventTable"
                                                            }
                                                        }
                                                    ]
                                                }
                                            }
                                        },
                                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                                    },
                                    "typeHandlerVersion": "1.5"
                                }
                            }
                        ]
                    },
                    "networkProfile": {
                        "networkInterfaceConfigurations": [
                            {
                                "name": "[concat(parameters('nicName'), '-2')]",
                                "properties": {
                                    "ipConfigurations": [
                                        {
                                            "name": "[concat(parameters('nicName'),'-',2)]",
                                            "properties": {
                                                "loadBalancerBackendAddressPools": [
                                                    {
                                                        "id": "[variables('lbPoolID2')]"
                                                    }
                                                ],
                                                "loadBalancerInboundNatPools": [
                                                    {
                                                        "id": "[variables('lbNatPoolID2')]"
                                                    }
                                                ],
                                                "subnet": {
                                                    "id": "[variables('subnet0Ref')]"
                                                }
                                            }
                                        }
                                    ],
                                    "primary": true
                                }
                            }
                        ]
                    },
                    "osProfile": {
                        "adminPassword": "[parameters('adminPassword')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "computernamePrefix": "[parameters('vmNodeType2Name')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[parameters('sourceVaultValue')]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateStore": "[parameters('certificateStoreValue')]",
                                        "certificateUrl": "[parameters('certificateUrlValue')]"
                                    }
                                ]
                            }
                        ]
                    },
                    "storageProfile": {
                        "imageReference": {
                            "publisher": "[parameters('vmImagePublisher')]",
                            "offer": "[parameters('vmImageOffer')]",
                            "sku": "[parameters('vmImageSku')]",
                            "version": "[parameters('vmImageVersion')]"
                        },
                        "osDisk": {
                            "caching": "ReadOnly",
                            "createOption": "FromImage",
                            "managedDisk": {
                                "storageAccountType": "[parameters('storageAccountType')]"
                            }
                        }
                    }
                }
            },
            "sku": {
                "name": "[parameters('vmNodeType2Size')]",
                "capacity": "[parameters('nt2InstanceCount')]",
                "tier": "Standard"
            },
            "tags": {
                "resourceType": "Service Fabric",
                "clusterName": "[parameters('clusterName')]"
            }
        },
```

## <a name="remove-old-virtual-machine-scale-set"></a>Remover conjunto de dimensionamento de máquina virtual antigo

1. Desative instâncias VM de conjunto com a intenção de remover o nó de dimensionamento de máquina virtual antigo. Esta operação pode demorar muito tempo a concluir. Pode desativar ao mesmo tempo e irá ser em fila de espera. Aguarde até que todos os nós estão desativados. 
#### <a name="azure-powershell"></a>Azure PowerShell
O exemplo seguinte utiliza o Azure PowerShell de Service Fabric para desativar a instância de nó com o nome *NTvm1_0* do conjunto nomeado de dimensionamento de máquina virtual antigo *NTvm1*:
```powershell
Disable-ServiceFabricNode -NodeName NTvm1_0 -Intent RemoveNode
```
#### <a name="azure-cli"></a>CLI do Azure
O comando seguinte utiliza a CLI do Azure Service Fabric para desativar a instância de nó com o nome *NTvm1_0* do conjunto nomeado de dimensionamento de máquina virtual antigo *NTvm1*:
```CLI
sfctl node disable --node-name "_NTvm1_0" --deactivation-intent RemoveNode
```
2. Remova o conjunto de dimensionamento concluída. Aguarde até que o estado de aprovisionamento do conjunto de dimensionamento é concluída com êxito
#### <a name="azure-powershell"></a>Azure PowerShell
O exemplo seguinte utiliza o Azure PowerShell para remover o conjunto nomeado de dimensionamento concluído *NTvm1* do grupo de recursos com o nome *myResourceGroup*:
```powershell
Remove-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName NTvm1
```
#### <a name="azure-cli"></a>CLI do Azure
O comando seguinte utiliza a CLI do Azure Service Fabric para remover o conjunto nomeado de dimensionamento concluído *NTvm1* do grupo de recursos com o nome *myResourceGroup*:

```CLI
az vmss delete --name NTvm1 --resource-group myResourceGroup
```

## <a name="remove-load-balancer-related-to-old-scale-set"></a>Remover o Balanceador de carga relacionados com o conjunto de dimensionamento antigo

Remova o Balanceador de carga relacionados com o conjunto de dimensionamento antigo. Este passo irá fazer com que um breve período de tempo de inatividade para o cluster

#### <a name="azure-powershell"></a>Azure PowerShell

O exemplo seguinte utiliza o Azure PowerShell para remover o Balanceador de carga com o nome *LB-myCluster-NTvm1* relacionados com o conjunto do grupo de recursos com o nome de dimensionamento antigo *myResourceGroup*:

```powershell
Remove-AzureRmLoadBalancer -Name LB-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>CLI do Azure

O comando seguinte utiliza a CLI do Azure Service Fabric para remover o Balanceador de carga com o nome *LB-myCluster-NTvm1* relacionados com o conjunto do grupo de recursos com o nome de dimensionamento antigo *myResourceGroup*:

```CLI
az network lb delete --name LB-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="remove-public-ip-related-to-old-scale-set"></a>Remova o IP público relacionados com o conjunto de dimensionamento antigo

As definições de DNS do arquivo de endereço IP público relacionadas com a escala antiga definido numa variável, em seguida, remova o endereço IP público

#### <a name="azure-powershell"></a>Azure PowerShell

O exemplo seguinte utiliza o Azure PowerShell para armazenar definições de endereço IP público com o nome DNS *LBIP-myCluster-NTvm1* numa variável e remover o endereço IP:

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn
Remove-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>CLI do Azure

O seguinte comando utiliza o CLI do Azure Service Fabric para obter definições de endereço IP público com o nome DNS *LBIP-myCluster-NTvm1* e remova o endereço IP:

```CLI
az network public-ip show --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
az network public-ip delete --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="update-public-ip-address-related-to-new-scale-set"></a>Endereço IP público atualização relacionados com o novo conjunto de dimensionamento

Definições de atualização de DNS do endereço IP público relacionadas com a escala novo conjunto com as definições de DNS do endereço IP público relacionados com o conjunto de dimensionamento antigo

#### <a name="azure-powershell"></a>Azure PowerShell
O exemplo seguinte utiliza o Azure PowerShell para atualizar as definições de DNS do endereço IP público com o nome *LBIP-myCluster-NTvm3* com as definições de DNS armazenadas as variáveis no passo anterior:

```powershell
$PublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm3  -ResourceGroupName myResourceGroup
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP
```

#### <a name="azure-cli"></a>CLI do Azure

O comando seguinte utiliza a CLI do Azure Service Fabric para atualizar as definições de DNS do endereço IP público com o nome *LBIP-myCluster-NTvm3* com as definições de DNS antigo do IP público recolhido no passo anterior:

```CLI
az network public-ip update --name LBIP-myCluster-NTvm3 --resource-group myResourceGroup --dns-name myCluster
```

## <a name="remove-knowledge-of-service-fabric-node-from-fm"></a>Remover dados de conhecimento do nó de recursos de infraestrutura de serviço FM

Notificar Service Fabric que os nós, que estão em baixo foram removidos do cluster. (Executar este comando para todas as VMS do antigo instâncias de conjunto de dimensionamento da máquina virtual) (Se o nível de durabilidade do conjunto de dimensionamento de máquina virtual antigo foi silver ou ouro, este passo pode não ser necessária. Uma vez que esse passo é efetuado pelo sistema automaticamente.)

#### <a name="azure-powershell"></a>Azure PowerShell
O exemplo seguinte utiliza o Azure PowerShell de Service Fabric para notificar os recursos de infraestrutura de serviço que o nó com o nome *NTvm1_0* foi removido:

```powershell
Remove-ServiceFabricNodeState -NodeName NTvm1_0
```

#### <a name="azure-cli"></a>CLI do Azure

O comando seguinte utiliza a CLI do Azure Service Fabric para notificar os recursos de infraestrutura de serviço que o nó com o nome *NTvm1_0* foi removido:

```CLI
sfctl node remove-state --node-name _NTvm1_0
```
