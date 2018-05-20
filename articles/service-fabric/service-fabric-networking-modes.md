---
title: Configurar os modos de funcionamento em rede para serviços de contentor do Azure Service Fabric | Microsoft Docs
description: Saiba como configurar os diferentes modos de rede que são suportados pelo Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f831c046bcf8f633841f9dc4a0fce6d1e419e6c2
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-container-networking-modes"></a>Modos de funcionamento em rede de contentor do Service Fabric

Um cluster do Service Fabric do Azure para o contentor de serviços utiliza **nat** modo de funcionamento em rede por predefinição. Quando mais do que um serviço de contentor está à escuta na mesma porta e o modo de nat está a ser utilizado, podem ocorrer erros de implementação. Para suportar vários serviços de contentor escuta na mesma porta, o Service Fabric oferece **abra** modo de funcionamento em rede (versões 5.7 e posteriores). Modo aberto, cada serviço de contentor tem uma interno atribuídas dinamicamente o endereço IP que suporta vários serviços de escuta na mesma porta.  

Se tiver um serviço de contentor com um ponto de final estático no seu manifesto do serviço, pode criar e eliminar novos serviços utilizando o modo aberto sem erros de implementação. Também pode ser utilizado o mesmo ficheiro docker-Compose.yml com mapeamentos de porta estática para criar vários serviços.

Quando um serviço de contentor é reiniciado ou movido para outro nó no cluster, o endereço IP é alterado. Por este motivo, recomendamos que não utilize o endereço IP dinamicamente atribuído para detetar os serviços de contentor. Apenas o serviço Service Fabric Naming ou o serviço DNS deve ser utilizado para a deteção do serviço. 

>[!WARNING]
>Azure permite um total de IPs 4.096 por rede virtual. A soma do número de nós e o número de instâncias de serviço de contentor (o que estiver a utilizar o modo aberto) não pode exceder os IPs 4.096 dentro de uma rede virtual. Para cenários high-density, recomendamos o modo de funcionamento em rede de nat.
>

## <a name="set-up-open-networking-mode"></a>Configurar o modo de funcionamento em rede aberto

1. Configure o modelo Azure Resource Manager. No **fabricSettings** secção, ative o serviço DNS e o fornecedor de IP: 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. Configure a secção de perfil de rede para permitir que vários endereços IP configurados em cada nó do cluster. O exemplo a seguir configura a cinco endereços IP por nó para um cluster do Windows/Linux Service Fabric. Pode ter cinco instâncias do serviço de escuta na porta em cada nó.

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
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
              }
   ```
 
3. Para apenas clusters do Windows, configure uma regra de grupo de segurança de rede (NSG) do Azure que abre-se a porta UDP/53 para a rede virtual com os seguintes valores:

   |Definição |Valor | |
   | --- | --- | --- |
   |Prioridade |2000 | |
   |Nome |Custom_Dns  | |
   |Origem |VirtualNetwork | |
   |Destino | VirtualNetwork | |
   |Serviço | DNS (UDP/53) | |
   |Ação | Permitir  | |
   | | |

4. Especificar o modo de funcionamento em rede no manifesto da aplicação para cada serviço: `<NetworkConfig NetworkType="Open">`. **Abra** resultados de modo no serviço de obter um endereço IP dedicado de rede. Se não está especificado um modo, o serviço será assumida a **nat** modo. No exemplo seguinte manifesto, o `NodeContainerServicePackage1` e `NodeContainerServicePackage2` serviços podem cada escuta na mesma porta (ambos os serviços estão à escuta em `Endpoint1`). Quando é especificado o modo de funcionamento em rede aberto, `PortBinding` não não possível especificar configurações.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```

    Pode combinar e misturar os modos de rede diferentes nos vários serviços dentro de uma aplicação para um cluster do Windows. Alguns serviços podem utilizar o modo aberto enquanto outros utilizam o modo de nat. Quando um serviço está configurado para utilizar o modo de nat, a porta que o serviço está a escutar tem de ser exclusiva.

    >[!NOTE]
    >Nos clusters do Linux, mistura modos de funcionamento em rede para serviços diferentes não é suportada. 
    >

5. Quando o **abra** modo estiver seleccionado, o **Endpoint** definição no manifesto de serviço explicitamente deve apontar para o pacote do código correspondente para o ponto final, mesmo que o pacote de serviço tem apenas um código pacote no mesmo. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```

## <a name="next-steps"></a>Passos Seguintes
* [Understand the Service Fabric application model (Compreender o modelo de aplicações do Service Fabric)](service-fabric-application-model.md)
* [Saiba mais sobre os recursos de manifesto de serviço de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Implementar um contentor do Windows para o Service Fabric no Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementar um contentor de Docker Service Fabric no Linux](service-fabric-get-started-containers-linux.md)
