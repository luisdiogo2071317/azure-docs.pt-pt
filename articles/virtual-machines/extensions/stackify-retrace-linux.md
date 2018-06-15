---
title: Stackify Retrace agente Linux do Azure extensão | Microsoft Docs
description: Implemente o agente de Stackify Retrace Linux numa máquina virtual Linux.
services: virtual-machines-linux
documentationcenter: ''
author: darinhoward
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/12/2018
ms.author: danis
ms.openlocfilehash: 376c5a087f74fbe087db9fa2df38b2ba4e6cf1ff
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33943083"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Stackify Retrace agente Linux extensão

## <a name="overview"></a>Descrição geral
Stackify fornece os produtos que controlam os detalhes sobre a sua aplicação para ajudar a localizar e corrigir rapidamente problemas. Para as equipas de programador, Retrace seja uma potência de super-obrigatórios de desempenho de aplicação totalmente integrada, ambiente de várias. Combina a várias ferramentas que tem de cada equipa de desenvolvimento.

Retrace é a ferramenta de apenas que fornece todas as seguintes capacidades em todos os ambientes de uma plataforma única.

* Gestão de desempenho de aplicações (APM)
* Registo de servidor e aplicação
* Monitorização e controlo de erro
* Servidor, a aplicação e métricas personalizadas

**Stackify prestes a extensão de agente do Linux**

Esta extensão fornece um caminho de instalação para o agente Linux para Retrace. 

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo 
O agente de Retrace pode ser executado relativamente a estes distribuições do Linux

| Distribuição | Versão |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS, 16.10 e 17.04 |
| Debian | 7.9 + e 8.2 +, 9 |
| RedHat | 6.7 +, 7.1 + |
| CentOS | 6.3 +, 7.0 + |

### <a name="internet-connectivity"></a>Conectividade Internet
A extensão de Stackify agente para Linux requer que a máquina virtual de destino está ligada à internet. 

Terá de ajustar a configuração de rede para permitir ligações a Stackify, consulte https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewall. 


## <a name="extension-schema"></a>Esquema de extensão
---

O JSON seguinte mostra o esquema para a extensão de Stackify agente Retrace. A extensão requer o `environment` e `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

## <a name="template-deployment"></a>Implementação de modelos 

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo Azure Resource Manager para executar a extensão de Stackify agente Linux Retrace durante uma implementação de modelo Azure Resource Manager.  

O JSON para uma extensão da máquina virtual pode ser aninhado o recurso de máquina virtual ou colocado no nível superior de um modelo do Resource Manager JSON ou raiz. O posicionamento de JSON afeta o valor do nome do recurso e do tipo. Para obter mais informações, consulte o nome do conjunto e tipo para recursos subordinados.

O exemplo seguinte assume que a extensão de Stackify Retrace Linux é aninhada o recurso de máquina virtual. Quando o recurso de extensão de aninhamento, JSON é colocado em "recursos": objeto [] da máquina virtual.

A extensão requer o `environment` e `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

Quando colocar a extensão JSON na raiz do modelo, o nome do recurso inclui uma referência à máquina virtual principal e o tipo de reflete a configuração aninhada.

```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "<parentVmResource>/StackifyExtension",
        "apiVersion": "[variables('apiVersion')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Stackify.LinuxAgent.Extension",
            "type": "StackifyLinuxAgentExtension",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "environment": "myEnvironment"
            },
            "protectedSettings": {
              "activationKey": "myActivationKey"
            }
        }
    }
```


## <a name="powershell-deployment"></a>Implementação de PowerShell

O `Set-AzureRmVMExtension` comando pode ser utilizado para implementar a extensão da máquina virtual Stackify agente Linux Retrace para uma máquina virtual existente. Antes de executar o comando, as configurações de públicas e privadas tem de ser armazenados numa tabela hash do PowerShell.

A extensão requer o `environment` e `activationKey`.

```
$PublicSettings = @{"environment" = "myEnvironment"}
$ProtectedSettings = @{"activationKey" = "myActivationKey"}

Set-AzureRmVMExtension -ExtensionName "Stackify.LinuxAgent.Extension" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Stackify.LinuxAgent.Extension" `
    -ExtensionType "StackifyLinuxAgentExtension" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
```

## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure 

A ferramenta da CLI do Azure pode ser utilizada para implementar a extensão da máquina virtual Stackify agente Linux Retrace para uma máquina virtual existente.  

A extensão requer o `environment` e `activationKey`.

```azurecli
az vm extension set --publisher 'Stackify.LinuxAgent.Extension' --version 1.0 --name 'StackifyLinuxAgentExtension' --protected-settings '{"activationKey":"myActivationKey"}' --settings '{"environment":"myEnvironment"}'  --resource-group 'myResourceGroup' --vm-name 'myVmName'
```

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="error-codes"></a>Códigos de erro

| Código de erro | Significado | Ação possíveis |
| :---: | --- | --- |
| 10 | Instalar o erro | é necessário o wget |
| 20 | Instalar o erro | é necessário o Python |
| 30 | Instalar o erro | é necessário o sudo |
| 40 | Instalar o erro | é necessário o activationKey |
| 51 | Instalar o erro | Distro de SO não suportada |
| 60 | Instalar o erro | ambiente é necessário |
| 70 | Instalar o erro | Desconhecidos |
| 80 | Ativar o erro | Falha na configuração de serviço |
| 90 | Ativar o erro | Arranque do serviço falhou |
| 100 | Desativar o erro | Paragem do serviço falhou |
| 110 | Desativar o erro | Falha na remoção do serviço |
| 120 | Erro de desinstalação | Paragem do serviço falhou |

Se precisar de mais ajuda pode contactar o suporte de Stackify em https://support.stackify.com.