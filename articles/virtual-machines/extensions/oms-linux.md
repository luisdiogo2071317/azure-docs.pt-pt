---
title: Extensão da máquina virtual do Azure do OMS para Linux | Microsoft Docs
description: Implemente o agente do OMS na máquina de virtual com Linux utilizando uma extensão da máquina virtual.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/21/2018
ms.author: danis
ms.openlocfilehash: f0d8224e5578a5ae46245e6c70792e962a44c933
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652860"
---
# <a name="oms-virtual-machine-extension-for-linux"></a>Extensão da máquina virtual OMS para Linux

## <a name="overview"></a>Descrição geral

Análise de registos fornece capacidades de remediação de monitorização, alertas e alertas na nuvem e no local ativos. A extensão de máquina virtual do agente do OMS de Linux está publicada e suportada pela Microsoft. A extensão instala o agente do OMS em máquinas virtuais do Azure e inscreve máquinas virtuais para uma área de trabalho de análise de registos existente. Este documento fornece detalhes sobre as plataformas suportadas, configurações e opções de implementação para a extensão de máquina virtual do OMS Linux.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo

A extensão de agente do OMS pode ser executada em relação a estas distribuições do Linux.

| Distribuição | Versão |
|---|---|
| CentOS Linux | 5, 6 e 7 (x86/x64) |
| Oracle Linux | 5, 6 e 7 (x86/x64) |
| Red Hat Enterprise Linux Server | 5, 6 e 7 (x86/x64) |
| Debian GNU/Linux | 6, 7, 8 e 9 (x86/x64) |
| Ubuntu | 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64) |
| SUSE Linux Enterprise Server | 11 e 12 (x86/x64) |

### <a name="agent-and-vm-extension-version"></a>Versão do agente e extensão de VM
A tabela seguinte fornece um mapeamento da versão da OMS VM extensão e pacote de agente do OMS para cada versão. Uma ligação para as notas de versão para a versão de pacote de agente do OMS está incluída. Notas de versão incluem detalhes sobre correções e novas funcionalidades disponíveis para uma versão do agente especificado.  

| Versão da extensão de VM de Linux do OMS | Versão do pacote de agente do OMS | 
|--------------------------------|--------------------------|
| 1.6.42.0 | [1.6.0-42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2-125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### <a name="azure-security-center"></a>Centro de Segurança do Azure

Centro de segurança do Azure Aprovisiona o agente do OMS e liga a uma área de trabalho de análise de registos predefinida criada pelo ASC na sua subscrição do Azure automaticamente. Se estiver a utilizar o Centro de segurança do Azure, não execute os passos neste documento. Se o fizer, substitui a área de trabalho configurada e interrompe a ligação com o Centro de segurança do Azure.

### <a name="internet-connectivity"></a>Conectividade Internet

A extensão de agente do OMS para Linux requer que a máquina virtual de destino está ligada à internet. 

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a extensão de agente do OMS. A extensão requer o ID da área de trabalho e a chave de área de trabalho a partir da área de trabalho de análise de registos do destino; Estes valores podem ser [encontrado na sua área de trabalho de análise de registos](../../log-analytics/log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) no portal do Azure. Porque a chave da área de trabalho deve ser tratada como dados confidenciais, devem ser armazenado numa configuração de definição protegido. Dados da definição de extensão protegido de VM do Azure é encriptados e desencriptados apenas na máquina virtual de destino. Tenha em atenção que **workspaceId** e **workspaceKey** diferenciam maiúsculas de minúsculas.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Fabricante | Microsoft.EnterpriseCloud.Monitoring |
| tipo | OmsAgentForLinux |
| typeHandlerVersion | 1.6 |
| workspaceId (por exemplo) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (por exemplo) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. Os modelos são ideais quando implementar um ou mais máquinas virtuais que necessitam de configuração de implementação de post, como de integração para análise de registos. Um modelo de Gestor de recursos de exemplo que inclui a extensão de VM de agente do OMS pode ser encontrado no [Galeria de início rápido de Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

A configuração de JSON para uma extensão da máquina virtual pode ser aninhada o recurso de máquina virtual ou colocada no nível superior de um modelo do Resource Manager JSON de raiz ou. A colocação da configuração JSON afeta o valor do nome do recurso e do tipo. Para obter mais informações, consulte [definir nome e tipo para recursos subordinados](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

O exemplo seguinte assume que a extensão da VM é aninhada o recurso de máquina virtual. Quando o recurso de extensão de aninhamento, JSON é colocado no `"resources": []` objeto da máquina virtual.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Quando colocar a extensão JSON na raiz do modelo, o nome do recurso inclui uma referência à máquina virtual principal e o tipo de reflete a configuração aninhada.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure

A CLI do Azure pode ser utilizada para implementar a extensão de VM de agente do OMS para uma máquina virtual existente. Substitua o *workspaceId* e *workspaceKey* com os da sua área de trabalho de análise de registos. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.6 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando a CLI do Azure. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando utilizando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Resultado da execução de extensão é registado para o ficheiro seguinte:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Códigos de erro e os respetivos significados

| Código de Erro | Significado | Ação possíveis |
| :---: | --- | --- |
| 9 | Ativar chamado prematuramente | [Atualizar o agente Linux do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) para a versão mais recente disponível. |
| 10 | VM já está ligada a uma área de trabalho de análise de registos | Para ligar a VM para a área de trabalho especificada no esquema de extensão, defina stopOnMultipleConnections como false nas definições públicas ou remova esta propriedade. Esta VM obtém cobrada depois de cada área de trabalho está ligado a um. |
| 11 | Configuração inválida fornecida para a extensão | Siga os exemplos anteriores para definir todos os valores de propriedade necessários para a implementação. |
| 12 | O Gestor de pacotes dpkg está bloqueado | Certifique-se de que todos os dpkg operações de atualização na máquina tiver concluído e tente novamente. |
| 17 | Falha de instalação de pacote do OMS | 
| 19 | Falha de instalação do pacote OMI | 
| 20 | Falha de instalação do pacote SCX |
| 51 | Esta extensão não é suportada no sistema de operação da VM | |
| 55 | Não é possível ligar ao serviço do Microsoft Operations Management Suite | Verifique se o sistema tem acesso à Internet ou que foi fornecido um proxy HTTP válido. Além disso, verifique a correção do ID de área de trabalho. |

Informações adicionais de resolução de problemas podem ser encontradas no [guia de resolução de problemas do OMS agente para Linux](../../log-analytics/log-analytics-azure-vmext-troubleshoot.md).

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
