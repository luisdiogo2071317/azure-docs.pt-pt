---
title: Analisar a segurança de rede com a vista de grupo de segurança de observador de rede do Azure - PowerShell | Documentos da Microsoft
description: Este artigo descreve como utilizar o PowerShell para analisar uma segurança de máquinas virtuais com a vista de grupo de segurança.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 04e76b49-6a1b-4d0f-9a9b-51cf2f4df5a2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9cc06e97730ac846e8aa42c2cee77dfe17be99bb
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089651"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a>Analisar a segurança de Máquina Virtual com a vista de grupo de segurança com o PowerShell

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI do Azure](network-watcher-security-group-view-cli.md)
> - [API REST](network-watcher-security-group-view-rest.md)

Vista de grupo de segurança devolve as regras de segurança de rede configurado e eficaz que são aplicadas a uma máquina virtual. Esta funcionalidade é útil para auditar e diagnosticar os grupos de segurança de rede e regras que estão configuradas numa VM para garantir que o tráfego está a ser corretamente permitido ou negado. Neste artigo, mostramos como obter as regras de segurança configurado e eficaz para uma máquina virtual com o PowerShell

## <a name="before-you-begin"></a>Antes de começar

Neste cenário, irá executar o `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet para obter as informações de regra de segurança.

Este cenário pressupõe que já tiver seguido os passos em [criar um observador de rede](network-watcher-create.md) para criar um observador de rede.

## <a name="scenario"></a>Cenário

O cenário abordado neste artigo obtém as regras de segurança configurado e eficaz para uma máquina virtual especificada.

## <a name="retrieve-network-watcher"></a>Obter o observador de rede

A primeira etapa é obter a instância do observador de rede. Esta variável é passada para o `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a>Obtenha uma VM

Uma máquina virtual é necessária para executar o `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet contra. O exemplo seguinte obtém um objeto VM.

```powershell
$VM = Get-AzurermVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a>Obter vista de grupo de segurança

A próxima etapa é obter o resultado de vista do grupo de segurança.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a>Visualizar os resultados

O exemplo seguinte é uma resposta abreviada dos resultados retornados. Os resultados mostram todas as regras de segurança eficazes e aplicado na máquina virtual dividida em grupos de **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, e  **EffectiveSecurityRules**.

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a>Passos Seguintes

Visite [auditoria rede grupos de segurança (NSG) com o observador de rede](network-watcher-nsg-auditing-powershell.md) para aprender a automatizar a validação dos grupos de segurança de rede.


